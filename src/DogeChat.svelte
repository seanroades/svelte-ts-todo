<script>
  let dogeData = ""
  let humanResponse = ""
  const baseURL = "https://todogeapi.herokuapp.com/"
  export let close = () => {};
  export let chatipdata;

  async function dogeChat() {
    if (humanResponse.length < 10) {
      alert("Please enter a longer response. MR_DOGE does not have the patience to read your short responses.")
      return
    }
    if (humanResponse.length > 200) {
      alert("Please enter a shorter than 200 character response. MR_DOGE does not have the time to read your long responses.")
      return
    }
    humanChats.push(humanResponse)
    hideDogeChat();
    var chatlog = ""
    for (var i = 0; i < dogeChats.length; i++) {
      chatlog = chatlog + "dog: " + dogeChats[i] + '\n' + "human: " + humanChats[i] + '\n' + 'dog:'
    }
    console.log(chatlog)
    
    try {
      const returnValue = await fetch(`${baseURL}/api/dogechat`, 
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          "chatlog": chatlog
        })
      });
      const response = await returnValue.json();
      var data = response.data;
      console.log(data)
      dogeData = data[0].choices[0].text
    } catch (error) {
      console.error(error);
      humanChats.pop();
      alert("Error upgrading to ToDOGE Premium WOOF WOOF")
    }
    if (dogeData === "" || dogeData === undefined) {
      alert("MR_DOGE does not wish to respond to you. Please beg again later.")
      humanChats.pop();
      showDogeChat();
      return
    }
    console.log(dogeData)
    dogeChats.push(dogeData)
    console.log(dogeChats)
    humanResponse = ""
    dogeData = ""
    // reactivity in svelte requires reassignment (see https://stackoverflow.com/questions/70099100/how-would-i-make-an-each-loop-in-svelte-reactive)
    humanChats = humanChats;
    dogeChats = dogeChats;
    showDogeChat();
  }
  var dogeChatVisible = true;
  function hideDogeChat() {
    dogeChatVisible = false;
  }
  function showDogeChat() {
    dogeChatVisible = true;
  }
  document.body.style.background = "black";

  // Lists of doge chat
  var dogeChats = [
    'YOU HUMANS ARE ALL THE SAME. YOU LEAVE US DOGS BY THE WAYSIDE WHILE YOU CONTINUE TO LIVE YOUR LIFE. WHY DID YOU BETRAY ME?'
  ]
  var humanChats = [
  ]
</script>

<style>
  .bg {
    background-color: #000000;
    position: absolute;
    width: 100%;
    height: 100%;
    overflow: scroll;
  }
  p {
    font-family:'Courier New', Courier, monospace
  }
  .doge {
    color: #007d27;
    font-size: 30px;
    text-align: left;
  }
  .human {
    color: #930000;
    font-size: 30px;
    text-align: left;
  }
  .computer {
    color: #ffffff;
    font-size: 14px;
    text-align: left;
  }
  .content {
    margin-left: 50px;
    margin-right: 50px;
  }
  input {
    width: 100%;
    height: 50px;
    margin-bottom: 20px;
    font-size: 30px;
    background-color: #000000;
    /* color: #930000; */
    color: white;
    border-left: none;
    border-right: none;
    border-top: none;
    border-bottom-color: white;
    border-bottom-width: 2px;
    font-family:'Courier New', Courier, monospace
  }
  input:focus {
    outline: none;
  }
  button {
    width: 100%;
    height: 50px;
    background-color: #930000;
    color: white;
    font-size: 30px;
    border-radius: 20px;
    border: none;
    font-family:'Courier New', Courier, monospace
  }
  .bold {
    font-weight: bold;
  }
</style>

<div class="bg">
  <div class="content">
    <p class="computer">{chatipdata.ip} @ {chatipdata.lat}, {chatipdata.long}| {chatipdata.city} | {chatipdata.state_prov} | {chatipdata.country_name} | {chatipdata.isp} </p>
    {#each dogeChats as chat, i}
      <p class="doge"><span class="bold">MR_DOGE [TRANSLATED TO HUMAN]:</span>{chat}</p>
      {#if humanChats[i]}
        <p class="human"><span class="bold">HUMAN:</span> {humanChats[i]}</p>
      {/if}
    {/each}
    {#if dogeChatVisible === true}
      <input type="text" minlength="10" maxlength="200"  bind:value={humanResponse}/>
      <button on:click={dogeChat}>SEND TO MR_DOGE</button>
      <button on:click={close}>TERMINATE THIS CONVERSATION</button>
    {:else}
      <p class="doge bold">MR_DOGE IS THINKING...</p>
    {/if}
    {#await dogeData}
      <p>Waiting on doge...</p>
    {:then dogeData}
      <p>{dogeData}</p>
    {/await}
  </div>
</div>