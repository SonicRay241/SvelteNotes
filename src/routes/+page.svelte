<script>
    import BookCover from "$lib/components/BookCover.svelte";
    const hexRe = new RegExp(/^([0-9, a-f]{1,6})$/g)
</script>

<main>
    <h1>My Notes</h1>
    <p>{hexRe.test("ffffff")}</p>
    <div class="grid-container">
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
        <BookCover title={"A mimir"} thumbname={"Amogus"} color={"125,10,27"}/>
        <BookCover title={"A mimir"} thumbname={"A"} color={"fff"}/>
    </div>
</main>

<style>
    main {
        padding-left: 15rem;
        padding-right: 15rem;
        padding-top: 1rem;
    }
    .grid-container {
      /**
       * User input values.
       */
      --grid-layout-gap: 0.5rem;
      --grid-column-count: 5;
      --grid-item--min-width: 10rem;

      /**
       * Calculated values.
       */
      --gap-count: calc(var(--grid-column-count) - 1);
      --total-gap-width: calc(var(--gap-count) * var(--grid-layout-gap));
      --grid-item--max-width: calc((100% - var(--total-gap-width)) / var(--grid-column-count));

      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(max(var(--grid-item--min-width), var(--grid-item--max-width)), 1fr));
      grid-gap: var(--grid-layout-gap);
    }
</style>