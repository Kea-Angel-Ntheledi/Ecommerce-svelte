<script>
  import { onMount } from 'svelte';
  import LoadingState from '../components/LoadingState.svelte';
  import Error from '../components/Error.svelte';

  export let id;

  let product = null;
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      const response = await fetch(`https://fakestoreapi.com/products/${id}`);
      if (!response.ok) {
        throw new Error('Failed to fetch product details');
      }
      product = await response.json();
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });
</script>

<style>
  .product-detail {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  img {
    max-width: 100%;
    height: auto;
  }

  .product-info {
    margin-top: 2rem;
    text-align: left;
    max-width: 600px;
  }

  @media (min-width: 768px) {
    .product-detail {
      flex-direction: row;
      align-items: flex-start;
    }

    .product-info {
      margin-left: 2rem;
      text-align: left;
    }
  }
</style>

<main>
  {#if loading}
    <LoadingState />
  {:else if error}
    <Error message={error} />
  {:else if product}
    <div class="mt-6 sm:mt-8 lg:flex lg:items-start lg:max-w-6xl xl:max-w-7xl product-detail">
      <div class="mx-auto w-2/5 flex-none">
        <img src={product.image} alt={product.title} class="w-[90%] h-[90%]" />
      </div>
      <div class="mx-auto w-[90%] space-y-2 product-info">
        <h1 class="text-2xl md:text-4xl lg:text-4xl font-bold">{product.title}</h1>
        {#if product.rating}
          <div class="flex gap-2">
            <svg
              class="w-4 h-4 text-yellow-300 ms-1"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              viewBox="0 0 22 20"
            >
              <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
            </svg>
            <div>{product.rating.rate}</div>
            <div>Reviews: {product.rating.count}</div>
          </div>
        {/if}

        <span
          class="inline-flex items-center rounded-md bg-gray-50 px-2 py-1 text-xs font-medium text-gray-600 ring-1 ring-inset ring-gray-500/10"
        >
          {product.category}
        </span>

        <h3 class="text-xl md:text-2xl lg:text-2xl font-bold">${product.price}</h3>
        <button class="bg-cyan-700 hover:bg-cyan-900 w-[90%] md:w-[14rem] lg:w-[14rem] text-white font-bold py-2 px-4 rounded">
          Add To Cart
        </button>
        <h2 class="text-lg font-bold">Description</h2>
        <p>{product.description}</p>
      </div>
    </div>
  {:else}
    <p>No product found.</p>
  {/if}
</main>
