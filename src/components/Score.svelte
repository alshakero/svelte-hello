<script>
  export let width, score;
  $: length = 30 + score.toString().length * 15;
</script>

<g>
  <text x={width - length} y="40" font-size="30px" font-family="monospace">
    {score}
  </text>
</g>
