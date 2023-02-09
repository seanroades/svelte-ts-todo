<script>
  export let close = () => {}
  var isSadDoge = false;
  function setIsSadDoge(emotion) {
    if (emotion !== isSadDoge) {
      isSadDoge = emotion;
    }
  }
  import { loadStripe } from '@stripe/stripe-js';
  const stripePromise = loadStripe("pk_live_51HvHlHA20LnLaUFwl6mW1xeD7Q663eUUevLKZaLprVbEdgxpzpmltPF0tocPaS7m22sw7SuqlAp3V6xzzPWBRG3P006kMecetF");
  async function startCheckout() {
    try {
      const returnValue = await fetch(`http://localhost:5001/api/upgrade`, 
      {
        method: 'GET'
      });
      const response = await returnValue.json();
      var data = response.data;
      console.log(data)
      open(data[0]);
    } catch (error) {
      console.error(error);
      alert("Error upgrading to ToDOGE Premium WOOF WOOF")
    }
  }
</script>

<style>
  .content {
    background-color: #ffffff;
    width: 400px;
    height: 570px;
    margin-bottom: 10px;
    border-radius: 20px;
  }
  .bg {
    background-color: rgba(0,0,0,.7);
    width: 100%;
    height: 100%;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 10px;
  }
  .smalltitle {
    color: #4db848;
    font-size: 24px;
    font-weight: bold;
    margin: 0;
    text-align: center;
  }
  .inner-content {
    padding: 10px;
  }
  li {
    color: #000000;
    font-size: 18px;
    font-weight: bold;
  }
  .regText {
    color: #000000;
    font-size: 18px;
    padding-left: 20px;
  }
  .list {
    /* list-style-type: none;
    padding: 5px; */
  }
  img {
    width: 300px;
  }
  .buttons {
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
  .buttons:hover {
    background-color: #5fe957;
    cursor: pointer
  }
  .buttonsContainer {
    display: flex;
    justify-content: space-around;
  }
  .imgPlaceholder {
    width: 300px;
    height: 300px;
    margin-left: auto;
    margin-right: auto;
    display: block;
  }
</style>

<div class="bg">
  <div class="content">
    <div class="inner-content">
      <p class=smalltitle>ToDOGE Premium</p>
      <ul class="list">
        <li>Ability to dogify lists</li>
        <li>See doge gifs</li>
        <li>Get a BIG doge suprise</li>
        <li>Help feed real doges</li>
        <li>Help make mr. doge happy</li>
      </ul>
      <p class="regText">Sounds great! I would love to</p>
      <div class="imgPlaceholder">
      {#if isSadDoge === false}
        <img src={'todoge.png'} alt="todoge logo"/>
      {:else}
        <img src={'todogesad.png'} alt="todoge sad logo" />
      {/if}
      </div>
      <div class="buttonsContainer">
        <button on:click={startCheckout} on:mouseover={() => setIsSadDoge(false)} on:focus={console.log("")} class="buttons">Upgrade</button>
        <button on:click={close} on:mouseover={() => setIsSadDoge(true)} on:focus={console.log("")} class="buttons">No Upgrade, not much wow</button>
      </div>
    </div>
  </div>
</div>
