<script lang="ts">
  export let username;
  export let openPopup = () => {};
  import Title from './Title.svelte';
</script>

<style>
  	button:hover {
		cursor: pointer;
	}
/* Creates the an amazing and simple looking navbar*/
.navbar {
  margin-top: 0px;
  /* background-color: #e3f2fd; */
  width: 100%;
  height: 68px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.navbar-brand {
  color: #0d47a1;
  display: flex;
  align-items: center;
}
.rightBlob {
  display: flex;
  align-items: center;
  margin-right: 10px;
}
button {
  /* width: 100px; */
  background-color: #4db848;
  color: white;
  font-weight: bold;
  border-radius: 20px;
  padding-left: 18px;
  padding-right: 18px;
  padding-top: 10px;
  padding-bottom: 10px;
  border: none;
  margin-bottom: 0;
}
.rightMargin {
  padding-right: 40px !important;
}
</style>

<nav class="navbar">
  <div class="navbar-brand">
    <img src={'todoge.png'} width="60" height="60" alt="doge logo">
  </div>
  <Title title="ToDOGE List" /> <!--Component Prop -->
  <div class="rightBlob">
    <!-- <p>Sean</p> -->
    <!-- <p className="rightMargin">Account: seanroade...</p> -->
    <button on:click={openPopup}>Upgrade</button>
  </div>
</nav>