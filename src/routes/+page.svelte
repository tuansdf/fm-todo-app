<script lang="ts">
	import { nanoid } from 'nanoid';
	import { fade, slide } from 'svelte/transition';

	type Todo = {
		id: string;
		text: string;
		done: boolean;
	};

	let todos: Todo[] = [
		{
			id: nanoid(),
			text: 'first todo',
			done: false
		},
		{
			id: nanoid(),
			text: 'second todo',
			done: false
		},
		{
			id: nanoid(),
			text: 'third todo',
			done: false
		}
	];
	let createDone: boolean = false;
	let createText: string = '';

	$: console.log(todos);

	const addTodo = (todo: Todo) => {
		todos = [...todos, todo];
	};
	const deleteTodo = (id: string) => {
		todos = todos.filter((todo) => todo.id !== id);
	};
	const handleSubmit = () => {
		if (!createText) return;
		const newTodo: Todo = {
			id: nanoid(),
			text: createText,
			done: createDone
		};
		addTodo(newTodo);
		resetForm();
	};
	const resetForm = () => {
		createText = '';
		createDone = false;
	};
</script>

<img src="/bg-desktop-dark.jpg" alt="background" />

<main class="main">
	<!-- header -->
	<header class="header">
		<h1 class="heading">TODO</h1>
		<button class="change-theme-button">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z"
				/>
			</svg>
		</button>
	</header>

	<!-- create new -->
	<form on:submit|preventDefault={handleSubmit} class="create-todo">
		<input type="checkbox" bind:checked={createDone} />
		<input type="text" bind:value={createText} placeholder="Add a todo" />
	</form>

	<!-- todos -->
	<ul class="todo-list">
		{#each todos as todo (todo.id)}
			<li in:slide out:fade class="todo-item" class:is-done={todo.done}>
				<input id={todo.id} type="checkbox" bind:checked={todo.done} class="todo-checkbox" />
				<label for={todo.id} class="todo-text">{todo.text}</label>
				<button type="button" on:click={() => deleteTodo(todo.id)} class="delete-button">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke-width="1.5"
						stroke="currentColor"
					>
						<path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
					</svg>
				</button>
			</li>
		{/each}
	</ul>
</main>

<style>
	:global(body) {
		background-color: black;
	}
	button,
	input[type='text'] {
		border: none;

		background-color: transparent;
	}
	input[type='text'] {
		outline: none;
	}
	button {
		padding: 0.25rem;

		border-radius: 9999px;

		transition: background-color 200ms ease;

		cursor: pointer;
	}
	button:hover {
		background-color: gray;
	}

	img {
		position: fixed;
	}

	.header {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.heading {
		font-size: 3rem;
		color: white;
		letter-spacing: 1rem;
	}
	.change-theme-button > svg {
		--size: 1.75rem;
		width: var(--size);
		height: var(--size);

		color: white;
	}

	.main {
		position: absolute;
		top: 10rem;
		left: 50%;
		translate: -50% 0;

		height: 100px;
		max-width: 50rem;
		width: 100%;
	}
	.main > * + * {
		margin-top: 1.5rem;
	}

	.create-todo {
		width: 100%;

		display: flex;
		align-items: center;
		gap: 1rem;

		padding: 1.25rem;

		border-radius: 0.2rem;

		background-color: black;
	}
	.create-todo > [type='text'] {
		width: 100%;

		color: white;
		font-size: 1rem;
	}

	.todo-list {
		width: 100%;

		border-radius: 0.2rem;

		background-color: black;

		color: white;
	}
	.todo-item + .todo-item {
		border-top: 1px solid white;
	}
	.todo-item {
		display: flex;
		align-items: center;
		gap: 1rem;

		padding: 0.5rem 1.25rem;
	}
	.todo-item.is-done > .todo-text {
		color: gray;
		text-decoration: line-through;
	}

	.delete-button {
		margin-left: auto;
	}
	.delete-button > svg {
		--size: 1.75rem;
		width: var(--size);
		height: var(--size);

		color: white;
	}
</style>
