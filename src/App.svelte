<script>
	import { onMount } from "svelte";

	// Variable to hold todos
	let todos = [];

	// Base Url
	const baseUrl = "http://localhost:3000/todos";

	// Get todos from backend
	const getTodos = async () => {
		const response = await fetch(baseUrl);
		const data = await response.json();
		todos = await data;
	};

	// I believe this runs when the component loads like React.useEffect
	onMount(() => {
		getTodos();
	});
</script>

<style>
</style>

<main>
	<h1>The Svelte To Do App</h1>
	{#each todos as todo}
		<div>
			<h2>{todo.title}</h2>
			<h2>{todos.body}</h2>
		</div>
	{/each}
</main>
