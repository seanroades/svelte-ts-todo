<script>
	const fetchImage = (async () => {
		const response = await fetch('https://dog.ceo/api/breeds/image/random')
    return await response.json() // async
	})()
  const fetchImage2 = (async () => {
		const response = await fetch('https://dog.ceo/api/breeds/image/random')
    return await response.json() // async
	})()
  const fetchImage3 = (async () => {
		const response = await fetch('https://dog.ceo/api/breeds/image/random')
    return await response.json() // async
	})()
</script>

<style>
  .image {
    height: 200px !important;
    width: 200px !important;
    border-radius: 20px;
    background-color: black;
    object-fit: cover;
  }
</style>

<div>
  {#await fetchImage}
    <p>Loading</p>
  {:then data}
    <img class="image" src={data.message} alt="dog"/>
  {/await}
  {#await fetchImage2}
  <p>Loading</p>
  {:then data}
    <img class="image" src={data.message} alt="dog"/>
  {/await}
  {#await fetchImage3}
  <p>Loading</p>
  {:then data}
  <img class="image" src={data.message} alt="dog"/>
  {/await}
</div>
