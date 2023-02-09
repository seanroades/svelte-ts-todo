<script>
  let listName = "";
  let listPin = "";
  export let cancelList = () => {};
  export let makeList = (listName, listPin) => {};
</script>

<style>
  .bg {
    background-color: rgba(0, 0, 0, 0.8);
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .content {
    background-color: #ffffff;
    padding: 20px;
    width: 220px;
    height: 220px;
    position: relative;
    border-radius: 20px;
  }
  input {
    border-color: #4db848;
    border-radius: 20px;
    color: #000000;
    padding-left: 15px;
    font-size: 16px;
  }
  .title {
    color: #4db848;
    font-size: 18px;
    font-weight: bold;
    margin: 0;
    text-align: left;
  }
  .subtitle {
    margin-top: 5px !important;
    margin-bottom: 5px !important;
    color: #000000;
    font-size: 16px;
    font-weight: medium;
    margin: 0;
    text-align: left;
  }
  .button {
    background: #4db848;
		color: white;
    border-radius: 20px;
    width: 100px;
    margin-top: 10px !important;
    display: inline;
  }
  .button:hover {
    background: #58d552;
    cursor: pointer;
  }
  .redButton {
    background: #aa3e3e;
    display: inline;
		color: white;
    border-radius: 20px;
    width: 100px;
    margin-top: 10px !important;
  }
  .redButton:hover {
    background: #ff5d5d;
    cursor: pointer;
  }
</style>

<div class="bg">
  <div class="content">
    <p class="title">Create a new list: </p>
    <p class="subtitle">Create a new list: </p>
    <input placeholder="create list name" maxlength="24" type="text" bind:value={listName} />
    <p class="subtitle">Create a new pin: </p>
    <input placeholder="0000" min="0" max="9999" oninput="validity.valid||(value='');" type="number" bind:value={listPin} />
    <br />
    <button class="button" on:click={() => makeList(listName, listPin)}>Create list</button>
    <button class="redButton" on:click={cancelList}>Cancel</button>
  </div>
</div>