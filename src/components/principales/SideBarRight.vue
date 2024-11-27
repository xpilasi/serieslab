<script >

import LastReviewsContainer from './listas/LastReviewsContainer.vue';
import UserLastReviewsContainer from './listas/UserLastReviewsContainer.vue';
import Login from './widgets/Login.vue';
import SearchBar from './widgets/SearchBar.vue';
import { useReviewsFirestore } from '../../../stores/reviewsFirestore';
import { useUserStore } from '../../../stores/userStore'
import { useLoginState } from '../../../stores/stateWidgetsStore';
import AllReviews from './reviews/AllReviews.vue';

export default {

 //Para llamar al nombre del usuario
 setup() {
    const userStore = useUserStore();
    const showLogIn = useLoginState();
    const useReviews = useReviewsFirestore()

    return {
      userStore, // Accediendo al store completo
      showLogIn,
      useReviews,
    };
  },
    data(){
        return{
            // useReviews : useReviewsFirestore(),
            useUser : useUserStore(),
            useTheReviews : useReviewsFirestore(),
            arrayFiltradoUser : [],
            allReviewsFirestore:[]
        }
        

    },
    methods:{

     
    },
    async created() {
        try {
            await this.useReviews.readReviews();
            this.arrayFiltradoUser = [...this.useReviews.filterReviewsByUser(this.useUser.userEmail)];
            console.log(this.arrayFiltradoUser);
            this.allReviewsFirestore = [...this.useTheReviews.reviewsFirestore]
            console.log(this.allReviewsFirestore);
            
        } catch (e) {
            console.error('Error al leer reviews:', e);
        }
        },

    components:{
    
        LastReviewsContainer,
        UserLastReviewsContainer,
        SearchBar,
        Login

    },
    watch: {
        'useReviews.reviews': {
            handler(newReviews) {
            // Filtra y actualiza el array local
            this.arrayFiltradoUser = [...this.useReviews.filterReviewsByUser(this.useUser.userEmail)];
            },
            deep: true // Observa cambios profundos en el array del store
  }
},
    computed: {
    arrayFiltradoUser() {
        return this.useReviews.filterReviewsByUser(this.useUser.userEmail);
    }
    }
}
</script>
<template>
    
    <div class=" bg-secondaryBackground h-screen flex flex-col gap-y-10 items-center justify-start px-8 z-50 overflow-y-auto pb-5">
        <SearchBar />
        <Login 
        :arrayFiltrado="arrayFiltradoUser" />
        <UserLastReviewsContainer v-if="useUser.userEmail"
        :arrayFiltrado="arrayFiltradoUser" />
        <UserLastReviewsContainer v-else
        :arrayFiltrado="allReviewsFirestore" />
      
    </div>
  
</template>
