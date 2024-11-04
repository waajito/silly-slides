<script>
  let count = $state(0);

  function increment() {
    count += 1;
    console.log(count);
  }
</script>

<button onclick={increment}>
  clicks: {count}
</button>
