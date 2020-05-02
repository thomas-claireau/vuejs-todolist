<template>
	<div id="app">
		<Todos></Todos>
	</div>
</template>

<script>
import Vue from 'vue';
import Todos from './components/Todos';

export default {
	name: 'App',
	components: {
		Todos,
	},
};
</script>

<style scoped>
</style>
