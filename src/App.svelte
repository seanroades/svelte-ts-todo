<script lang="ts">
	import Title from './Title.svelte';
	import Subtitle from './Subtitle.svelte';
	import Copyright from './Copyright.svelte';
	import Doge from './Doge.svelte';
	let todos: string[] = [];
	let task: string = "";
	let filter: string[] = "all";
	function addTask() { // ReactiveDeclaration, Life Cycle
		if (task.length > 50) {
			alert("Your task cannot be greater than 50 characters long"); // Printing
			task = ""
			return;
		}
		if (task.length >= 2) {
			todos = [{
				task: task,
				status: 'pending'
			}, ...todos];
			task = "";
		}
		else {
			alert('Your task must be at least 2 characters long') // Printing
		}
	}
	function markComplete(i) { // ReactiveDeclaration, Life Cycle
		todos[i].status = "completed";
		todos = [...todos];
	}
	function removeTask(i) { // ReactiveDeclaration, Life Cycle
		todos.splice(i, 1);
		todos = [...todos];
	}
	let referrer = document.referrer;

	var level = "unknown"

	navigator.getBattery().then(function(battery) {
		level = battery.level * 100;
	});
</script>

<style>
	* {
		margin: 0px;
		padding: 0px
	}
	.container { /**Class*/
		width: 100%;
		height: 100vh;
		background: white;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}
	.todo { /**Class*/
		padding: 20px;
		background: #f5f5f5;
		width: 500px;
		border-radius: 20px;
	}
	.todo > div { /**Class*/
		margin: 20px 0px;
	}
	.todo .form{
		display: flex; /* LocalStyle */
	}
	.todo .form input,
	.todo .form button {
		border: 1px solid #4db848;
		height: 40px;
		outline: none;
		border-radius: 20px; /* LocalStyle */
	}
	.todo .form input {
		flex: 1;
		text-indent: 20px; /* LocalStyle */
	}
	.todo .form button {
		width: 60px;
		margin-left: 5px;
		color: #4db848;
		cursor: pointer;
	}
	.todo .tasks {
		min-height:100px;
	}
	.todo .tasks > .task {
		margin: 10px 0px;
		display: flex;
	}
	.todo .tasks > .task > div {
		flex:1;
	}
	.todo .tasks > .task > button {
		width: 25px;
		height: 25px;
		border: 1px solid #4db848;
		color: #4db848;
		border-radius: 50%;
		margin: 0px 5px;
		cursor: pointer;
	}
	.todo .tasks > .task > button.active{
		background: #4db848;
		color: white;
	}
	.todo .filters {
		display: flex;
		justify-content: space-between;
	}
	.todo .filters > button {
		padding:10px 8px;
		border:1px solid #4db848;
		color: #4db848;
		border-radius: 20px;
		cursor: pointer;
		min-width: 100px;
	}
	.todo .filters > button.active {
		background: #4db848;
		color: white;
		font-weight: bold;
	}
	.button {
		min-width: 100px;
	}
	.buttonLong {
		min-width: 200px;
	}
</style>

<div class="container">
	<Doge />
	<Title title="CS178 ToDOGE List" /> <!--Component Prop -->
	<Subtitle subtitle="By Sean Roades" /> <!--Component Prop -->
	<div class="todo">
		<div class = "form">
			<input type="text" bind:value={task} /> <!--Properties--> <!--Binding-->
			<button class="buttonLong" on:click={addTask}>Add {todos.length > 0  ? "another" : 'your first'} ToDOGE</button> <!--Properties Reactivity-->
		</div>
		<div class="tasks">
			{#each todos as todo, i} <!--controlFlow template-->
				{#if filter === 'all'} <!--controlFlow Nesting-->
					<div class="task">
						<div>{todo.task}</div> <!--format stateful text-exp-->
						<button 
							class="{(todo.status === 'completed') ? 'active' : ''}" 
							on:click={() => {markComplete(i)}}> <!--Attributes-->
							&#10004;
						</button> <!--controlFlow Properties Button-->
						<button on:click={() => {removeTask(i)}}>&#10006</button> <!--Attributes Button-->
					</div>
				{:else if filter === 'completed'} <!--controlFlow template-->
					{#if todo.status === 'completed'} <!--controlFlow Nesting-->
						<div class="task"> 
							<div>{todo.task}</div> <!--format stateful-->
							<button on:click={() => removeTask(i)}> <!--Attributes-->
								&#10006
							</button> <!--controlFlow Button-->
						</div>
					{/if} <!--controlFlow-->
				{:else} <!--controlFlow template-->
					{#if todo.status === 'pending'} <!--controlFlow Nesting-->
						<div class="task">
							<div>{todo.task}</div> <!--format stateful-->
							<button 
								class="{todo.status=='completed' ? 'active' : ''}"
								on:click={() => (markComplete(i))}> <!--Attributes-->
							&#10004
							</button> <!--controlFlow Properties Button-->
						</div>
					{/if} <!--controlFlow-->
				{/if} <!--controlFlow-->
			{/each} <!--controlFlow-->
		</div>
		<div class="filters">
			<button 
				class="{filter === 'all' ? 'active' : 'button'}" 
				on:click={() => {filter = 'all'}}>
				All ({todos.length}) <!--Reactivity-->
			</button> <!--controlFlow Properties Button-->
			<button 
				class="{filter === 'completed' ? 'active' : 'button'}" 
				on:click={() => {filter = 'completed'}}> 
				Completed
			</button> <!--controlFlow Properties Button Attributes-->
			<button class="{filter === 'incomplete' ? 'active' : 'button'}" on:click={() => {filter = 'incomplete'}}> <!--Reactivity-->
				Incomplete
			</button> <!--controlFlow Properties Button Attributes-->
		</div>
	</div>
	<br />
	<p style="width: 500px; text-align: center">By the way, it's great to have you visiting from {referrer}, {level < 30 ? "but you might want to stop visiting them and focus on your todo as your battery is currently at" : "and you have plently of time to spent on your todo list as your battery is at"} {level}%</p>
	<Copyright disclaimer="All rights reserved" name="copyrightsAreLegitIfTheyExistOnAWebsite©"/> <!--Component Props-->
</div>