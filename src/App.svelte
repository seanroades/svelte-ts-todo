<script lang="ts">
	import Copyright from './Copyright.svelte';
	import Doge from './Doge.svelte';
  import Navbar from './Navbar.svelte';
  import DogeFact from './DogeFact.svelte';
	import AlertPopup from './Alert.svelte';
  import DogeChat from './DogeChat.svelte';
  import CreateNewList from './CreateNewList.svelte';
  import AccessOldList from './AccessOldList.svelte';

	let todos = [];
	let task: string = "";
	let filter: string = "all";

	const baseURL = "https://todogeapi.herokuapp.com";
	console.log("baseurl:", baseURL)

	async function modifyList(list) {
		try {
			const returnValue = await fetch(`${baseURL}/api/modifyList`, 
				{
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						"name": listCurrName,
						"list": list
					})
				});
				const response = await returnValue.json();
				var data = response.data;
			} catch (error) {
				console.log(error);
			}
	}
	async function addTask() { // ReactiveDeclaration, Life Cycle
		if (task.length > 50) {
			alert("Your task cannot be greater than 50 characters long"); // Printing
			task = ""
			return;
		}
		else if (task.length >= 2) {
			todos = [{
				task: task,
				status: 'pending'
			}, ...todos];
			task = "";
			await modifyList(todos);
		}
		else {
			alert('Your task must be at least 2 characters long') // Printing
		}
	}
	async function markComplete(i) { // ReactiveDeclaration, Life Cycle
		todos[i].status = "completed";
		todos = [...todos];
		await modifyList(todos);
	}
	async function removeTask(i) { // ReactiveDeclaration, Life Cycle
		todos.splice(i, 1);
		todos = [...todos];
		await modifyList(todos);
	}
	let referrer = "";
	if (document.referrer){
		referrer = document.referrer;
	}

	var level = "unknown"

	let width = window.innerWidth;

	if (typeof navigator.getBattery !== 'undefined') {
		navigator.getBattery().then(function(battery) {
			level = (battery.level * 100).toFixed(0).toString();
		});
	}
	function closeAlertPopup () {
		popupVisible = false;
		revengeDoge();
	}
	function openAlertPopup () {
		popupVisible = true;
	}
	var popupVisible = false;

	// Big Doge Logic
	let dogeClass = "corner0";
	function sleep(ms) {
  	return new Promise(resolve => setTimeout(resolve, ms));
	}
	async function revengeDoge() {
		await sleep(5000).then(() => {
			dogeClass = "corner1";
		});
		await sleep(3000).then(() => {
			dogeClass = "corner2";
		});
		await sleep(2000).then(() => {
			dogeClass = "corner3";
		});
		await sleep(1000).then(() => {
			dogeClass = "corner4";
		});
		await sleep(1000).then(() => {
			dogeClass = "corner5";
		});
		await sleep(1000).then(() => {
			dogeClass = "corner1";
			openChat();
		});
	}
	let chatVisible = false;
	let ipdata = {
		ip: "",
		city: "",
		state_prov: "",
		country_name: "",
		time: "",
		isp: "",
		lat: "",
		long: ""
	};
	async function openChat() {
		ipdata = ipdata;
		// await getIPData();
		chatVisible = true;
	}
	function closeChat() {
		chatVisible = false;
	}
	/*Usage example*/
	async function getIPData() {
		let apiKey = '9dae6884af134720bc23d94f24980a75';
		// Make the request
		fetch('https://api.ipgeolocation.io/ipgeo?apiKey=9dae6884af134720bc23d94f24980a75')
			// Extract JSON body content from HTTP response
			.then(response => response.json())
			// Do something with the JSON data
			.then(data => {
				// console.log(JSON.stringify(data, null, 2)) 
				ipdata.ip = data.ip;
				ipdata.city = data.city;
				ipdata.state_prov = data.state_prov;
				ipdata.country_name = data.country_name;
				ipdata.isp = data.isp;
				ipdata.lat = data.latitude;
				ipdata.long = data.longitude;
			});
	}
	getIPData();

	function openListCreatorTool () {
		console.log('openListCreatorTool', listCreatorToolVisible);
		listCreatorToolVisible = true;
	}
	function closeListCreatorTool () {
		listCreatorToolVisible = false;
	}
	let listCreatorToolVisible = false;

	async function makeListAndUpload (listName, listPin) {
		todos = [];
		if (listName == "") {
			alert("Please enter a list name");
			return;
		}
		if (listPin == 0 || listPin.toString().length !== 4) {
			alert("Please enter a list pin that is 4 digits long");
			return;
		}
		await makeListInDatabase(listName, listPin);
		activeList = true;
		console.log("actvie list", activeList);
		closeListCreatorTool();
	}

	async function makeListInDatabase(listName, listPin) {
		console.log("baseurl:", baseURL)
		try {
			const returnValue = await fetch(`${baseURL}/api/makeList`, 
				{
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						"name": listName,
						"pin": listPin
					})
				});
      const response = await returnValue.json();
      var data = response.data;
			console.log(data)
			listCurrName = listName;
		} catch (error) {
			console.log(error);
		}
	}
	let listCurrName = "--No current list--";
	let listCurrPin = 0;
	let activeList = false;

	function openAccessOldList () {
		accessOldListToolVisible = true;
	}
	function closeAccessOldList () {
		accessOldListToolVisible = false;
	}
	let accessOldListToolVisible = false;

	async function getListFromDB(listname, listpin) {
		try {
			const returnValue = await fetch(`${baseURL}/api/getList`, 
				{
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						"name": listname,
						"pin": listpin
					})
				});
      const response = await returnValue.json();
      var data = response.data;
			console.log(data)
			if (data === null) {
				let text = "No list named\"" + listname + "\" found" + "or incorrect pin";
				alert(text)
			}
			todos = data.todos;
			listCurrName = data.name;
			activeList = true;
			console.log(todos)
			console.log(data.name)
			closeAccessOldList();
		} catch (error) {
			console.log(error);
		}
	}
