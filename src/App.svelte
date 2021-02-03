<script>

  import { fade } from 'svelte/transition';

  export let itemDeletePanelIndex = undefined;
  export let newItemDescription = '';
  export let items = [
    {description: 'Take out garbage'},
    {description: 'Feed cat'}
  ];

  function itemCreate(description) {
    
    if (!newItemDescription) return false;
    
    let a, b;
    
    let newItem = {
      id: items.length,
      description
    }
    items = [...items, newItem];
    newItemDescription = '';
  }
  
  function itemDeleteSelect(index) {
    console.log('itemDeleteSelect()');
    if (itemDeletePanelIndex === index) {
      itemDeletePanelIndex = undefined;
    } else {
      itemDeletePanelIndex = index;
    }
  }
  
  const itemDelete = (index) => {
    console.log(index)
    items.splice(index, 1);
    items = items;
  }
</script>

<main>
  <h2>To-Do List</h2>

  <input type="text"
         bind:value="{newItemDescription}"
         on:keyup="{e => e.key === 'Enter' && itemCreate(newItemDescription)}">
  <button on:click="{() => itemCreate(newItemDescription)}">
    Create new item
  </button>

  {#if items.length}
  <ol>
    {#each items as item, index}
      <li transition:fade class="cursor-url">
        <span class="list-item-description"
              on:click="{() => itemDeleteSelect(index)}">
          {item.description}
        </span>
        
      {#if itemDeletePanelIndex === index}
        <span on:click="{() => itemDelete(index)}">&#x1f5d1</span>
      {/if}
        
    </li>
    {/each}
  </ol>
  {/if}

</main>

<style>
.list-item-description {
	font-weight: bold;
}
	
.cursor-url {
	cursor: pointer;
}

@media (min-width: 640px) {
	main {
		max-width: none;
	}
}

</style>
