<script >
import { onMounted } from 'vue'; 
import { useRoute } from 'vue-router'; 
import AllReviews from '../principales/reviews/AllReviews.vue';
import FormReviews from '../principales/reviews/FormReviews.vue';
import TopReview from '../principales/reviews/TopReview.vue';
import LoadingSpinner from '../principales/widgets/LoadingSpinner.vue'


export default {

 data(){
    return{
        reviewId: null, // Almacena el ID recibido por URL
        reviewData: {}, // Almacena los datos de la API
        loading: true, // Estado de carga
        error: null,
        pathBaseSrcImg:'https://image.tmdb.org/t/p/w200/',
        
    }

},

    components:{

        AllReviews,
        TopReview,
        FormReviews,
        LoadingSpinner,
      
    //   Footer,
    },
    methods:{
        async fetchReviewDetails() {
      try {
        const API_KEY = 'de4b6461e071a300a503b91fb095113d';
        const LANGUAGE = 'es-ES'; // Idioma español de España
        const DB_ENDPOINT = `https://api.themoviedb.org/3/tv/${this.reviewId}?api_key=${API_KEY}&language=${LANGUAGE}`;
        const response = await fetch(DB_ENDPOINT);
        const data = await response.json();
        this.reviewData = data;
        this.loading = false;
      } catch (error) {
        console.error('Error fetching review details:', error);
        this.loading = false;
      }
    }
    },
    mounted() {

        // Obtener el parámetro de la URL
        const route = useRoute(); 
        this.reviewId = route.params.id; 
       
        // Asignar el ID recibido a la propiedad `reviewId`
        this.fetchReviewDetails(); // Llamar a la API

    },
    
    watch: {
    '$route.params.id': {
      handler(newId) {
        // Cuando el ID cambie, vuelve a cargar los datos
        this.reviewId = newId;
        this.fetchReviewDetails();
      },
      immediate: true, // Ejecuta el watcher de inmediato cuando el componente se monta
    },
  },
    
}



</script>
<template>
    
    <div class=" min-h-[calc(100vh-14rem)] flex flex-col gap-4 justify-start align-middle w-full ">
<div class="flex flex-col">
    <div v-if="loading !=true">
        <TopReview 
        :idSerie="reviewId" 
        :nombreSerie="reviewData.name" 
        :srcImg="pathBaseSrcImg+reviewData.poster_path"
        :popularidad="reviewData.popularity"
        :fechaEmision="reviewData.first_air_date"
        :descripcionSerie="reviewData.overview"
        :ranking="reviewData.vote_average"
        />
    </div>
    <!-- Contenedor LOADING -->
    <div v-else class="flex flex-row justify-center h-96 items-center ">
        <div class="flex flex-col justify-center  h-32 ">
            <LoadingSpinner/>
        </div>
        
    </div>

    
    <FormReviews 
   
    :serieId="reviewId" 
    :nombreSerie="reviewData.name" 
    :srcImgProp="pathBaseSrcImg+reviewData.poster_path"
     />
    <AllReviews 
    class="pb-10"
   :serieId="String(reviewId)"
    />
</div>
    </div>
  
</template>