<script>
  export let width;
  export let height;
  export let topTouchingPoint, bottomTouchingPoint;
  export let hasLost;

  function distance(a, b) {
    return Math.abs(a - b);
  }

  let level = 0.2;
  let levelInverse = 1 - level;

  let midPoint = width * 0.5;
  let pipeWidth = width * 0.05;

  let gap = pipeWidth;
  let pipesCount = width / pipeWidth;
  let speed = 5;

  const pipes = [];
  for (let i = 0; i < pipesCount; i++) {
    pipes.push({
      x: midPoint + i * gap,
      y: (i % 2) * height * levelInverse,
      width: width * 0.05,
      height: height * level
    });
  }
  function animate() {
    if (hasLost) {
      return;
    }
    let topTouching = 0,
      bottomTouching = height;
    for (let i = 0; i < pipesCount; i++) {
      pipes[i].x -= speed;
      if (pipes[i].x < -pipeWidth * 2) {
        let randomizedLevel = level * Math.random();
        let randomizedLevelInverse = 1 - randomizedLevel;

        pipes[i].x = width;
        pipes[i].y = (i % 2) * height * randomizedLevelInverse;
        pipes[i].height = height * randomizedLevel;
      }
      if (distance(pipes[i].x, midPoint) < gap) {
        if (i % 2 === 0) {
          topTouching = pipes[i].height;
        } else {
          bottomTouching = pipes[i].y;
        }
      }
    }
    topTouchingPoint = topTouching + 15;
    bottomTouchingPoint = bottomTouching - 15;

    if (level < 0.48) {
      level += 0.0001;
      levelInverse = 1 - level;
    } else {
      speed += 0.005;
    }
    requestAnimationFrame(animate);
  }
  requestAnimationFrame(animate);
</script>

<g>
  {#each pipes as pipe}
    <rect fill="yellow" transform="translate({pipe.x} {pipe.y})" width={pipe.width} height={pipe.height} />
  {/each}
</g>
