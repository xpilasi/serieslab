<script >
import { LogOut,UserRound } from 'lucide-vue-next';
import {useUserStore} from '../../../../stores/userStore.js'
import { useLoginState } from '../../../../stores/stateWidgetsStore.js';
import {auth} from '../../../../firebase/firebase.js';
import { signOut } from 'firebase/auth';
import LoadingText from './LoadingText.vue';


export default {

  //Para llamar al nombre del usuario
  setup() {
    const userStore = useUserStore();
    const showLogIn = useLoginState();

    return {
      userStore, // Accediendo al store completo
      showLogIn,
    };
  },
  
  //Aquí llamamos a todos los componentes
    components:{
      LogOut,
      UserRound,
      LoadingText
    },
    props:{
      arrayFiltrado:{
        type:Array,
        default: () => []
      }
    },
    data(){
        return{
            
        }
    },
    methods:{
      async logout(){

        try{
          //Cierro sesión usuario de Firestore
          await signOut(auth)

          //Limpio el estado de Pinia
          this.userStore.clearUser();
          console.log('Usuario deslogueado');
        }catch(e){
          console.error(`Error cerrando la sesión: ${e}`);
        }
      },
       generarNickName (email){
        let nickname = '@';

        for(const char of email){
          if(char == '@'){
            break
          }
          nickname += char;
        }
        return nickname
      }
    
    },
    mounted(){
      console.log(this.arrayFiltrado.length);
      
    },
    watch: {
    arrayFiltrado: {
        handler(newVal) {
            console.log('Prop arrayFiltrado actualizado en Login:', newVal);
        },
        immediate: true // Ejecuta el watcher inmediatamente al montar
    }
}
    
}
</script>

<template>
<div class="grid grid-cols-2 gap-y-2 w-full text-sm">


  <div class="flex col-span-2 lg:justify-between mb-2  text-xs justify-end ">
    <div class="flex flex-row items-center justify-center gap-x-3 ">
      <div class="flex flex-row gap-2 items-center lg:text-xs text-sm">
        <span class="lg:block hidden">Hola </span>
        <div class="lg:hidden block"><UserRound width="20px"/></div>
        <span v-if="userStore.isLoggedIn" class="text-primary ">{{generarNickName(userStore.userEmail)}} </span>
        <span v-if="userStore.isLoggedIn == false" class="text-white lg:text-purple-300" >Invitad@</span>
        <span v-else class="text-purple-300" ><LoadingText/></span>
      </div>
      
      
      
    </div>
    <button v-if="userStore.isLoggedIn" @click="logout" class="hover:text-purple-300 transition-all duration-500" >
      <LogOut />
    </button>
    <button v-else @click="showLogIn.toggleMenuLogin()" class="lg:text-xs ml-3 text-sm px-5 py-2 rounded-2xl bg-primary hover:bg-purple-300  transition-all duration-500">
      Log In
    </button>
  </div>
  <span class="text-xs lg:block hidden">Total reviews</span>
  <span class="text-right text-xs lg:block hidden">{{ arrayFiltrado.length ? arrayFiltrado.length : 0 }}</span>

</div>
</template>
