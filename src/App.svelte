<script lang="ts">
	import { onMount } from 'svelte';

	const title: string = 'My tiny ToDo'
	let todos: string[] = []
	let todoText: string = ''
	let todoInput: HTMLInputElement | undefined = undefined

	onMount(() => {
		todos = loadTodos()
	})

	function addTodo(): void {
		todos = [ ...todos, todoText]
		saveTodos()
	}

	function removeTodo(todoToRemove): void {
		todos = todos.filter(m => m !== todoToRemove)
		saveTodos()
	}

	function saveTodos(): void {
		localStorage.setItem('todos', JSON.stringify(todos))
		todoText = ''
		todoInput?.focus()
	}

	function loadTodos(): string[] {
		const existingTodosString: string = localStorage.getItem('todos')
		return JSON.parse(existingTodosString) || []
	}
</script>

<main>
	<h1>{title}</h1>
	<ul>
		{#each todos as todo}
			<li>
				{todo} <input type="button" value="❌" on:click={() => removeTodo(todo)}>
			</li>
		{/each}
	</ul>
	<form on:submit|preventDefault={addTodo}>
		<input bind:value={todoText} bind:this={todoInput}>
		<input type="submit" value="➕">
	</form>
</main>

