<script>
	import { writable } from 'svelte/store'
	
	let inputValue
	function addTodo () {
		if (inputValue) {
			todosArr = [...todosArr, {id: Math.random() * 100, text: inputValue, 
			done: false}]
			localStorage.setItem('todos', JSON.stringify(todosArr))
			inputValue = ''
		}
	}
	function changeState () {
		localStorage.setItem('todos', JSON.stringify(todosArr))
	}
	function deleteDoneTodos () {
		const filteredArr = todosArr.filter(i => i.done === false)
		todosArr = filteredArr
		localStorage.setItem('todos', JSON.stringify(todosArr))
	}

	const stored = localStorage.todos

	let todosArr = stored ? JSON.parse(stored) : [{id: Math.random() * 100, text: 'Заценить мою приложушку', done: false}, {id: Math.random() * 100, text: 'Написать мне, какой я молодец', done: false}]

	const todos = writable(stored || JSON.stringify([{id: Math.random() * 100, text: 'Заценить мою приложушку', done: false}, {id: Math.random() * 100, text: 'Написать мне, какой я молодец', done: false}]))
	
	todos.subscribe((value) => {
		localStorage.todos = value
	})

</script>

<main>
	<div class="todo">
		<header class="todo__add">
			<div class="container">
				<textarea placeholder="Введите тудушку" bind:value={inputValue}/>
				<button on:click={addTodo}>Закиньте тудушку</button>
			</div>
		</header>
		<ul class="container">
			{#each todosArr as {id, text, done} (id)}
			<li class="todo__item">
				<input type="checkbox" bind:checked={done} on:change={changeState}/>
				{text}	
			</li> 
			{/each}
		</ul>
		<footer>
			<div class="container">
			<button on:click={deleteDoneTodos}>
				Удалите выполненные
			</button>
		</div>
		</footer>
	</div>
</main>

<style>
	:global(body) {
		background-color: #000;
        background-image: url("https://lifeonphoto.com/img/images/2021/09/26/image001-38-1280x720.jpg");
		backdrop-filter: blur(12px) brightness(.8);
		overflow: hidden;
		font-family: 'Roboto', sans-serif;
		font-size: 16px;
    }
	
	main {
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #fff;
		padding: 15px;
	}
	.container {
		text-align: center;
		padding: 15px;
		display: flex;
		align-items: center;
		flex-direction: column;
		overflow: hidden;
	}
	.todo {
		display: flex;
		flex-direction: column;
		background-color: #25273C;
		width: min(600px, 100%);
		height: 550px;
		border-radius: 20px;
		box-shadow: 3px -3px 10px 1px #000, -3px 3px 10px 1px #000, 3px 3px 10px 1px #000, -3px -3px 10px 1px #000;
		overflow: auto;
	}
	.todo__add {
		border-bottom: 3px solid rgb(50, 54, 75);
	}
	textarea {
		width: 80%;
		font-size: 16px;
		color: #fff;
		background-color: #25273C;
		text-align: center;
		resize: none;
		margin-bottom: 5px;
	}
	textarea::placeholder{
		text-align: center;
	}
	button {
		background-color: #1f2030;
		padding: 10px 30px;
		border-radius: 30px;
		color: #fff;
	}
	button:hover{
		background-color: #2c2d46;
	}
	ul {
		flex: 1;
	}
	li {
		max-width: 90%;
		margin: 10px 0;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}
	footer {
		border-top: 3px solid rgb(50, 54, 75);
	}

</style>