<script>
import { MetaTags } from "svelte-meta-tags";
import Logo from "../components/icons/Logo.svelte";
import Title from "../components/Title.svelte";
</script>

<MetaTags
  title = "About"
  titleTemplate="%s | NeoMoviesDB"
  description="Personal project to learn about Svelte.js"
  noindex
  nofollow
  openGraph={{
    title: 'About',
    description: 'Personal project to learn about Svelte.js',
    images: [
      { url: '/images/default-image.jpg' }
    ],
    site_name: 'NeoMoviesDB'
  }}
/>

<main>
  <section class="about">
    <div class="about__wrapper">
      <div class="about__title-wrapper wrap">
        <Title title={"About"} />
      </div>
      <div class="about__content-wrapper">
        <div class="about__content">
          <div class="logo__wrapper">
            <Logo />
          </div>
          <p class="about__text">This site uses the TMDB API but is not endorsed or certified by TMDB.</p>
        </div>
      </div>
    </div>
  </section>

</main>

<style>
.about {
  height: 460px;
  padding-top: 142px;
  background: linear-gradient(rgba(0,0,0,.55),rgba(0,0,0,.55)), url('/images/default-image.jpg');
  background-position: top;
  background-size: cover;
}
.about__content-wrapper {
  padding: 0 20px;
  margin: 16px auto 0;
}
.about__content {
  padding: 32px;
  background: var(--gradient);
  border-radius: 16px;
  width: fit-content;
}
.logo__wrapper {
  max-width: 80px;
}
.about__text {
  color: #fff;
  max-width: 680px;
  margin-top: 24px;
}
</style>