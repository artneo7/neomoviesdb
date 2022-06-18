<script>
import { Splide, SplideSlide } from '@splidejs/svelte-splide';

export let popular;

let popularList = popular.sort((a, b) => b['vote_average'] - a['vote_average']);

</script>

{#each popularList as movie, i}
  {@const length = `${movie.vote_average}`.length}
  {#if i < 8}
  <SplideSlide>
    <a sveltekit:prefetch href={`/movie/${movie.id}`} class="movie-wrapper">
      <div class="movie">
        <div class="movie__image-wrapper">
          <img src={`https://image.tmdb.org/t/p/original${movie.backdrop_path}`} alt="Image for {movie.title}" class="movie__image">
        </div>
        <div class="movie__info">
          <span class="rating">{movie.vote_average}{length < 3 ? '.0' : ''}</span>
          <h2 class="title">{movie.title}</h2>
        </div>
      </div>
    </a>
  </SplideSlide>
  {/if}
{/each}

<style>
.movie-wrapper {
  display: block;
  background-color: rgba(0, 0, 0, .2);
  height: 100%;
  padding: 20px;
  position: relative;
  top: -40px;
  border-radius: 8px;
  backdrop-filter: blur(8px);
}
.movie {
  height: 100%;
  display: grid;
  position: relative;
  top: 40px;
  transform: translate3d(0,0,0);
  transition: 450ms;
}
.movie::after {
  content: '';
  background-color: rgba(0, 0, 0, .55);
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  border-radius: 16px;
}
.rating {
  color: #fff;
  font-size: 1.125rem;
  padding: 4px 12px;
  background: var(--gradient);
  border-radius: 4px;
  z-index: 1;
  margin-top: auto;
}
.title {
  color: #fff;
  font-size: 1.5rem;
  z-index: 1;
  line-height: 1.2;
  margin-top: 8px;
}
.movie__image-wrapper {
  grid-area: 1/1;
  height: 100%;
  border-radius: 16px;
  overflow: hidden;
}
.movie__image {
  height: 100%;
  object-fit: cover;
  transition: 450ms;
}
.movie__info {
  grid-area: 1/1;
  padding: 20px;
  z-index: 1;
  display: grid;
  justify-items: start;
  align-content: end;
}

/* Prevent hover on mobile */
@media(hover: hover) {
  .movie-wrapper:hover .movie {
    transform: translate3d(0,-8px,0);
    transition: 125ms;
  }
}
</style>