<script>
	import './styles.css';
</script>

<div class="app">

	<main>
		<slot />
	</main>

</div>

<style>
  main {
    width: 100%;
    height: 100vh;
    display: grid;
    place-content: center;
    box-sizing: border-box;
  }
</style>
