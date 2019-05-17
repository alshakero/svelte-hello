<script>

  import Bird from "./components/Bird.svelte";
  import World from "./components/World.svelte";
  import Score from "./components/Score.svelte";

  let topTouchingPoint, bottomTouchingPoint;
  let width = window.innerWidth;
  let height = window.innerHeight;
  let hasLost = false;
  let score = 0;
  let player;

  const SPACE = 32;

  let position = { x: 0, y: height / 2 - 25 };
  let speed = 0.1;
  let accelaration = 1;

  function checkIfLost() {
    if (Math.abs(position.y - topTouchingPoint) < 10) {
      hasLost = true;
    }
    if (Math.abs(position.y - bottomTouchingPoint) < 10) {
      hasLost = true;
    }
  }

  function animate() {
    if (hasLost) {
      player.pause();
      return;
    }
    if (position.x - 50 < width / 2) {
      position.x += 5;
    }
    position.y += speed * accelaration;
    accelaration += 0.5;
    score++;
    checkIfLost();
    requestAnimationFrame(animate);
  }
  requestAnimationFrame(animate);
  let isKeyDown = false;
  function handleDown(event) {
    if ((event.which === SPACE && !isKeyDown) || event.type === 'touchstart') {
      isKeyDown = true;
      speed = -1;
      accelaration = 1;
    }
  }
  function handleUp(event) {
    if ((event.which === SPACE) || event.type === 'touchend') {
      isKeyDown = false;
      speed = 1;
      accelaration = 1;
    }
  }
</script>

<style>
  svg {
    width: 100%;
  }
</style>

<svelte:window
  on:keydown={handleDown}
  on:touchstart={handleDown}
  on:keyup={handleUp}
  on:touchend={handleUp} />

<audio
  loop
  bind:this={player}
  src="./assets/music-loop.mp3"
  autoplay
  volume="0.2" />

<svg
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink"
  x="0px"
  y="0px"
  viewBox="0 0 {width}
  {height}">
  <World
    {hasLost}
    {width}
    {height}
    bind:topTouchingPoint
    bind:bottomTouchingPoint />
  <Bird {hasLost} x={position.x} y={position.y} />
  <Score {score} {width} />
</svg>
