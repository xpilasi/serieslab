<script >
//Aquí importamos el componente Title

import CardUserLastReviews from '../cards/CardUserLastReviews.vue';
import { useReviewsFirestore } from '../../../../stores/reviewsFirestore';
import { RouterLink } from 'vue-router';


export default {

  //Aquí llamamos a todos los componentes
    components:{
      CardUserLastReviews
    },
    props:{
      arrayFiltrado:{
        type: Array
      }
    },
    data(){
        return{
       
        pathUrl: '/review-serie/',
        reviewId : null,
        }
    },

    watch: {
    arrayFiltrado: {
        handler(newVal) {
            console.log('arrayFiltrado actualizado:', newVal);
        },
        immediate: true
    },
   
    }
}
</script>

<template>
  <div class="last-reviews-container flex flex-col w-full">
    <h3 class="pb-5 text-md">Últimas reviews</h3>
    <div class="flex flex-col gap-y-5 rounded-2xl">
      <RouterLink v-for="r in arrayFiltrado" :to="pathUrl +r.serieId">
        <CardUserLastReviews
          :key="r.id"
          :totalSeries="r.length"
          :idSerie="r.serieId"
          :nombreSerie="r.serieName"
          :srcImg="r.srcImage"
          :rating="r.rating"
          :descripcionSerie="r.comment" 
          />
        </RouterLink>

      
    </div>
  </div>

</template>
