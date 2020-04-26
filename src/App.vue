<template>
	<div id="app">
		<Todos></Todos>
	</div>
</template>

<script>
import Todos from './components/Todos';
import Vue from 'vue';

export default {
	name: 'App',
	components: {
		Todos,
	},
};
</script>

<style scoped>
</style>
