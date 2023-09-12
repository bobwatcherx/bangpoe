<script>
  import { onMount } from 'svelte';
  import { Link,useParams } from 'svelte-navigator'; // Import svelte-routing's useParams function
 import {apiurl} from '../configapi/api.js'

  let videoDetails = null;
  let relatedVideos = [];
  let isLoading = true;
  
  const getcode = useParams();
  let fileCode = $getcode;
  // Log the id from fileCode
  console.log(fileCode.id)
 

   async function fetchRelatedVideos() {
    const response = await fetch(`${apiurl}/random_posts/?num_posts=35`);
    const data = await response.json();
    return data.random_posts;
  }

  onMount(async () => {
    try {
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

  <div class="mt-2"> 
  <Link to="/" class="btn btn-primary text-white">Kembali</Link>
  </div>
  <div class="mt-3">
      <iframe src={`https://doods.pro/e/${fileCode.id}`} width="100%" height="300px" frameborder="0" allowfullscreen allowscrolling="no"></iframe>
       <a href={`http://ouo.io/qs/nvM0IVNA?s=https://doodstream.co/d/${fileCode.id}`} target="_blank" 
       class="btn btn-primary text-white"
       style="font-weight: bold;width:100%"
       rel="noopener noreferrer">Download</a>
    </div>

<div class="container">
  <!-- Loading State -->
  {#if isLoading}
    <div style="display: flex; justify-content: center;" class="m-2">
      <div class="spinner-border text-danger" role="status" style="width: 8rem; height: 8rem;"></div>
    </div>
  {:else}
    <!-- RELATED -->
    <div id="related" class="row mt-4">
      <h5 style="font-weight: bold;">Video Lainnya</h5>
      {#each relatedVideos as relatedVideo (relatedVideo.id)}
        <div class="col-lg-3 col-md-4 col-12">
          <div class="card p-2 m-2 shadow">
            <a href={`/player/${relatedVideo.file_code}`}>
              <img src={relatedVideo.single_img} alt={relatedVideo.title} style="max-width: 100%;">
              <p style="font-weight: bold; font-size: 16px">{relatedVideo.title}</p>
              <p style="font-weight: bold;">Di tonton: {relatedVideo.views}</p>
              <p>Di upload : {formatUploadDate(relatedVideo.uploaded)}</p>
            </a>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>
