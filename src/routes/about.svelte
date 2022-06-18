<script>
import { MetaTags } from "svelte-meta-tags";
import Title from "../components/Title.svelte";
</script>

<MetaTags
  title = "About"
  titleTemplate="%s | NeoMoviesDB"
  noindex
  nofollow
/>

<main>
  <section class="intro">
    <div class="intro__wrapper">
      <div class="intro__title-wrapper wrap">
        <Title title={"About"} />
      </div>
    </div>
  </section>

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