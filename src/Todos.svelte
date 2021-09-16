<!-- Types -->

<script lang="ts" context="module">
	type ITodos = ITodo[];
</script>

<!-- Logic -->

<script lang="ts">
	import Todo from './Todo.svelte'
	import type { ITodo } from './Todo.svelte'

	async function fetchTodos(): Promise<ITodo[]>{
		const res = await fetch('https://jsonplaceholder.typicode.com/todos')
		if (!res.ok) throw new Error()
		return await res.json()
	}
</script>

<!-- Markup -->

{#await fetchTodos()}
	<p>Loading</p>
{:then todos}
	<ul>
		{#each todos as todo}
			<Todo {todo}/>
		{/each}
	</ul>
{:catch error}
	<p>Error</p>
{/await}

<!-- Styles -->

<style>
	ul {
		padding: 0;
		margin: 0;
	}
</style>