<!-- Types -->
<script lang="ts" context="module">
  import type { ITodo } from './Todo.svelte'
</script>

<!-- Logic -->
<script lang="ts">
  import Todo from './Todo.svelte'
  import { onMount } from 'svelte'

  let todos: ITodo[] | undefined

  // Handle getting the todos from localStorage
  function getTodos() {
    const stringTodos = localStorage.getItem('todos')
    todos = stringTodos ? JSON.parse(stringTodos) : []
  }

  // Handle adding a todo
  function add() {
    const newTodo = prompt('Add a todo')
    if (!newTodo || !todos) return
    todos = [...todos, { id: Math.random(), title: newTodo, completed: false }]
  }

  // Handle removing a todo title
  function remove(event: CustomEvent<{ todo: ITodo }>) {
    if (!todos) return
    todos = todos.filter(curr => event.detail.todo.id !== curr.id)
  }

  // Handle toggling a todo's completed status
  function toggle(event: CustomEvent<{ todo: ITodo }>) {
    if (!todos) return
    todos = todos.map(curr => {
      if (curr.id === event.detail.todo.id) curr.completed = !curr.completed
      return curr
    })
  }

  // Handle editing a todo title
  function edit(event: CustomEvent<{ todo: ITodo }>) {
    const updated = prompt('Edit todo', event.detail.todo.title)
    if (!updated || !todos) return
    todos = todos.map(curr => {
      if (curr.id === event.detail.todo.id) curr.title = updated
      return curr
    })
  }

  // Update localStorage whenever the `todos` value changes
  $: {
    if (todos) localStorage.setItem('todos', JSON.stringify(todos))
  }

  // Get the todos on mount
  onMount(getTodos)
</script>

<!-- Markup -->

{#if todos}
  <button on:click={add}>Add todo</button>
  {#if todos.length === 0}
    <p>No todos</p>
  {:else}
    <ul>
      {#each todos as todo (todo.id)}
        <Todo on:remove={remove} on:edit={edit} on:toggle={toggle} {todo} />
      {/each}
    </ul>
  {/if}
{/if}

<!-- Styles -->
<style>
  ul {
    padding: 0;
    margin: 0;
  }
</style>
