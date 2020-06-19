<script>
  import Pagination from "./Pagination.svelte";

  let items = [
    "Item 1",
    "Item 2",
    "Item 3",
    "Item 4",
    "Item 5",
    "Item 6",
    "Item 7",
    "Item 8",
    "Item 9",
    "Item 10",
    "Item 11",
    "Item 12",
    "Item 13",
    "Item 14",
    "Item 15",
    "Item 16",
    "Item 17",
    "Item 18",
    "Item 19",
    "Item 20",
    "Item 21",
    "Item 22",
    "Item 23",
    "Item 24",
    "Item 25",
    "Item 26",
    "Item 27",
    "Item 28",
    "Item 29",
    "Item 30"
  ];

  let current_page = 1;
  let page = 0;
  let rows_per_page = 10;
  let rows = 5;
  let start = 0;
  // let loop = start - 1
  let arr = [];

  $: if(current_page === 1) {
    page = 0
  } else {
    page = current_page - 1
  }
  $: arrF = (() => {
    // let loop_start = current_page * rows
    let newArr = items.slice(page * rows, current_page * rows);
    arr = newArr;
  })();

  $: totalPages = items.length / 5;

  // const nextPage =
  $: console.log("curr page",current_page);
  $: console.log("page",page);
</script>

<style>

</style>

<h1>stefanki</h1>

{#each arr as item}{item}{/each}
<br />
<!-- {totalPages} -->
<!-- <br>
{#each totalPages as page}
   {page}
{/each} -->

<h1>dasdasd</h1>
<Pagination pages={totalPages} bind:currentPage={current_page} />
