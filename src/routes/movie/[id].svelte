<script context="module">
  export async function load({fetch, params}) {
    const res = await fetch(`https://api.themoviedb.org/3/movie/${params.id}?api_key=${import.meta.env.VITE_API}&language=en-US`);
    const movieDetail = await res.json();
    if (res.ok) {
      return {
        props: {movieDetail}
      }
    }
  }
</script>

<script>
import IconBudget from "../../components/icons/IconBudget.svelte";
import IconDate from "../../components/icons/IconDate.svelte";
import IconOverview from "../../components/icons/IconOverview.svelte";
import IconRating from "../../components/icons/IconRating.svelte";
import IconRuntime from "../../components/icons/IconRuntime.svelte";

import { fade } from 'svelte/transition';
import { MetaTags } from "svelte-meta-tags";

export let movieDetail;

// Convert runtime to hours and minutes
function convertMinutes(n) {
  let number = n;
  let hours = (number / 60);
  let rhours = Math.floor(hours);
  let minutes = (hours - rhours) * 60;
  let rminutes = Math.round(minutes);
  
  let result = '';
  if (rhours >= 1) {
    result += `${rhours}h `;
  }
  if (rminutes != 0 && rminutes === 1) {
    result += `${rminutes}min`
  }
  if (rminutes != 0 && rminutes > 1) {
    result += `${rminutes}mins`
  }

  return result;
}

// Convert budget to USD
const money = new Intl.NumberFormat('us',
  { style:'currency', currency: 'USD' });
</script>

<MetaTags
  title={movieDetail.title}
  titleTemplate="%s | NeoMoviesDB"
  noindex
  nofollow
/>

<div class="single">
  <div class="single__background" style="background-image: url({'https://image.tmdb.org/t/p/original' + movieDetail.backdrop_path})" in:fade={{ duration: 300 }}></div>
  <div class="single__wrapper wrap">
    <a href="/" class="single__btn-container">
      <span class="btn single__btn">&lt;- Go back</span>
    </a>
    <h1 class="title">{movieDetail.title}</h1>
    <div class="single__info">
      <div class="single__img-wrapper">
        <div class="skeleton"></div>
        <img src={`https://image.tmdb.org/t/p/original/${movieDetail.poster_path}`} alt={`Poster image of ${movieDetail.title}`} class="single__img">
      </div>
      <div class="single__content">
        <h2 class="content__title"><IconOverview /> Overview</h2>
        <p class="content__info">{movieDetail.overview}</p>

        <div class="content__list">
          {#if movieDetail.runtime > 60}
          <div class="content__item">
            <h2 class="content__title"><IconRuntime /> Runtime</h2>
            <p class="content__info">{convertMinutes(movieDetail.runtime)}</p>
          </div>
          {/if}

          {#if movieDetail.vote_average != 0}
          <div class="content__item">
            <h2 class="content__title"><IconRating /> Rating</h2>
            <p class="content__info">{movieDetail.vote_average}</p>
          </div>
          {/if}

          {#if movieDetail.release_date}
          <div class="content__item">
            <h2 class="content__title"><IconDate /> Release date</h2>
            <p class="content__info">{movieDetail.release_date.replaceAll('-', '/')}</p>
          </div>
          {/if}

          {#if movieDetail.budget != 0}
          <div class="content__item">
            <h2 class="content__title"><IconBudget /> Budget</h2>
            <p class="content__info">{money.format(movieDetail.budget)}</p>
          </div>
          {/if}
        </div>
      </div>
    </div>
  </div>
</div>

<style>
.single__background {
  height: 460px;
  background-size: cover;
  background-position: top;
  position: relative;
}
.single__background::after {
  content: '';
  background-color: rgba(0,0,0,.55);
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  backdrop-filter: blur(8px);
}
.single__wrapper {
  margin-top: -328px;
  position: relative;
}
.single__btn {
  display: inline-block;
  transform: translate3d(0,0,0);
  transition: 450ms;
}
.title {
  font-size: 3rem;
  line-height: 1.3;
  margin-top: 32px;
  color: #fff;
}
.single__info {
  margin-top: 16px;
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 20px;
  align-items: start;
}
.single__img-wrapper {
  display: grid;
  border-radius: 16px;
  overflow: hidden;
}
.skeleton {
  grid-area: 1/1;
  background: var(--gradient);
}
.single__img {
  border-radius: 16px;
  grid-area: 1/1;
  transition: 450ms;
}
.single__content {
  padding: 32px;
  background: var(--gradient);
  border-radius: 16px;
}
.content__title {
  display: grid;
  grid-template-columns: max-content 1fr;
  gap: 8px;
  align-items: center;
  color: var(--gray);
  font-size: 0.875rem;
}
.content__info {
  color: #fff;
  font-size: 1.125rem;
  margin-top: 8px;
  max-width: 680px;
}
.content__list {
  padding-top: 24px;
  margin-top: 24px;
  border-top: 1px solid var(--gray-2);
  display: flex;
  flex-wrap: wrap;
  gap: 20px 60px;
  justify-content: start;
}

@media(max-width: 1000px) {
  .single__info {
    grid-template-columns: 1fr;
  }
  .single__img {
    max-width: 320px;
  }
}
@media(max-width: 600px) {
  .content__list {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 30px 20px;
  }
  .title {
    font-size: 2rem;
  }
}
@media(max-width: 400px) {
  .content__list {
    grid-template-columns: 1fr;
  }
  .single__img {
    min-width: unset;
  }
}

/* Prevent hover on mobile */
@media(hover: hover) {
  .single__btn-container:hover .single__btn {
    transform: translate3d(-4px,0,0);
    transition: 125ms;
  }
}
</style>