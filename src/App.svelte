<script>
  import Switch from "./Switch.svelte";

  function randomNumber(max) {
    return Math.floor(Math.random() * Math.floor(max) + 1);
  }
  let count = 1;
  let randomTypewriterSound = randomNumber(3);
  let randomInstrumentSound = randomNumber(14);
  let rain = false;
  let typewriter = true;
  let instrument = true;
  let rainSound = new Audio("/sounds/rain.mp3");
  let rainVolume = 0.8;
  let typewriterSound;
  let typewriterVolume = 0.3;
  let instrumentSound;
  let instrumentVolume = 0.4;

  function changeOpacityToZero() {
    document.querySelector(".fx-container").style = `
      opacity: 0;
    `;
  }

  function changeOpacityToVisible() {
    document.querySelector(".fx-container").style = `
      opacity: 1;
    `;
  }

  function enableRain() {
    if (rain) {
      rainSound.play();
      rainSound.volume = rainVolume;
      rainSound.loop = true;
    } else {
      rainSound.pause();
    }
  }

  function enableTypewriter(e) {
    randomTypewriterSound = randomNumber(2);
    typewriterSound = new Audio(
      "/sounds/typewriter/typewriter_" + randomTypewriterSound + ".wav"
    );
    typewriterSound.play();
    typewriterSound.volume = typewriterVolume;
  }

  function enableInstrument() {
    randomInstrumentSound = randomNumber(15);
    instrumentSound = new Audio(
      "/sounds/piano/piano_" + randomInstrumentSound + ".mp3"
    );
    instrumentSound.volume = instrumentVolume;
    if (count === randomNumber(8)) instrumentSound.play();
  }
</script>

<style>
  main {
    margin: 20px;
  }
  .fx {
    width: 230px;
    margin-bottom: 10px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    justify-content: space-between;
  }

  textarea {
    display: block;
    margin: 40px auto 0;
    padding: 12px;
    width: 600px;
    height: 400px;
    outline: none;
    font-size: 22px;
    background: #111;
    color: #c859fe;
    border: none;
    resize: none;
    line-height: 1.5;
    overflow: scroll;
  }

  .fx-container {
    opacity: 1;
    transition: 2s;
  }
</style>

<main on:click={() => document.querySelector('textarea').focus()}>
  <div on:mousemove={changeOpacityToVisible} class="fx-container">
    <div class="fx">
      <span class="fx-name">Rain</span>
      <Switch
        on:click={() => {
          rain = !rain;
          enableRain();
        }}
        bind:check={rain}
        bind:volume={rainVolume}
        sound={rainSound} />
    </div>

    <div class="fx">
      <span class="fx-name">Typewriter</span>
      <Switch
        bind:check={typewriter}
        bind:volume={typewriterVolume}
        sound={typewriterSound}
        on:click={() => {
          typewriter = !typewriter;
        }} />
    </div>

    <div class="fx">
      <span class="fx-name">instrument</span>
      <Switch
        bind:check={instrument}
        bind:volume={instrumentVolume}
        sound={instrumentSound}
        on:click={() => {
          instrument = !instrument;
        }} />
    </div>
  </div>

  <textarea
    on:mousemove={changeOpacityToVisible}
    on:keydown={e => {
      if (typewriter && e.key != 'Shift') enableTypewriter();
      if (instrument) enableInstrument();
      count++;
      if (count === 9) count = 1;
      changeOpacityToZero();
      console.log(e);
    }}
    cols="30"
    rows="10"
    autofocus
    placeholder="What are you thinking about?.." />

</main>
