<script >
//Aquí importamos el componente Title

import Main from './components/principales/Main.vue'
import SideBarRight from './components/principales/SideBarRight.vue'
import BannerSerie from './components/principales/banners/BannerSerie.vue'
import Footer from './components/principales/Footer.vue'
import { useReviewsFirestore } from '../stores/reviewsFirestore.js'
import SearchBar from './components/principales/widgets/SearchBar.vue'
import LogInAuth from '@/components/principales/auth/LoginAuth.vue';
import {useLoginState} from '../stores/stateWidgetsStore.js'
import { UserRound } from 'lucide-vue-next'
import { monitorAuthState } from '../firebase/firebase.js'
import { useUserStore } from '../stores/userStore.js'
import LogoSeriesLab from './components/principales/widgets/LogoSeriesLab.vue'
import Login from './components/principales/widgets/Login.vue'


export default {

    setup(){
      const reviewsStore = useReviewsFirestore()
      const showLogIn = useLoginState()
      const userStore = useUserStore();

      return {
        reviewsStore,
        showLogIn,
        userStore,
      }
    },

  //Aquí llamamos a todos los componentes
    components:{

      Main,
      SideBarRight,
      BannerSerie,
      Footer,
      SearchBar,
      LogInAuth,
      UserRound,
      LogoSeriesLab,
      Login
      
     
    },
    data(){
        return{
            // showLogIn : true,
            showSignup: false,
        }
    },
    mounted(){
      this.reviewsStore.readReviews();
      console.log(this.reviewsStore);
      

      monitorAuthState((user) => {
        if (user) {
          this.userStore.setUser({
            email: user.email,
            uid: user.uid,
          });
          console.log(`Usuario loggeado: ${user.email}`);
        } else {
          userStore.clearUser();
          console.log(`No hay un usuario loggeado`);
        }
});
      
    }
}
</script>

<template>
  <div class=" font-poppinsLight min-h-screen text-white bg-mainBackground flex lg:flex-row flex-col ">

    <div v-if="showLogIn.showLoginMenu" class="fixed z-30 h-screen bg-backgroundColor bg-opacity-90 w-full ">
      
      <LogInAuth/>
    </div>
    <div class="bg-cover  items-center gap-7 bg-center bg-no-repeat h-28 px-5 flex flex-row justify-between lg:hidden " :style="{backgroundImage: `url(https://static.hbo.com/content/dam/hbodata/series/the-wire/ka/the-wire-ka-1920.jpg)`}" >
      <RouterLink to="/">

        <LogoSeriesLab/>
      </RouterLink>
      <SearchBar/>
    </div>
    <div class="px-5 pt-2 lg:hidden ">
      <Login/>
    </div>

    <div id='lef-side'class="lg:w-[calc(100%-300px)] w-full mt-5 lg:mt-0  ">
      
      <RouterView/> 
    </div>

    <div class="lg:w-[300px] lg:min-w-[300px]  lg:fixed  lg:top-0 lg:right-0 h-full hidden lg:block  ">
      <SideBarRight/>
      
    </div>
    


  </div>
</template>
