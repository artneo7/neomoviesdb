<script context="module">
  export async function load({fetch}) {
    const res = await fetch(`https://api.themoviedb.org/3/movie/popular?api_key=${import.meta.env.VITE_API}&language=en-US&page=1`);
    const data = await res.json();
    if (res.ok) {
      return {
        props: {popular: data.results}
      }
    }
  }
</script>

<script>
export let popular;
import Title from '../components/Title.svelte';
import { Splide, SplideSlide } from '@splidejs/svelte-splide';
import '@splidejs/svelte-splide/css/core';
import PopularMovies from '../components/PopularMovies.svelte';
import Search from '../components/Search.svelte';
import { MetaTags } from 'svelte-meta-tags';

</script>

<MetaTags
  title="NeoMoviesDB"
  noindex
  nofollow
  openGraph={{
    title: 'NeoMoviesDB',
    description: 'Personal project to learn about Svelte.js',
    images: [
      { url: '/images/default-image.jpg' }
    ],
    site_name: 'NeoMoviesDB'
  }}
/>

<main>
  <section class="intro">
    <div class="intro__wrapper">
      <div class="intro__title-wrapper wrap">
        <Title title={"Featured"} />
      </div>
      <Splide class="slider"
        options={ {
          perPage: 3,
          fixedHeight: 380,
          arrows: false,
          pagination: false,
          gap: '10px',
          type: 'loop',
          padding: '120px',
          breakpoints: {
            1000: {
              perPage: 2,
              padding: '90px'
            },
            700: {
              perPage: 1,
              padding: '60px',
            }
          }
        } }
        aria-label="Recommended movies to watch">
        <PopularMovies {popular} />
      </Splide>
    </div>
  </section>

  <Search results={popular} />

</main>

<style>
.intro {
  height: 460px;
  padding-top: 142px;
  background: linear-gradient(rgba(0,0,0,.55),rgba(0,0,0,.55)), url('/images/default-image.jpg');
  background-position: top;
  background-size: cover;
}
</style>
