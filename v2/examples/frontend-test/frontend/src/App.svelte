<script lang="ts">
  import logo from './assets/images/logo-universal.png'
  import {Greet} from '../wailsjs/go/main/App.js'
  import * as runtime from '../wailsjs/runtime/runtime'

  let resultText: string = "Please enter your name below 👇"
  let name: string

  let title: string = ""

  function greet(): void {
    Greet(name).then(result => resultText = result)
  }

  function actionThenRevert(action: () => void, revert: () => void): () => void {
    return () => {
      action()
      setTimeout(revert, 1000);
    }
  }
</script>

<main>
  <img alt="Wails logo" id="logo" src="{logo}">

  <div class="actions">
    <div>
      <h3>Api Test</h3>
      <div class="result" id="result">{resultText}</div>
      <div class="input-box" id="input">
        <input autocomplete="off" bind:value={name} class="input" id="name" type="text"/>
        <button class="btn" on:click={greet}>Greet</button>
      </div>
    </div>

    <div>
      <h3>Window Title</h3>
      <div class="input-box" id="input">
        <input autocomplete="off" bind:value={title} class="input" id="name" type="text"/>
        <button class="btn" on:click={() => runtime.WindowSetTitle(title)}>Set Title</button>
      </div>
    </div>

    <div>
      <h3>Window States</h3>
      <button class="btn" on:click={actionThenRevert(runtime.WindowMaximise, runtime.WindowUnmaximise)}>Maximize/UnMaximize</button>
      <button class="btn" on:click={actionThenRevert(runtime.WindowFullscreen, runtime.WindowUnfullscreen)}>Fullscreen/UnFullscreen</button>
      <button class="btn" on:click={actionThenRevert(runtime.WindowMinimise, runtime.WindowUnminimise)}>Minimize/UnMinimize</button>
      <button class="btn" on:click={actionThenRevert(runtime.WindowHide, runtime.WindowShow)}>Hide/Show</button>
      <button class="btn" on:click={
        actionThenRevert(
          () => runtime.WindowSetAlwaysOnTop(true),
          () => runtime.WindowSetAlwaysOnTop(false)
        )}>AlwaysOnTop/NotOnTop</button>
    </div>
    <div>
      <h3>Window Positioning</h3>
      <button class="btn" on:click={() => runtime.WindowSetPosition(100, 100)}>Move to (100,100)</button>
      <button class="btn" on:click={() => runtime.WindowCenter()}>Center</button>
    </div>
  </div>
</main>

<style>
  main {
    padding: 2rem;
  }

  #logo {
    display: block;
    width: 10em;
    height: auto;
    margin: auto;
  }

  .result {
    height: 20px;
    line-height: 20px;
    margin: 1.5rem auto;
  }

  .btn {
    /*width: 60px;*/
    height: 30px;
    line-height: 30px;
    border-radius: 3px;
    border: none;
    /*margin: 0 0 0 20px;*/
    padding: 0 8px;
    margin-left: 1rem;
    margin-bottom: 2rem;
    cursor: pointer;
  }

  .input-box .btn:hover {
    background-image: linear-gradient(to top, #cfd9df 0%, #e2ebf0 100%);
    color: #333333;
  }

  .input-box .input {
    border: none;
    border-radius: 3px;
    outline: none;
    height: 30px;
    line-height: 30px;
    padding: 0 10px;
    background-color: rgba(240, 240, 240, 1);
    -webkit-font-smoothing: antialiased;
  }

  .input-box .input:hover {
    border: none;
    background-color: rgba(255, 255, 255, 1);
  }

  .input-box .input:focus {
    border: none;
    background-color: rgba(255, 255, 255, 1);
  }

  .actions {
    display: flex;
    flex-direction: column;
    /*justify-content: center;*/
    align-items: center;
    /*gap: 8px;*/
  }
</style>
