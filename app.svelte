<script>
  import Comp from "./comp.svelte";
  let mainContainer;
  let mouse = { x: 0, y: 0 };

  let data = { x: 0, y: 0 };

  function debounce(func, timesPerSecond = 30) {
    const timeout = 1000 / timesPerSecond;
    let lastCall = Date.now() - timeout;
    return (event) => {
      const cur = Date.now();
      if (cur - lastCall < timeout) return;
      func(event);
      lastCall = cur;
    };
  }

  function handleMousemove(event) {
    const rect = mainContainer.getBoundingClientRect();
    const cX = event.clientX - rect.left; //x position within the element.
    const cY = event.clientY - rect.top; //y position within the element.

    console.log("mouse update");
    mouse.x = cX;
    mouse.y = cY;
    mouse = mouse;
  }
  const debouncedMouseMove = debounce(handleMousemove);
</script>

<div class="main" bind:this={mainContainer} on:mousemove={debouncedMouseMove}>
  <Comp {data} {mouse} test={data.x === 1} />
</div>

<style>
  .main {
    width: 100vw;
    height: 100vh;
  }
</style>
