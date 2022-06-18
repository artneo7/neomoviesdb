<script>
import Loading from "./Loading.svelte";
import Title from "./Title.svelte";
import { fly } from 'svelte/transition';
import IconSearch from "./icons/IconSearch.svelte";

export let results;
let cloneResults = [...results];

// Remove the first 8 movies as they are in the slider
let searchResults = cloneResults.splice(8, 20);
let cloneSearchResults = [...searchResults];

let keyword = '';
let loading;

$: keywordLength = keyword.length;

const getData = async () => {
  loading = true;
  try {
    const response = await fetch(`https://api.themoviedb.org/3/search/movie?api_key=${import.meta.env.VITE_API}&language=en-US&query=${keyword}&page=1`);

    if (response.ok) {
      loading = false;
      const jsonResponse = await response.json();
      // console.log(jsonResponse.results);
      return jsonResponse.results;
    }

  } catch(error) {
    loading = false;
    console.log(`Got an error: ${error}`)
  }
}

let removeFocus = () => {
  let submitBtn = document.getElementById('submit-btn');
  submitBtn.focus();
}

let formValidation = "";
const getResults = async () => {
  if (keywordLength && keywordLength < 3) {
    formValidation = "Please search with at least 3 characters";
    setTimeout( () => formValidation = '', 2000);
  } else if (keywordLength === 0) {
    formValidation = "The search was reset";
    searchResults = cloneSearchResults;
    setTimeout( () => formValidation = '', 2000);
    removeFocus();
    keyword = "";
  } 
  else {
    formValidation = `You searched for ${keyword}`;
    setTimeout( () => formValidation = '', 2000);
    searchResults = await getData();
    removeFocus();
    keyword = "";
  }
}

</script>

<section class="search__section">
  <div class="search__wrapper wrap">
    <form class="search__form" on:submit|preventDefault={getResults}>
      <label for="search" class="search__label">
        <div class="search__icon">
          <IconSearch />
        </div>
        <input bind:value={keyword} type="text" placeholder="Search movies" name="search" class="search" id="search" />
      </label>
      <button id="submit-btn" class="search__btn-container">
        <span class="btn btn--alt search__btn">Search -></span>
      </button>
    </form>

    <div class="validation-wrapper">
      {#if formValidation}
      <p class="validation" in:fly={{ y: -8, duration: 125 }} out:fly={{ y: -8, duration: 125 }}>{formValidation}</p>
      {/if}
    </div>

  </div>
</section>

<section class="results">
  <div class="results__wrapper wrap">
    <Title title={"Movies list"} />
    <div class="loading__wrapper">
      {#if loading}
      <Loading />
      {/if}
    </div>
    {#if searchResults}
    <div class="list">
      {#each searchResults as movie}
        {@const length = `${movie.vote_average}`.length}
        {#if movie.poster_path}
        <a sveltekit:prefetch href={`/movie/${movie.id}`} class="movie-wrapper">
          <div class="movie" style="background-image: url({'http://image.tmdb.org/t/p/w500' + movie.poster_path})" in:fly={{ y: -8, duration: 300 }}>
            {#if movie.vote_average != 0}
            <span class="rating">{movie.vote_average}{length < 3 ? '.0' : ''}</span>
            {/if}
          </div>
        </a>
        {/if}
      {/each}
    </div>
    {/if}
  </div>
</section>

<style>
.search__section {
  margin-top: 140px;
}
.search__form {
  display: grid;
  grid-template-columns: 1fr max-content;
}
.search__label {
  position: relative;
}
.search {
  height: 56px;
  border: none;
  background-color: var(--black);
  color: #fff;
  outline: none;
  padding-right: 24px;
  padding-bottom: 2px;
  padding-left: 56px;
  width: 100%;
  border-radius: 4px;
}
.search::placeholder {
  color: #fff;
}
.search__icon {
  position: absolute;
  top: 16px;
  left: 20px;
  cursor: pointer;
}
.search__btn-container {
  background-color: transparent;
  padding: 0;
  border: none;
  outline: none;
}
.search__btn {
  display: inline-block;
  border: none;
  cursor: pointer;
  margin-left: -8px;
  transform: translate3d(0,0,0);
  transition: 450ms;
}
.results {
  margin-top: 16px;
}
.list {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 20px;
  margin-top: 3px;
}
.movie {
  aspect-ratio: 2/3;
  background-size: cover;
  background-position: center;
  position: relative;
  border-radius: 8px;
  overflow: hidden;
  transform: translate3d(0,0,0);
  transition: 450ms;
}
.movie::after {
  content: '';
  background-color: rgba(0,0,0,.55);
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  opacity: 1;
  transition: 450ms;
}
.rating {
  color: #fff;
  font-size: 1.125rem;
  padding: 4px 12px;
  background: var(--gradient);
  border-radius: 4px;
  z-index: 1;
  margin-top: auto;
  position: absolute;
  top: 4px;
  right: 4px;
}
.loading__wrapper {
  margin-top: 3px;
  height: 10px;
}
.validation-wrapper {
  min-height: 40px;
  display: grid;
  align-items: center;
  margin-top: 8px;
}
.validation {
  color: #fff;
  padding: 8px 16px;
  background-color: var(--black);
  border-radius: 4px;
  width: max-content;
}

@media(max-width: 1100px) {
  .list {
    grid-template-columns: repeat(5, 1fr);
  }
}
@media(max-width: 900px) {
  .list {
    grid-template-columns: repeat(4, 1fr);
  }
}
@media(max-width: 700px) {
  .list {
    grid-template-columns: repeat(3, 1fr);
  }
}
@media(max-width: 500px) {
  .list {
    grid-template-columns: repeat(2, 1fr);
  }
  .search__form {
    grid-template-columns: 1fr;
    gap: 8px;
  }
  .search__btn {
    width: 100%;
    margin-left: unset;
  }
}
@media(max-width: 400px) {
  .list {
    grid-template-columns: 1fr;
  }
}

/* Prevent hover on mobile */
@media(hover: hover) {
  .search__btn-container:hover .search__btn {
    transform: translate3d(4px,0,0);
    transition: 125ms;
  }
  .movie-wrapper:hover .movie {
    transform: translate3d(0,-8px,0);
    transition: 125ms;
  }
  .movie-wrapper:hover .movie::after {
    opacity: 0;
    transition: 125ms;
  }
}
</style>