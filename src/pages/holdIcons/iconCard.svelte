<script>
  let { iconUrl, name } = $props();
  let enable = $state(0);
  let count = $state(0);

  function increment() {
    count = count + 1;
  }
  let hidden = $derived(count != 0);

  let progress = $state(0);
  let filled = $derived(count > 0);
  let interval;

  function decrement() {
    count = count - 1;
  }
  function onPressStart() {
    progress = 0;
    interval = setInterval(() => {
      progress += 0.01;
      if (progress >= 1) {
        increment();
        clearInterval(interval);
      }
    }, 10);
  }

  function onPressRelease() {
    clearInterval(interval);
    progress = 0;
  }
</script>

<button
  class="container iconCard"
  class:filled
  onmousedown={onPressStart}
  onmouseup={onPressRelease}
  onmouseleave={onPressRelease}
  ontouchstart={onPressStart}
  ontouchend={onPressRelease}
  ontouchcancel={onPressRelease}
>
  <div class="count" class:show={hidden} onclick={decrement}>
    <img src="/icons/close.svg" alt="close" height="6" width="6" />
    <p>{count}</p>
  </div>
  <img class="icon" src={iconUrl} alt={name} />
  <p class="name">{name}</p>
  <div class="fill" style={`width : ${progress * 100}%`}></div>
</button>

<style lang="scss">
  button {
    all: unset;
    background-color: none;

    &:focus {
      outline: none;
      background-color: none;
    }
  }
  *::selection {
    background: none;
  }

  .count {
    transition: all 0.3s ease;
    transform: translateY(-80px);
  }

  .show {
    transform: translateY(0px);
  }

  .container {
    -webkit-tap-highlight-color: transparent;
    height: 100px;
    width: 100px;
    display: flex;
    flex-direction: column;
    background-color: #2222221a;
    border-radius: 24px;
    padding: 16px;
    gap: 8px;
    justify-content: center;
    align-items: center;
    transition: all 0.3s ease 0.1s;
    overflow: hidden;
    position: relative;
    cursor: pointer;

    .icon {
      height: 50px;
      width: 50px;
      filter: invert(0.4);
      transition: all 0.3s ease;
      z-index: 3;
      pointer-events: none;
      &::selection {
        background: none;
      }
    }

    .name {
      font-family: "Noto Sans", sans-serif;
      font-size: 16px;
      font-weight: 500;
      color: #a6a6a6;
      margin: 0px;
      transition: all 0.3s ease-in;
      z-index: 1;
    }
    .count {
      position: absolute;
      top: 12px;
      right: 12px;
      background-color: white;
      color: #4487de;
      border-radius: 50%;
      height: 24px;
      width: 24px;
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 4;
      display: flex;
      justify-content: center;
      align-items: center;

      img {
        transform: translateY(1px);
        margin-right: 2px;
        /* non clickable */
        pointer-events: none;
      }
      p {
        color: #539dff;
        font-size: 12px;
        font-weight: 700;
      }
    }
    .fill {
      height: 100%;
      /* width: 0%; */
      background: #00000017;
      position: absolute;
      top: 0;
      left: 0;
      z-index: 2;
      /* transition: all 1.5s ease 0.2s; */
    }
    &.filled {
      background-color: #00000018;
      .icon {
        filter: invert(0) !important;
      }
      .name {
        color: #ffffff !important;
      }
    }

    @media screen and (min-width: 768px) {
      &:hover {
        background-color: #539dff;
        .icon {
          filter: invert(0);
          scale: 1.3;
          transform: translateY(8px);
        }

        .name {
          color: #ffffff;
          transform: translateY(50px);
          opacity: 0;
        }
      }
    }

    &:active {
      scale: 0.95;

      .fill {
        /* width: 100%; */
      }
    }

    @media screen and (max-width: 768px) {
      &.filled {
        background-color: #4487de !important;
      }
      transition: all 0.3s ease 0s;
      &:hover {
        background-color: #2222221a;
      }

      &:active {
        /* background-color: #539dff; */
        .icon {
          /* filter: invert(0); */
          scale: 1.3;
          transform: translateY(8px);
        }

        .name {
          /* color: #ffffff; */
          transform: translateY(50px);
          opacity: 0;
        }

        scale: 0.95;
      }
    }
  }
</style>