</script>


<style>
	* {
		margin: 0px;
		padding: 0px
	}
	.container { /**Class*/
		width: 100%;
		height: content-contain;
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
	button:hover {
		cursor: pointer;
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
		padding: 10px 8px;
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
		min-width: 150px;
	}
	a {
		color: #4db848;
	}
	.bigdoge {
		position: absolute;
	}
	.corner0 {
		position: absolute;
		right: -100%;
		bottom: -100%;
	}
	.corner1 {
		position: absolute;
		right: -30%;
		bottom: -30%;
	}
	.corner2 {
		position: absolute;
		right: -20%;
		bottom: -20%;
	}
	.corner3 {
		position: absolute;
		right: -10%;
		bottom: -10%;
	}
	.corner4 {
		position: absolute;
		right: -2%;
		bottom: -2%;
	}
	.corner5 {
		position: absolute;
		right: 10%;
		bottom: 10%;
	}
	.main {
		overflow: hidden;
		width: 100%;
		position: relative;
	}
	.bold {
		font-weight: bold;
	}
	.secondaryButton {
		background: #6155ce;
		color: white;
		padding-left: 8px;
		padding-right: 8px;
		padding-top: 4px;
		padding-bottom: 4px;
		border-radius: 20px;
	}
	.secondaryButton:hover {
		background: #7162f5;
	}
</style>

<div class="main">
	<div class="bigdoge-container">
		<img class={dogeClass} src={"todogesad.png"} alt="big-doge"/>
	</div>
	{#if chatVisible === true}
		<DogeChat chatipdata={ipdata} close={closeChat}/>
	{/if}
	<div class="container">
	<Navbar username="unknown unknown" openPopup={openAlertPopup}/>
	{#if popupVisible === true}
		<AlertPopup close={closeAlertPopup}/>
	{/if}
	{#if listCreatorToolVisible === true}
		<CreateNewList 
			cancelList={closeListCreatorTool}
			makeList={makeListAndUpload}
		/>
	{/if}
	{#if accessOldListToolVisible === true}
		<AccessOldList 
			cancelList={closeAccessOldList}
			getList={getListFromDB}
		/>
	{/if}
	{#if width > 200}
		<Doge />
		<DogeFact />
		<div class="todo">
			<p><span class="bold">List: </span>{listCurrName} 
				<button class="secondaryButton" on:click={openListCreatorTool}>Make new list</button>
				<button class="secondaryButton" on:click={openAccessOldList}>Access old list</button>
			</p>
			{#if activeList === true}
			<div class = "form">
				<input type="text" bind:value={task} /> <!--Properties--> <!--Binding-->
				<button class="buttonLong" on:click={addTask}>Add ToDOGE</button> <!--Properties Reactivity-->
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
			{:else}
				<div class="tasks">
					<p>There is no current listlist</p>
				</div>
			{/if}
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
		<p style="width: 500px; text-align: center">By the way, it's great to have you visiting from {referrer === "" ? "from a random link" : referrer}, {level < 30 ? "but you might want to stop visiting them and focus on your toDOGES as your battery is currently at" : "and you have plenty of time to spend on your toDOGE list as your battery is at"} {level}%</p>
		<Copyright disclaimer="All rights reserved" name="copyrightsAreLegitIfTheyExistOnAWebsite©"/> <!--Component Props-->
	{:else}
		<p style="width: 270px">Please visit with a larger screen size. This is not made for mobile</p>
	{/if}
	</div>
</div>