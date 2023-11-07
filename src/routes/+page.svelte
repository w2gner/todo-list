<script>
  let todos = [];
  let newTodo = '';

	if (typeof window !== 'undefined') {
		// Code runs in the browser
		todos = JSON.parse(localStorage.getItem('todos')) || [];
	}

	// Save todos to local storage
	function saveTodos() {
		localStorage.setItem('todos', JSON.stringify(todos));
	}

	// Updated addTodo function with saveTodos call
	function addTodo() {
		if (newTodo.trim()) {
			todos = [...todos, { id: Date.now(), text: newTodo, done: false }];
			newTodo = '';
			saveTodos(); // Save todos when a new one is added
		}
	}

	// Updated toggleDone function with saveTodos call
	function toggleDone(todo) {
		todo.done = !todo.done;
		todos = todos.map((t) => (t.id === todo.id ? todo : t));
		saveTodos(); // Save todos when one is toggled
	}

	// Updated deleteTodo function with saveTodos call
	function deleteTodo(id) {
		todos = todos.filter((t) => t.id !== id);
		saveTodos(); // Save todos when one is deleted
	}
</script>

<input type="text" bind:value={newTodo} placeholder="What needs to be done?" />
<button on:click={addTodo}>Add</button>

{#if todos.length > 0}
	<ul>
		{#each todos as todo}
			<li class:done={todo.done}>
				<input type="checkbox" bind:checked={todo.done} on:change={() => toggleDone(todo)} />
				<span class:done={todo.done}>{todo.text}</span> <!-- Apply strikethrough style -->
				<button on:click={() => deleteTodo(todo.id)}>Delete</button>
			</li>
		{/each}
	</ul>
{:else}
	<p>Your to-do list is empty.</p>
{/if}

<style>
  .done {
		text-decoration: line-through; /* Apply strikethrough style */
	}
	body {
		font-family: 'Arial', sans-serif;
		background-color: #f3f4f6;
		display: flex;
		justify-content: center;
		padding-top: 50px;
	}

	input[type='text'] {
		padding: 10px;
		border: none;
		border-radius: 4px;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
		margin-right: 5px;
		width: 300px;
	}

	button {
		background-color: #5cb85c;
		color: white;
		border: none;
		padding: 10px 20px;
		margin: 0 5px;
		border-radius: 4px;
		cursor: pointer;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
		transition: background-color 0.3s;
	}

	button:hover {
		background-color: #4cae4c;
	}

	ul {
		list-style: none;
		padding: 0;
	}

	li {
		background-color: white;
		margin: 10px 0;
		padding: 10px;
		border-radius: 4px;
		display: flex;
		align-items: center;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	li.done {
		text-decoration: line-through;
		color: #d3d3d3;
	}

	input[type='checkbox'] {
		margin-right: 10px;
	}

	p {
		color: #888;
	}
</style>
