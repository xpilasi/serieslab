<script >
import BannerSerie from '../principales/banners/BannerSerie.vue';
import Main from '../principales/Main.vue';

export default {

    components:{
      Main,
      BannerSerie,
    }
}
</script>
<template>
    
    <div class=" min-h-[calc(100vh-14rem)] flex flex-col gap-4 justify-center align-middle w-full">
        <BannerSerie/>
        <Main/>
    </div>
  
</template>