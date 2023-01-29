<script>
  export let disclaimer;
  export let name;
</script>

<style>
  .smallText {
    font-size: 12px;
    color: black;
    text-align: center;
  }
</style>

<main>
  <p class="smallText">{disclaimer}</p>
  <br />
  <p class="smallText">{name}</p>
</main>