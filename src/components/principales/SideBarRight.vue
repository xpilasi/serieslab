<script >

import LastReviewsContainer from './listas/LastReviewsContainer.vue';
import UserLastReviewsContainer from './listas/UserLastReviewsContainer.vue';
import Login from './widgets/Login.vue';
import SearchBar from './widgets/SearchBar.vue';
import { useReviewsFirestore } from '../../../stores/reviewsFirestore';
import { useUserStore } from '../../../stores/userStore'
import { useLoginState } from '../../../stores/stateWidgetsStore';

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
            arrayFiltradoUser : []
        }
        

    },
    methods:{

     
    },
    async created(){

        try{
            await this.useReviews.readReviews()
            this.arrayFiltradoUser = this.useReviews.filterReviewsByUser(this.useUser.userEmail)
            console.log(this.arrayFiltradoUser);
            
        }catch(e){

        }
     
           
            
    },

    components:{
    
        LastReviewsContainer,
        UserLastReviewsContainer,
        SearchBar,
        Login

    }
}
</script>
<template>
    
    <div class=" bg-secondaryBackground h-full flex flex-col gap-y-10 items-center justify-start px-8 z-50">
        <SearchBar />
        <Login 
        :arrayFiltrado="arrayFiltradoUser" />
        <UserLastReviewsContainer v-if="arrayFiltradoUser.length > 0"
        :arrayFiltrado="arrayFiltradoUser" />
      
    </div>
  
</template>
