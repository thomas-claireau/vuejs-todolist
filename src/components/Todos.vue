<template>
	<div class="todoapp-container">
		<header class="header">
			<img src="../assets/logo.png" alt />
			<h1>todos</h1>
		</header>
		<section class="todoapp">
			<input
				class="new-todo"
				placeholder="What needs to be done ?"
				v-model="newTodo"
				@keyup.enter="addTodo()"
			/>
			<div class="main">
				<input id="toggle-all" type="checkbox" class="toggle-all" v-model="allCompleted" />
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<li
						class="todo"
						:class="{ completed: todo.completed, editing: editedTodo == todo }"
						v-for="(todo, index) in filteredTodo"
						:key="index"
					>
						<div class="view">
							<input type="checkbox" class="toggle" v-model="todo.completed" />
							<label @dblclick="editTodo(todo)">{{todo.title}}</label>
							<button class="destroy" @click="deleteTodo(todo)"></button>
						</div>
						<input
							type="text"
							class="edit"
							ref="edit"
							v-model.lazy="todo.title"
							@blur="doneEdit(todo)"
							@keyup.enter="doneEdit(todo)"
							@keyup.esc="cancelEdit(todo)"
						/>
					</li>
				</ul>
			</div>
			<footer class="footer" v-show="todos.length > 0">
				<span class="todo-count">
					<strong>{{remaining}}</strong> items left
				</span>
				<ul class="filters">
					<li>
						<a href="#" :class="{ selected: visibility == 'all' }" @click="visibility = 'all'">All</a>
					</li>
					<li>
						<a
							href="#"
							:class="{ selected: visibility == 'active' }"
							@click="visibility = 'active'"
						>Active</a>
					</li>
					<li>
						<a
							href="#"
							:class="{ selected: visibility == 'completed' }"
							@click="visibility = 'completed'"
						>Completed</a>
					</li>
				</ul>
				<button class="clear-completed" v-show="isCompleted" @click="clearCompleted()">Clear completed</button>
			</footer>
		</section>
		<footer class="info">
			<p>Double-click to edit a todo</p>
			<p>
				Written by
				<a href="http://evanyou.me">Evan You</a>
			</p>
			<p>
				Part of
				<a href="http://todomvc.com">TodoMVC</a>
			</p>
		</footer>
	</div>
</template>

<script>
import Vue from 'vue';

const filters = {
	all(todos) {
		return todos;
	},
	active(todos) {
		return todos.filter((item) => !item.completed);
	},
	completed(todos) {
		return todos.filter((item) => item.completed);
	},
};

export default {
	data() {
		return {
			todos: [],
			newTodo: '',
			visibility: 'all',
			editedTodo: null,
		};
	},
	methods: {
		addTodo() {
			if (this.filteredTodo.filter((item) => item.title == this.newTodo).length > 0) return;

			if (this.newTodo !== '') {
				this.todos.push({
					completed: false,
					title: this.newTodo,
				});
			}

			this.newTodo = '';
		},
		editTodo(todo) {
			this.editedTodo = todo;

			this.$nextTick(() => {
				this.$refs.edit[0].focus();
			});
		},
		deleteTodo(todo) {
			this.todos = this.filteredTodo.filter((item) => item !== todo);
		},
		doneEdit(todo) {
			if (todo.title == '') {
				this.deleteTodo(todo);
			}

			this.editedTodo = null;
		},
		cancelEdit(todo) {
			const selectedTodo = this.todos.filter((item) => item == todo)[0];

			if (!selectedTodo) return;

			selectedTodo.title = this.editedTodo.title;
			this.editedTodo = null;
		},
		clearCompleted() {
			this.todos = this.todos.filter((item) => !item.completed);
		},
	},
	computed: {
		filteredTodo() {
			return filters[this.visibility](this.todos);
		},
		remaining() {
			return this.todos.filter((item) => !item.completed).length;
		},
		isCompleted() {
			return (
				this.filteredTodo.filter((item) => !item.completed).length !==
				this.filteredTodo.length
			);
		},
		allCompleted: {
			get() {
				const allCompleted =
					this.filteredTodo.filter((item) => !item.completed).length == 0;

				if (allCompleted && this.filteredTodo.length > 0) {
					return true;
				} else {
					return false;
				}
			},
			set(value) {
				this.filteredTodo.forEach((todo) => {
					todo.completed = value;
				});
			},
		},
	},
};
</script>

<style src="./style.scss" lang="scss">
</style>