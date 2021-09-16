<!-- Types -->

<script lang="ts" context="module">
    export type ITodo = {
		title: string
		completed: boolean
	};
</script>

<!-- Logic -->

<script lang="ts">
    export let todo: ITodo
</script>

<!-- Markup -->

<li class:completed={todo.completed}>{todo.title} <input type="checkbox" bind:checked={todo.completed} /></li>

<!-- Styles -->

<style>
    li {
        padding: 1rem;
    }
    li.completed {
       text-decoration: line-through;
    }
    li:not(:last-child){
        border-bottom: 1px solid #efefef;
    }
</style>