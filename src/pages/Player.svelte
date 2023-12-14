<script>
  import { onMount } from 'svelte';
  import { Link,navigate } from 'svelte-navigator'; // Import svelte-routing's useParams function
    import Swal from 'sweetalert2'

 const apiurl = 'https://voe.sx/api/file/list?key=Wr7fjmWTBp6EY0XGYJZwleaMJiJ2cuf21c3UvSpDd7GtPLAVnQTGiY9RNtwCyCbK';

  let videoDetails = null;
  let relatedVideos = [];
  let isLoading = true;
  
  export let id;
  // Log the id from fileCode
 
      async function fetchRelatedVideos() {
          isLoading = true;

          relatedVideos = [];
          const response = await fetch(`${apiurl}`);
          const data = await response.json();
          const totalPages = Math.ceil(data.result.total / 25);
          console.log(totalPages)
          const randomPage = Math.floor(Math.random() * totalPages) + 1;

          console.log(randomPage)
          const responseRandomPage = await fetch(`${apiurl}&page=${randomPage}&per_page=25`);
          const dataRandomPage = await responseRandomPage.json();
          relatedVideos =  dataRandomPage.result.data;
          isLoading = false;
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
   navigate("/player/" + id);
fetchRelatedVideos();
    
}
</script>

<style >
   a{
    color: black;
    text-decoration: none;
  }
</style>

  <div class="m-2" style="display:flex;justify-content:space-evenly;"> 
  <Link to="/" class="btn btn-primary text-white">Kembali</Link>
  <Link to="/request" class="btn btn-danger text-white">Request BOKEP</Link>
  </div>
  

  <!-- Loading State -->
  {#if isLoading}
    <div style="display: flex; justify-content: center;" class="m-2">
      <div class="spinner-border " role="status" style="width: 8rem; height: 8rem;color:#13157d"></div>
    </div>
  {:else}
    <div class="mt-3">
      <iframe src={`https://voe.sx/e/${id}`} width="100%" height="330px" frameborder="0" allowfullscreen allowscrolling="no"></iframe>
       <a href={`https://cuty.io/quick?token=3c2f8445e662326c2ebcd8d60&url=https://voe.sx/${id}`} target="_blank" 
       class="btn text-white"
       style="font-weight: bold;width:100%;
       background-color: #13157d;color:white
       "
       rel="noopener noreferrer">Download Bokep</a>
       <p style="font-size: 15px;color:red;
       text-align: center;font-weight: bold;
       ">Lewatin Iklan ouo.io kalo mau download bokep cuy</p>
    </div>
    <!-- RELATED -->
    
   <div id="related" class="row mt-4">
  <h5 style="font-weight: bold;margin:5px">Video Lainnya</h5>
  {#each relatedVideos as relatedVideo (relatedVideo.filecode)}
    <div class="col-lg-3 col-md-4 col-12">
      <div class="card p-2 m-2 shadow">
        <div on:click={()=>gotoplayer(relatedVideo.filecode)}>
          <img src={relatedVideo.thumbnails[5].url}
                style="width:100%;background-size:cover;
                border: none;
                " 
                >
          <p style="font-weight: bold; font-size: 16px">{relatedVideo.name}</p>
          <p style="font-weight: bold;font-size: 13px;">Uang: {relatedVideo.file_money}</p>
          <p>Di upload : {formatUploadDate(relatedVideo.uploaded)}</p>
          <div style="display:flex;justify-content:end">
                  <Link to={`/player/${relatedVideo.filecode}`}
                  class="btn"
                  style="background-color: #16188a;color:white"
                  >Nonton Bokep</Link>
            </div>
        </div>
      </div>
    </div>
  {/each}
</div>
  {/if}
