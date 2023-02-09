<script lang="ts">;
// fetch an api and return the data which is a string
const dogFact = (async () => {
  const response = await fetch('https://dogapi.dog/api/facts')
  return await response.json() // async
})()
</script>

<style>
  .factColor {
    color: #4db848;
    font-size: 12px;
    font-weight: bold;
    width: 400px;
    margin: 0;
    text-align: center;
    display: inline;
    text-decoration: underline;
  }
  .factBlack {
    color: #000000;
    font-size: 12px;
    font-weight: bold;
    width: 400px;
    margin: 0;
    text-align: center;
    display: inline;
  }
  main {
    margin-top: 10px;
    margin-bottom: 10px;
    width: 550px;
    margin-left: auto;
    margin-right: auto;
  }
  .textContainer {
    margin: auto;
  }
</style>

<main>
  {#await dogFact}
  <p>Loading doge fact</p>
  {:then data}
    <div class="textContainer">
      <p class="factColor">Dog Fact:</p>
      <p class="factBlack">{data.facts}</p>
    </div>
  {/await}
</main>