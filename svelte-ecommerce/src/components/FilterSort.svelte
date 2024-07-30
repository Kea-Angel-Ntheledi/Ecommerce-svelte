<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let filters = [];
  export let sortOptions = [];
  export let selectedFilter = '';
  export let selectedSort = '';

  function handleFilterChange(event) {
    selectedFilter = event.target.value;
    dispatch('filterChange', selectedFilter);
  }

  function handleSortChange(event) {
    selectedSort = event.target.value;
    dispatch('sortChange', selectedSort);
  }
</script>

<div class="filter-sort-container">
  <div class="filter-container">
    <label for="filter">Filter:</label>
    <select id="filter" bind:value={selectedFilter} on:change={handleFilterChange}>
      <option value="">All</option>
      {#each filters as filter}
        <option value={filter}>{filter}</option>
      {/each}
    </select>
  </div>

  <div class="sort-container">
    <label for="sort">Sort by:</label>
    <select id="sort" bind:value={selectedSort} on:change={handleSortChange}>
      {#each sortOptions as sortOption}
        <option value={sortOption.value}>{sortOption.label}</option>
      {/each}
    </select>
  </div>
</div>

<style>
  .filter-sort-container {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
  }

  .filter-container,
  .sort-container {
    display: flex;
    align-items: center;
  }

  label {
    margin-right: 0.5rem;
  }

  select {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
</style>
