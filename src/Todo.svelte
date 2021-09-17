<script lang="ts" context="module">
  export type ITodo = {
    id: number
    title: string
    completed: boolean
  }
  type DispatchActions = Record<'remove' | 'toggle' | 'edit', { todo: ITodo }>
</script>

<script lang="ts">
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher<DispatchActions>()
  export let todo: ITodo
</script>

<!-- Markup -->

<li class:completed={todo.completed}>
  <div>
    <input
      type="checkbox"
      on:change={() => dispatch('toggle', { todo })}
      checked={todo.completed}
    />
    <span>{todo.title}</span>
  </div>
  <div>
    <button on:click={() => dispatch('edit', { todo })}>Edit</button>
    <button on:click={() => dispatch('remove', { todo })}>Remove</button>
  </div>
</li>

<!-- Styles -->
<style>
  div {
    display: inherit;
    align-items: inherit;
    gap: inherit;
  }
  li {
    padding: 1rem 0;
    display: flex;
    gap: 1rem;
    justify-content: space-between;
    align-items: center;
  }

  input {
    margin: 0;
  }

  li.completed span {
    text-decoration: line-through;
  }
  li:not(:last-child) {
    border-bottom: 1px solid #efefef;
  }
  button {
    margin: 0;
  }
</style>
