<script>
// Check page slug
import { page } from '$app/stores';
let slug;
$: slug = $page.routeId;
</script>

<header class="header">
  <div class="header__wrapper wrap">
    <a href="/" class="header__logo">NeoMoviesDB</a>
    <a href="/about" class={`header__link ${slug === 'about' ? ' header__link--active' : ''}`}>About</a>
  </div>
</header>

<style>
.header {
  position: fixed;
  z-index: 1;
  width: 100%;
}
.header__wrapper {
  height: 100px;
  background-color: rgba(0, 0, 0, .1);
  display: flex;
  align-items: center;
}
.header__logo {
  font-size: 2rem;
  font-weight: bold;
  color: #fff;
  width: max-content;
  padding: .5rem;
  margin-left: -.5rem;
}
.header__link {
  margin-left: auto;
  padding: 20px;
  color: #fff;
  font-weight: bold;
}
.header__link--active {
  text-decoration: underline;
  text-underline-offset: 4px;
  text-decoration-thickness: 2px;
}

/* Add style only to desktop */
@media(hover: hover) {
  .header {
    backdrop-filter: blur(8px);
  }
  .header__link:hover {
    text-decoration: underline;
    text-underline-offset: 4px;
    text-decoration-thickness: 2px;
  }
}
/* Add style only to mobile */
@media(hover: none) {
  .header {
    backdrop-filter: unset;
  }
  .header__wrapper {
    height: 80px;
    background-color: rgba(0, 0, 0, .75);
  }
  .header__logo {
    font-size: 1.5rem;
  }
}
</style>