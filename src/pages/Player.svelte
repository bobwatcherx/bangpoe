<script>
  import { onMount } from 'svelte';
  import { Link,navigate } from 'svelte-navigator'; // Import svelte-routing's useParams function
    import Swal from 'sweetalert2'

 import {apiurl} from '../configapi/api.js'
let directlink = "https://www.highcpmrevenuegate.com/w49w9gey2?key=2722eb58e1097c6545b889a3a048ef08"


  let videoDetails = null;
  let relatedVideos = [];
  let isLoading = true;
  
  export let id;
  // Log the id from fileCode
  console.log(id)
 

   async function fetchRelatedVideos() {
  isLoading = true

    relatedVideos = [];
    const response = await fetch(`${apiurl}/random_posts/?num_posts=35`);
    const data = await response.json();
    relatedVideos =  data.random_posts;
  isLoading = false

  }

  onMount(async () => {
    try {
      await fetchRelatedVideos()
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

function gotoplayer(myid){
  id = myid;
    const choices = [1,2,3,4] 
  const randomChoice = choices[Math.floor(Math.random() * choices.length)];
  console.log(randomChoice)
  if (randomChoice === 3) {
    Swal.fire({
        icon: 'success',
        title: 'Kamu Iklan Dulu Bos',
        text: 'Sesaat lagi kamu akan iklan dulu......',
        footer: 'kami bisa kembali lagi ke web ini setelah iklan..'
      })
    setTimeout(() => {
        navigate(directlink);
      }, 3000);
  } else {
    
    navigate("/player/" + id);
  }

    fetchRelatedVideos();
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
  

  <!-- Loading State -->
  {#if isLoading}
    <div style="display: flex; justify-content: center;" class="m-2">
      <div class="spinner-border text-danger" role="status" style="width: 8rem; height: 8rem;"></div>
    </div>
  {:else}
    <div class="mt-3">
      <iframe src={`https://ds2play.com/e/${id}`} width="100%" height="300px" frameborder="0" allowfullscreen allowscrolling="no"></iframe>
       <a href={`http://ouo.io/qs/nvM0IVNA?s=https://doodstream.co/d/${id}`} target="_blank" 
       class="btn btn-primary text-white"
       style="font-weight: bold;width:100%"
       rel="noopener noreferrer">Download</a>
    </div>
    <!-- RELATED -->
    
    <div id="related" class="row mt-4">
      <h5 style="font-weight: bold;">Video Lainnya</h5>
      {#each relatedVideos as relatedVideo (relatedVideo.id)}
        <div class="col-lg-3 col-md-4 col-12">
          <div class="card p-2 m-2 shadow">
            <div on:click={()=>gotoplayer(relatedVideo.file_code)}>
              <img src={relatedVideo.single_img} alt={relatedVideo.title} style="max-width: 100%;">
              <p style="font-weight: bold; font-size: 16px">{relatedVideo.title}</p>
              <p style="font-weight: bold;">Di tonton: {relatedVideo.views}</p>
              <p>Di upload : {formatUploadDate(relatedVideo.uploaded)}</p>
            </div>
          </div>
        </div>
      {/each}
    </div>
  {/if}
