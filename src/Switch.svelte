<script>
  export let check = false;
  export let volume;
  export let sound;

  function changeVolume(e) {
    sound.volume = e.target.value;
  }
</script>

<style>
  /* The switch - the box around the slider */
  .switch {
    position: relative;
    display: inline-block;
    width: 30px;
    height: 16px;
  }

  /* Hide default HTML checkbox */
  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  /* The slider */
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    transition: 0.4s;
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 10px;
    width: 10px;
    left: 4px;
    bottom: 3px;
    background-color: white;
    transition: 0.4s;
  }

  input:checked + .slider {
    background-color: #c859fe;
  }

  /* input:focus + .slider {
    box-shadow: 0 0 1px #2196f3;
  } */

  input:checked + .slider:before {
    transform: translateX(12px);
  }

  .vol {
    margin-top: 8px;
    width: 30px;
  }
</style>

<label class="switch">
  <input type="checkbox" on:click bind:checked={check} />
  <span class="slider" />
</label>
<div class="vol">
  <input
    on:input={changeVolume}
    type="range"
    bind:value={volume}
    {sound}
    min="0"
    max="1"
    step="0.1" />
</div>
