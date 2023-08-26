<script>
  import {Link} from 'svelte-navigator'

  import Semua from '../lib/components/Semua.svelte';
  import Jepang from '../lib/components/Jepang.svelte';
  import Barat from '../lib/components/Barat.svelte';
  import Indo from '../lib/components/Indo.svelte';
  import Semi from '../lib/components/Semi.svelte';
  import Anal from '../lib/components/Anal.svelte';
  import Arab from '../lib/components/Arab.svelte';
  import Pijat from '../lib/components/Pijat.svelte';
  import Permintaan from '../lib/components/Permintaan.svelte';

  let activeTabIndex = 0;

  const tabs = [
    { label: 'Semua ', component: Semua },
    { label: 'Jepang', component: Jepang },
    { label: 'Barat', component: Barat },
    { label: 'Indo', component: Indo },
    { label: 'Semi', component: Semi },
    { label: 'Anal', component: Anal },
    { label: 'Pijat', component: Pijat },
    { label: 'Muslim', component: Arab },
    { label: 'Request Kalian', component: Permintaan },
  ];

  function setActiveTab(index) {
    activeTabIndex = index;
  }
</script>

<style>
  .tabs-container {
    display: flex;
    overflow-x: auto;
    white-space: nowrap;
  }

  .tab {
    padding: 10px 20px;
    cursor: pointer;
    border: 1px solid #ccc;
    border-bottom: none;
  }

  .tab.active {
    background-color: #ccc;
  }
</style>
  

 <div class="mt-3">  
  <Link to="/request" class="text-red"
  style="text-decoration: none;"
  >Request Bokep Disini</Link>
  <h5 style="font-weight: bold;">Pilih Genre Bokep</h5>
<div class="tabs-container ">
  {#each tabs as tab, index}
    <div
      class="tab {activeTabIndex === index && 'active'}"
      on:click={() => setActiveTab(index)}
    >
      {tab.label}
    </div>
  {/each}
</div>

{#each tabs as tab, index}
  {#if activeTabIndex === index}
    <div class="m-2 mt-4">
      <svelte:component this={tab.component} />
    </div>
  {/if}
{/each}
</div>