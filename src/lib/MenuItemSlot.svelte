<script>
  export let option;
  export let idx;
</script>

<div>
  {idx + 1}
  <span style:background={option} />
  {option}
</div>

<style>
  div {
    display: flex;
    place-items: center;
    gap: 5pt;
  }
  div > span {
    width: 1em;
    height: 1em;
    border-radius: 2pt;
  }
</style>
