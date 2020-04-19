<template>
	<section class="todoapp">
		<header class="header" data-children-count="1">
			<h1>todos</h1>
			<input
				autofocus="autofocus"
				autocomplete="off"
				placeholder="What needs to be done?"
				class="new-todo"
				v-model="writeTodo"
				@keypress.enter="addTodo()"
			/>
		</header>
		<section class="main" v-if="todos.length">
			<input id="toggle-all" type="checkbox" class="toggle-all" v-model="toggleCompleted" />
			<label for="toggle-all">Mark all as complete</label>
			<ul class="todo-list">
				<li class="todo" :class="editedTodo ? 'editing' : ''" v-for="todo in data" :key="todo.text">
					<div class="view">
						<input type="checkbox" class="toggle" v-model="todo.completed" />
						<label @dblclick="editTodo(todo)">{{todo.text}}</label>
						<button class="destroy" @click="deleteTodo(todo)"></button>
					</div>
					<input
						type="text"
						class="edit"
						v-todo-focus="todo == editedTodo"
						v-model.lazy="todo.text"
						@blur="doneEdit"
						@keyup.esc="doneEdit"
					/>
				</li>
			</ul>
		</section>
		<footer class="footer" v-if="todos.length">
			<span class="todo-count">
				<strong>{{ nbTodos }}</strong> items left
			</span>
			<ul class="filters">
				<li>
					<a href="#/all" :class="filtre == 'all' ? 'selected' : ''" @click="filteredTodo('all')">All</a>
				</li>
				<li>
					<a
						href="#/active"
						:class="filtre == 'active' ? 'selected' : ''"
						@click="filteredTodo('active')"
					>Active</a>
				</li>
				<li>
					<a
						href="#/completed"
						:class="filtre == 'completed' ? 'selected' : ''"
						@click="filteredTodo('completed')"
					>Completed</a>
				</li>
			</ul>
			<button class="clear-completed" v-if="isCompleted" @click="clearCompleted()">Clear completed</button>
		</footer>
	</section>
</template>

<script>
import Vue from 'vue';

export default {
	data() {
		return {
			writeTodo: '',
			todos: [],
			filtre: 'all',
			editedTodo: null,
		};
	},
	methods: {
		addTodo() {
			if (this.writeTodo == '' || this.isDuplicate()) return;

			const todo = {
				text: this.writeTodo,
				completed: false,
			};

			this.todos.push(todo);
			this.clearInput();
		},
		editTodo(todo) {
			this.editedTodo = todo;
		},
		doneEdit() {
			this.editedTodo = null;
		},
		deleteTodo(todo) {
			this.todos = this.todos.filter((item) => item != todo);
		},
		filteredTodo(filtre) {
			this.filtre = filtre;
		},
		toggleCompleted() {
			const array = this.todos.filter((item) => !item.completed);
			console.log(array);
		},
		clearCompleted() {
			this.todos = this.todos.filter((item) => !item.completed);
		},
		isDuplicate() {
			return this.todos.filter((item) => item.text == this.writeTodo).length > 0;
		},
		clearInput() {
			this.writeTodo = '';
		},
	},
	computed: {
		nbTodos() {
			return this.todos.filter((item) => !item.completed).length;
		},
		isCompleted() {
			return this.todos.filter((item) => item.completed).length;
		},
		data() {
			if (this.filtre == 'completed') {
				return this.todos.filter((item) => item.completed);
			} else if (this.filtre == 'active') {
				return this.todos.filter((item) => !item.completed);
			} else {
				return this.todos;
			}
		},
	},
	directives: {
		todoFocus(el, value) {
			Vue.nextTick((_) => {
				if (value) el.focus();
			});
		},
	},
};
</script>

<style src="./style.css">
</style>