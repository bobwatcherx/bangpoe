<script>
  import { onMount } from 'svelte';
  import { navigate,Link } from 'svelte-navigator'; // Import svelte-routing's navigate function

  

  let posts = [];
  let currentPage = 1;
  let totalPages = 3; // Ganti sesuai dengan total halaman yang Anda dapatkan dari API
  let isLoading = true;
  let totalData = 0; // Inisialisasi variabel totalData

  async function fetchPosts(page) {
    const response = await fetch(`https://corsany-1-g0403094.deta.app/https://voe.sx/api/file/list?key=Wr7fjmWTBp6EY0XGYJZwleaMJiJ2cuf21c3UvSpDd7GtPLAVnQTGiY9RNtwCyCbK&page=${page}&per_page=35`);
    const data = await response.json();
    totalData = data.result.total; // Update totalData dengan nilai dari respons API
    return data.result.data;
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

<div class="mt-3">
  {#if isLoading}
    <div style="display: flex; justify-content: center;">
      <div class="spinner-border text-danger" role="status" style="width: 8rem; height: 8rem;"></div>
    </div>
  {:else}
    <div>
      <div style="display:flex;justify-content: space-between;"> 
      <h4 style="font-weight:bold">Bokep Indo Jepang</h4>
         <h6 style="color: red;font-weight: bold">{totalData} Video</h6> 
      </div>
      <div class="row">
        {#each posts as post (post.filecode)}
          <div class="col-lg-3 col-md-4 col-12">
            <div class="card  shadow" on:click={() => navigateToPlayer(post.filecode)}>
              <div class="p-2">
                <img src={post.thumbnails[5].url}
                style="width:100%;background-size:cover;
                border: none;
                " 
                >
                <p style="font-weight: bold; font-size: 16px">{post.name}</p>
                <p style="font-size: 16px">Diupload : {post.uploaded}</p>
                <p style="font-size: 14px">Uang : {post.file_money}</p>
                <div style="display:flex;justify-content:end">
                  <Link to={`/player/${post.filecode}`}
                  class="btn btn-primary"
                  >Nonton Bokep</Link>
                </div>
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
          {#each Array.from({ length: totalPages }, (_, i) => i + 1) as page}
            <button class="btn btn-danger" on:click={() => goToPage(page)} disabled={page === currentPage}>{page}</button>
          {/each}
          <button class="btn btn-danger" on:click={goToNextPage} disabled={currentPage === totalPages}>Next</button>
        </div>
      </div>
    </div>
  {/if}
</div>
