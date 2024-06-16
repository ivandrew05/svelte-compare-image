<script>
  export let imageLeftSrc = "";
  export let imageLeftAlt = "";
  export let imageRightSrc = "";
  export let imageRightAlt = "";
  let sliderPosition = 50;
  let animationFrame = null;
  function handleInput(e) {
    if (animationFrame) cancelAnimationFrame(animationFrame);
    animationFrame = requestAnimationFrame(() => {
      sliderPosition = e.target.valueAsNumber;
    });
  }
  function handleClick(e) {
    e.target.focus();
  }
</script>

<div
  class="svelte-compare-image-container"
  style="--slider-position: {sliderPosition}%;"
  data-testid="svelte-compare-image"
>
  <img src={imageLeftSrc} alt={imageLeftAlt} class="left-img" />
  <img src={imageRightSrc} alt={imageRightAlt} class="right-img" />

  <label>
    <span class="visually-hidden">
      <slot name="slider-label"
        >Set the visibility of one image over the other. 0 is full visibility of
        the second image and 100 is full visibility of the first image. Any
        amount in-between is a left/right cutoff at the percentage of the
        slider.</slot
      >
    </span>
    <input
      type="range"
      min="0"
      max="100",
      step="0.25",
      value={sliderPosition}
      on:input={handleInput}
      on:change={handleInput}
      on:click={handleClick}
    />
  </label>
</div>

<style>
  .svelte-compare-image-container {
    box-sizing: border-box;
    position: relative;
    overflow: hidden;
    width: 100%;
    height: 100%;
  }

  img {
    display: block;
    width: 100%;
    height: 100%;
    margin: 0;
    object-fit: contain;
  }

  .left-img {
    clip-path: polygon(
      0 0,
      var(--slider-position) 0,
      var(--slider-position) 100%,
      0 100%
    );
  }

  .right-img {
    position: absolute;
    top: 0;
    clip-path: polygon(
      var(--slider-position) 0,
      100% 0,
      100% 100%,
      var(--slider-position) 100%
    );
  }

  .visually-hidden {
    clip: rect(0 0 0 0);
    clip-path: inset(50%);
    height: 1px;
    overflow: hidden;
    position: absolute;
    white-space: nowrap;
    width: 1px;
  }

  label {
    position: absolute;
    display: flex;
    align-items: stretch;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
  }

  label::before {
    content: "";
    position: absolute;
    top: 0;
    bottom: 0;
    left: calc(var(--slider-position) - var(--slider-width, 0.1875rem) / 2);
    width: var(--slider-width, 0.1875rem);
    background: var(--slider-color, #ffffff);
    pointer-events: none;
    transform: translateY(calc(50% + var(--handle-size, 2.75rem) / 2));
  }

  label::after {
    content: "";
    position: absolute;
    top: 0;
    bottom: 0;
    left: calc(var(--slider-position) - var(--slider-width, 0.1875rem) / 2);
    width: var(--slider-width, 0.1875rem);
    background: var(--slider-color, #ffffff);
    pointer-events: none;
    transform: translateY(calc(-50% - var(--handle-size, 2.75rem) / 2));
  }

  input[type="range"] {
    position: relative;
    cursor: pointer;
    margin: 0 calc(var(--handle-size, 2.75rem) / -2);
    width: calc(100% + var(--handle-size, 2.75rem));
    padding: 0;
    border-radius: 0;
    appearance: none;
    -webkit-appearance: none;
    background: none;
    border: none;
  }

  input[type="range"]::-moz-range-thumb {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: var(--handle-background-color, rgba(0, 0, 0, 0));
    background-image: var(
      --handle-background-image,
      url('data:image/svg+xml,<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><polygon fill="%23FFFFFF" points="6.36,18 0,12 6.36,6"/><polygon fill="%23FFFFFF" points="17.64,18 24,12 17.64,6"/></svg>')
    );
    background-size: 59%;
    background-position: center center;
    background-repeat: no-repeat;
    border: var(--handle-border-width, 0.1875rem) solid
      var(--slider-color, #ffffff);
    border-radius: 100%;
    box-shadow:
      0 3px 1px -2px rgba(0, 0, 0, 0.2),
      0 2px 2px 0 rgba(0, 0, 0, 0.14),
      0 1px 5px 0 rgba(0, 0, 0, 0.12);
    color: var(--slider-color, #ffffff);
    width: var(--handle-size, 2.75rem);
    height: var(--handle-size, 2.75rem);
  }

  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: var(--handle-background-color, rgba(0, 0, 0, 0));
    background-image: var(
      --handle-background-image,
      url('data:image/svg+xml,<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><polygon fill="%23FFFFFF" points="6.36,18 0,12 6.36,6"/><polygon fill="%23FFFFFF" points="17.64,18 24,12 17.64,6"/></svg>')
    );
    background-size: 59%;
    background-position: center center;
    background-repeat: no-repeat;
    border: var(--handle-border-width, 0.1875rem) solid
      var(--slider-color, #ffffff);
    border-radius: 100%;
    box-shadow:
      0 3px 1px -2px rgba(0, 0, 0, 0.2),
      0 2px 2px 0 rgba(0, 0, 0, 0.14),
      0 1px 5px 0 rgba(0, 0, 0, 0.12);
    color: var(--slider-color, #ffffff);
    width: var(--handle-size, 2.75rem);
    height: var(--handle-size, 2.75rem);
  }
</style>
