<!-- NOTE: demo https://codepen.io/robertcooper_rc/pen/XeabLa -->
<script>
  import { onMount } from 'svelte'; 
  import lodash from 'lodash'; 

  export let pages = 0;
  export let currentPage = 0;
  export let onChange = n => n;

  // let active = 1;
  let pageCutLow = currentPage - 1;
  let pageCutHigh = currentPage + 1;

  const createPagination = page =>  {
    currentPage = page;
    pageCutLow = page - 1;
    pageCutHigh = page + 1;

    // pages to show after the current page index
    if (pages >= 6) {
      if (page === 1) {
        // pageCutHigh = pageCutHigh + 2;
      } else if (page === 2) {
        pageCutHigh = pageCutHigh + 1;
      }
      // pages to show before the current page index
      if (page === pages) {
        pageCutLow = pageCutLow -2;
      } else if (page === pages-1) {
        pageCutLow = pageCutLow - 1;
      }
    }

    onChange(currentPage - 1);
  };

//   $: console.log('currentPage:', currentPage);
//   $: console.log('pageCutLow:', pageCutLow);
//   $: console.log('pageCutHigh:', pageCutHigh);

  onMount(() => {
    createPagination(1);
  });
</script>


<style>
  .pag-pagination {
    display: flex;
    justify-content: center;
    border: 1px solid var(--prime);
    border: 1px solid #a9a9a9;
    border-radius: 4px;
    background-color: #fff;
  }

  ul {
    display: flex;
    justify-content: center;
    align-items: stretch;
    margin: 0;
    padding: 0;
    list-style: none;
  }

  ul li {
    display: flex;
    align-items: center;
    margin-top: 0px;
    padding: 5px 10px;
    color: #777;
    cursor: pointer;
    transition: all 0.2;
  }

  .pag-next-previous {
    /*background-color: #f2f2f2;*/
  }

  .pag-center-pagination li {
    /*background-color: #f2f2f2;*/
  }

  .pag-next-previous,
  .pag-center-pagination li:last-child {
    /*border-right: yellow;*/
  }

  ul li.pag-bc-active {
    background-color: var(--prime);
    color: #fff;
  }

  .pag-inactive {
    opacity: 0.4;
    cursor: auto !important;
  }

  .pag-page-item {
    transition: all 0.2;
  }

  @media (hover: hover) {
    .pag-page-item:hover {
      transition: all 0.2;
      opacity: 0.8;
      background-color: var(--prime);
      color: var(--prime-contrast);
    }

    .pag-next-previous-hover:hover {
      background-color: var(--prime);
      color: var(--prime-contrast);
    }
  }
</style>


<div class="pag-pagination">
  <!-- Previous button - active and deactiv -->
  <ul>
    {#if currentPage > 1}
      <li 
        class="pag-next-previous pag-next-previous-hover pag-page-item"
        on:click={() => createPagination(currentPage - 1)}
      >
        <i class="fas fa-chevron-left"></i>
      </li>
    {:else}
      <li class="pag-next-previous pag-inactive">
        <i class="fas fa-chevron-left"></i>
      </li>
    {/if}
  </ul>

  <ul class="pag-center-pagination">
    <!-- Show all if there is < 6 pages -->
    {#if pages < 9}
      <ul>
        {#each lodash.range(1, pages + 1) as item}
          <li 
            class="pag-page-item" 
            class:pag-bc-active={currentPage == item}
            on:click={() => createPagination(item)}
          >
            {item}
          </li>
        {/each}
      </ul>

    {:else}
      <!-- Show the very first page -->
      {#if currentPage > 2}
        <li 
          class="pag-page-item" 
          class:pag-bc-active={currentPage == 1}
          on:click={() => createPagination(1)}
        >
          1
        </li>
         <!-- "..." to collapse pages outside of a certain range -->
        {#if currentPage > 3}
          <li> ... </li>
        {/if}
      {/if}

      <!-- Output the indexes for pages that fall inside the range of pageCutLow -->
      {#each lodash.range(pageCutLow, pageCutHigh + 1) as item}
        {#if item <= pages && item > 0}
          <li 
            class="pag-page-item" 
            class:pag-bc-active={currentPage === item}
            on:click={() => createPagination(item)}
          >
            {item}
          </li>
        {/if}
      {/each}

      <!-- Show the very last page preceded by a "..." at the end of the pagination -->
      {#if currentPage <= pages - 2}
        {#if currentPage < pages - 2}
          <li>...</li>
        {/if}
        <li 
          class="pag-page-item"  
          on:click={() => createPagination(pages)}
        >
          {pages}
        </li>
      {/if}
    {/if} 
  </ul>

  <!-- next button - hide if activ is a last -->
  <ul>
    {#if currentPage < pages}
      <li
        class="pag-next-previous  pag-next-previous-hover pag-page-item "
        on:click={() => createPagination(currentPage + 1)}
      >
        <i class="fas fa-chevron-right"></i>
      </li>
    {:else}
      <li class="pag-next-previous pag-inactive">
        <i class="fas fa-chevron-right"></i>
      </li>
    {/if}
  </ul>
</div>
