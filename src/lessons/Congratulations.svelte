<script>
  import { onMount } from "svelte";

  let characters = ["🥳", "🎉", "✨"];

  let confetti = new Array(100)
    .fill(0)
    .map((_, i) => {
      return {
        character: characters[i % characters.length],
        x: Math.random() * 100,
        y: -20 - Math.random() * 100,
        r: 0.1 + Math.random() * 1,
      };
    })
    .sort((a, b) => a.r - b.r);

  onMount(() => {
    let frame;

    function loop() {
      frame = requestAnimationFrame(loop);
      confetti = confetti.map((emoji) => {
        emoji.y += 0.7 * emoji.r;
        if (emoji.y > 120) emoji.y = -20;
        return emoji;
      });
    }

    loop();

    return () => cancelAnimationFrame(frame);
  });
</script>

<style>
  div {
    position: relative;
    width: 500px;
    height: 500px;
    border: 1px solid red;
    overflow: hidden;
  }

  span {
    position: absolute;
    font-size: 5vw;
  }
</style>

<div>
  {#each confetti as c}
    <span
      style="left: {c.x}%; top: {c.y}%; transform: scale({c.r})">{c.character}</span>
  {/each}
</div>
