<script>
import global from '../global.css';
import Header from '../components/Header.svelte';
import Footer from '../components/Footer.svelte';
</script>

<svelte:head>
  <title>NeoMoviesDB</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@500;700&display=swap" rel="stylesheet">
</svelte:head>

<Header />
<slot />
<Footer />