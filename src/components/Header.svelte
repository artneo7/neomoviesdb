<header class="header">
  <div class="header__wrapper wrap">
    <a href="/" class="header__logo">NeoMoviesDB</a>
  </div>
</header>

<style>
.header__logo {
  font-size: 2rem;
  font-weight: bold;
  color: #fff;
  width: max-content;
  padding: .5rem;
  margin-left: -.5rem;
}

/* Add style only to desktop */
@media(hover: hover) {
  .header {
    position: fixed;
    z-index: 1;
    width: 100%;
    backdrop-filter: blur(8px);
  }
  .header__wrapper {
    height: 100px;
    background-color: rgba(0, 0, 0, .1);
    display: grid;
    align-items: center;
  }
}
/* Add style only to mobile */
@media(hover: none) {
  .header {
    position: fixed;
    z-index: 1;
    width: 100%;
    backdrop-filter: unset;
  }
  .header__wrapper {
    height: 80px;
    background-color: rgba(0, 0, 0, .75);
    display: grid;
    align-items: center;
  }
  .header__logo {
    font-size: 1.5rem;
  }
}
</style>