<script>
  import { onMount } from 'svelte';
  import { navigate } from 'svelte-navigator'; // Import svelte-routing's navigate function

  import {apiurl} from '../../configapi/api.js'


  let posts = [];
  let currentPage = 1;
  let totalPages = 167; // Ganti sesuai dengan total halaman yang Anda dapatkan dari API
  let isLoading = true;
  let totalData = 0; // Inisialisasi variabel totalData

  async function fetchPosts(page) {
    const response = await fetch(`${apiurl}/posts/?page=${page}&per_page=65`);
    const data = await response.json();
    totalData = data.total_data; // Update totalData dengan nilai dari respons API
    return data.data;
  }

  function updatePosts() {
    isLoading = true; // Set isLoading to true while fetching data
    fetchPosts(currentPage).then(newPosts => {
      posts = newPosts;
      isLoading = false; // Set isLoading to false when data is fetched
    });
  }

  onMount(() => {
    updatePosts();
  });

  function goToPage(page) {
    if (page >= 1 && page <= totalPages) {
      currentPage = page;
      updatePosts();
    }
  }

  function goToPreviousPage() {
    if (currentPage > 1) {
      currentPage--;
      updatePosts();
    }
  }

  function goToNextPage() {
    if (currentPage < totalPages) {
      currentPage++;
      updatePosts();
    }
  }

  // Function to handle card click
  function navigateToPlayer(fileCode) {
    navigate(`/player/${fileCode}`); // Redirect to /player/:file_code
  }
</script>




<div class="mt-4">
  {#if isLoading}
    <div style="display: flex; justify-content: center;">
      <div class="spinner-border text-danger" role="status" style="width: 8rem; height: 8rem;"></div>
    </div>
  {:else}
    <div>
      <div style="display:flex;justify-content: space-between;"> 
      <h4 style="font-weight:bold">Semua bokep</h4>
         <h6 style="color: red;font-weight: bold">{totalData} Video</h6> 
      </div>
      <div class="row">
        {#each posts as post (post.id)}
          <div class="col-lg-3 col-md-4 col-12">
            <div class="card  shadow" on:click={() => navigateToPlayer(post.file_code)}>
              <img src={post.single_img} alt={post.title} style="max-width: 100%;">
              <div class="p-2">
                <p style="font-weight: bold; font-size: 16px">{post.title}</p>
                <p style="font-size: 16px">Ditonton : {post.views}</p>
                <p style="font-size: 13px">Diupload : {post.uploaded}</p>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>

    <div class="container mt-3">
      <div class="row">
        <div class="col-12">
          <button class="btn btn-danger" on:click={goToPreviousPage} disabled={currentPage === 1}>Previous</button>
          {#each Array.from({ length: 3 }, (_, i) => currentPage - 1 + i) as page}
            <button class="btn btn-danger" on:click={() => goToPage(page)} disabled={page === currentPage}>{page}</button>
          {/each}
          <button class="btn btn-danger" on:click={goToNextPage} disabled={currentPage === totalPages}>Next</button>
        </div>
      </div>
    </div>
  {/if}
</div>
