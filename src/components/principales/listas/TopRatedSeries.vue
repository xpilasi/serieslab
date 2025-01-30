<script>
//Aquí importamos el componente Title

import { RouterLink } from 'vue-router';
import CardResults from '../cards/CardResults.vue';
import LoadingSpinner from '../widgets/LoadingSpinner.vue';
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue';
// Import required modules
import { Navigation, Pagination, Autoplay } from 'swiper/modules';
// Import Swiper styles
import 'swiper/css';
import 'swiper/css/navigation';
import 'swiper/css/pagination';

export default {
    props: {
      arrayFromData: {
        type: Array
      }
    },

    components: {
      CardResults,
      LoadingSpinner,
      Swiper,
      SwiperSlide
    },

    data() {
        return {
          pathBaseSrcImg: 'https://image.tmdb.org/t/p/w200/',
          pathUrl: '/review-serie/',
          swiperOptions: {
            modules: [Navigation, Pagination, Autoplay],
            slidesPerView: 6,
            spaceBetween: 10,
            navigation: true,
            breakpoints: {
              320: {
                slidesPerView: 3,
                spaceBetween: 5
              },
              640: {
                slidesPerView: 4,
                spaceBetween: 8
              },
              1024: {
                slidesPerView: 6,
                spaceBetween: 10
              }
            }
          }
        }
    },
}
</script>

<template>
  <div class="lg:px-20 px-5 font-poppinsLight text-white flex flex-col pb-8">
    <h2 class="text-lg pb-6">Top Rated series</h2>
    
    <div v-if="arrayFromData.length > 0">
      <swiper
        :modules="swiperOptions.modules"
        :slides-per-view="swiperOptions.slidesPerView"
        :space-between="swiperOptions.spaceBetween"
        :navigation="swiperOptions.navigation"
        :breakpoints="swiperOptions.breakpoints"
        class="mySwiper"
      >
        <swiper-slide
          v-for="result in arrayFromData"
          :key="result.id"
          class="p-3"
        >
          <RouterLink :to="pathUrl+result.id" class="block overflow-hidden shadow-xl shadow-black/40 hover:shadow-xl hover:shadow-black/60 rounded-xl transition-all duration-300 ease-in-out">
            <CardResults
              :idSerie="result.id"
              :nombreSerie="result.name"
              :srcImg="pathBaseSrcImg+result.poster_path"
              :rating="result.vote_average"
              :anyoEmision="result.first_air_date.slice(0,4)"
              :descripcionSerie="result.overview"
            />
          </RouterLink>
        </swiper-slide>
      </swiper>
    </div>

    <div v-else class="flex h-80 flex-col justify-center items-center w-full">
      <LoadingSpinner/>
    </div>
  </div>
</template>

<style>
.swiper-button-next,
.swiper-button-prev {
  color: white !important;
  background: rgba(0, 0, 0, 0.5);
  padding: 2rem;
  border-radius: 50%;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.swiper:hover .swiper-button-next,
.swiper:hover .swiper-button-prev {
  opacity: 1;
}

.swiper-button-next::after,
.swiper-button-prev::after {
  font-size: 1.5rem !important;
}

.swiper-slide {
  height: auto;
}

.swiper {
  padding: 15px 0;
  margin: 0 -15px;
  width: calc(100% + 30px);
}
</style>
