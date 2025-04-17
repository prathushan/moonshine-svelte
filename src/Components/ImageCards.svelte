<script>
  import { onMount } from 'svelte';
  import { client } from '$lib/sanityClient';
  import { imageCardsQuery } from '$lib/queries/imageCards';

  let section = null;

  onMount(async () => {
    try {
      section = await client.fetch(imageCardsQuery);
    } catch (err) {
      console.error("Failed to fetch cards:", err);
    }
  });
</script>

{#if section}
  <section class="image-cards">
    <div class="title">
      {#each section.title as block}
        {#if block._type === 'block'}
          {#each block.children as span}
            {#if span.marks && span.marks.includes('em')}
              <em>{span.text}</em>
            {:else}
              {@html span.text}
            {/if}
          {/each}
        {/if}
      {/each}
    </div>

    <div class="cards-container">
      {#each section.cards as card}
        <div class="card" style="background-color: {card.backgroundColor}">
          <img src={card.image.asset.url} alt={card.heading} />
          <h3>{card.heading}</h3>
          <p>{card.text}</p>
        </div>
      {/each}
    </div>
  </section>
{/if}

<style>
  .title {
    font-family: 'Inter';
    font-weight: 500;
    font-size: 80px;
    line-height: 76px;
    text-align: center;
    margin: 0 auto;
  }

  em {
    display: block;
    font-family: 'Covered By Your Grace';
    color: #009387;
    font-size:70px;
    font-weight:400;
    line-height:100px;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
    gap: 1.5rem;
  }

  .card {
    flex: 1 1 400px;
    padding: 1.5rem;
    border-radius: 1rem;
  }

  img {
    max-width: 100%;
    height: auto;
    margin-bottom: 1rem;
  }
</style>
