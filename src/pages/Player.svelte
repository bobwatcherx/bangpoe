<script>
  import { onMount } from 'svelte';
  import { Link,useParams } from 'svelte-navigator'; // Import svelte-routing's useParams function

  let videoDetails = null;
  let relatedVideos = [];
  let isLoading = true;
  
  const getcode = useParams();
  let fileCode = $getcode;
  // Log the id from fileCode

  async function fetchVideoDetails(fileCode) {
    const response = await fetch(`https://bobwatcherx-baruserverbokep.hf.space/detail/?file_code=${fileCode.id}`);
    const data = await response.json();
    return data;
  }

  async function fetchRelatedVideos() {
    const response = await fetch('https://bobwatcherx-baruserverbokep.hf.space/random_posts/?num_posts=35');
    const data = await response.json();
    return data.random_posts; // Mengambil array random_posts dari respons
  }

  onMount(async () => {
    try {
      videoDetails = await fetchVideoDetails(fileCode);
      relatedVideos = await fetchRelatedVideos();
    } catch (error) {
      console.error('Error fetching data:', error);
    } finally {
      isLoading = false;
    }
  });

  function formatUploadDate(dateString) {
    const options = { year: 'numeric', month: 'long', day: 'numeric' };
    const date = new Date(dateString);
    return date.toLocaleDateString('en-US', options);
  }

</script>

<style >
   a{
    color: black;
    text-decoration: none;
  }
</style>
<div class="container">

  <div class="mt-2"> 
  <Link to="/" class="btn btn-primary text-white">Kembali</Link>
  </div>

  {#if isLoading}
    <div 
    class="mt-4" 
    style="display: flex; justify-content: center;">
      <div class="spinner-border text-danger" role="status" style="width: 8rem; height: 8rem;"></div>
    </div>
  {:else if videoDetails}
    <div class="mt-3">
      <iframe src={`https://doods.pro/e/${videoDetails.file_code}`} width="100%" height="250px" frameborder="0" allowfullscreen></iframe>
      <p style="font-weight:bold">{videoDetails.title}</p>
      <p style="font-weight: bold;">Di tonton: {videoDetails.views}</p>
      <p>Di upload : {formatUploadDate(videoDetails.uploaded)}</p>
       <a href={videoDetails.download_url} target="_blank" 
       class="btn btn-primary text-white"
       style="font-weight: bold;width:100%"
       rel="noopener noreferrer">Download</a>
    </div>
  {:else}
    <p>Error fetching video details.</p>
  {/if}

  <!-- RELATED -->
  <div id="related" class="row mt-4">
    <h5 style="font-weight: bold;">Video Lainnya</h5>
    {#each relatedVideos as relatedVideo (relatedVideo.id)}
      <div class="col-lg-3 col-md-4 col-12">
        <div class="card p-2 m-2 shadow">
          <Link to={`/player/${relatedVideo.file_code}`}>
            <img src={relatedVideo.single_img} alt={relatedVideo.title} style="max-width: 100%;">
            <p style="font-weight: bold; font-size: 16px">{relatedVideo.title}</p>
      <p style="font-weight: bold;">Di tonton: {relatedVideo.views}</p>
      <p>Di upload : {formatUploadDate(relatedVideo.uploaded)}</p>
          </Link>
        </div>
      </div>
    {/each}
  </div>
</div>
