<script>
  export let mouse = { x: 0, y: 0 };

  export let data = { x: 0, y: 0 };
  export let test = false;
  export let last = false;

  $: {
    if (last) {
      // ------- version 1 ---------------
      // this blocks the entire render process, till it breaks
      // uncomment this and comment "version 2":

      //   data.x = mouse.x;
      //   data.y = mouse.y;
      //   console.log("v1 computed updated", data, mouse, test);
      //   data = data;
      // --- end version 1

      // -------- version 2 --------------
      // this will be used to update
      // it wont break the rendering, but it seems, that there is a memory leak?
      updateStuff(data, mouse);
      // --- end version 2
    }
  }

  function updateStuff(d, m) {
    d.x = m.x;
    d.y = m.y;
    data = data;
    console.log("v2 computed updated", d, m, test);
  }
</script>

<p>
  last:{last}
  {"x: " + data.x + " | y: " + data.y}
  {test}
</p>

{#if !last}
  <svelte:self bind:data {mouse} test={data.x === 2} last={true} />
{/if}
