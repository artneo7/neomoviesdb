<script>
  export let title = "Title";
</script>

<h1 class="title">(>) {title}</h1>

<style>
  .title {
    font-size: 1.5rem;
    color: #fff;
  }
  .title::after {
    content: '';
    display: block;
    width: 50px;
    height: 4px;
    background-color: var(--feat);
    margin-top: 8px;
  }
</style>