<script>
import { Star } from 'lucide-vue-next';
import Button from '../buttons/Button.vue';
import { getFirestore, collection, addDoc } from 'firebase/firestore';
import { useReviewsFirestore } from '../../../../stores/reviewsFirestore.js'
import { useUserStore } from '../../../../stores/userStore.js';




export default {
    
    setup() {
        
        const useReviews = useReviewsFirestore();
        const useUser = useUserStore();
return{
  useReviews,
  useUser
}
  },

  components: {
    Button,
    Star
  },
  props: {
    serieId:{
        type: String,
        required:true,
        },
    nombreSerie: {
        type: String,
        required:true,
    },
    srcImgProp: {
        type: String,
        required:true,
    }
  },
  data() {
    return {
      id:'',
      date: '',
      review: '',
      rating: '',
      userEmail: '',  
      username: '',
      imgSrc:'',
      serieName:'',
      objetoReview:{
        serieNombre:'',
        fecha: '',
        comentario: '',
        rate: '',
        idSerie: '',
        emailUsuario: '',
        srcImg:''
      },
   
}
  },
  methods: {
    getCurrentData() {
      this.date = new Date().toLocaleDateString();
    },
    async submitReview() {
      this.objetoReview = {

        serieNombre : this.nombreSerie,
        fecha : new Date().toLocaleDateString(),
        comentario : this.review,
        rate : Number(this.rating),
        idSerie : Number(this.serieId),
        emailUsuario : this.useUser.isLoggedIn ? this.useUser.userEmail : 'Invitado',
        srcImg:this.srcImgProp,
    }

      console.log(this.objetoReview);
      
      try{
        await this.useReviews.createReview({objetoReview :this.objetoReview})
        // Limpiar el formulario después de enviar
        this.review = '';
        this.rating = '';
        console.log('review enviada');
        
      }catch{
        console.log('nuevamente un ERROR');
        
      }
    },
  },


  mounted() {
    this.serieNombre = this.serieName
    this.getCurrentData()
    this.id = this.serieId
    this.imgSrc = this.srcImgProp
    
    
    
  }
};
</script>

<template>
  <div v-if="useUser.isLoggedIn" class="w-full flex flex-col justify-center items-center px-5 lg:px-20 my-3">
    <div class="w-full bg-secondaryBackground  justify-center rounded-2xl px-5">
      
      <div class="flex md:flex-row flex-col md:justify-between justify-center items-center gap-2  mt-8 ">

        <!-- nota -->
        <div class="flex items-center ">
          <div class="flex gap-x-2 items-center">
            <select v-model="rating" name="" id="" class="bg-mainBackground px-2 py-2">
              <option value="">Nota</option>
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
              <option value="6">6</option>
              <option value="7">7</option>
              <option value="8">8</option>
              <option value="9">9</option>
              <option value="10">10</option>
            </select>
            <span>/10</span><img src="" alt="">
          </div>
          <button><Star color="#dcd62e" strokeWidth={3} fill="#dcd62e" /></button>
        </div>

        <!-- mail -->
        <span v-if="useUser.isLoggedIn" class="text-primary ">{{useUser.userEmail}} </span>


        <!-- usuario -->
        <span v-else class="text-purple-300" >invitad@</span>

        <!-- fecha -->
        <span class="text-center  ">{{ date }}</span>

      </div>

      <textarea v-model="review" class="w-full mt-10 mb-5  rounded bg-mainBackground h-40 text-gray-100 pt-3 pl-3" name="" id="" placeholder="Escribe tu review"></textarea>
      <div class="col-span-3 flex justify-center mb-5"><Button @click="submitReview( { serieId })" buttonText="Enviar"></Button></div>
    
    </div>
  </div>
</template>

<style scoped>
/* tus estilos aquí */
</style>