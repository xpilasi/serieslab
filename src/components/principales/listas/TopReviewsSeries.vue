<script>
import { RouterLink } from 'vue-router';
import LoadingSpinner from '../widgets/LoadingSpinner.vue';
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue';
// Import required Swiper modules
import { Navigation, Pagination, Autoplay } from 'swiper/modules';
// Import Swiper styles
import 'swiper/css';
import 'swiper/css/navigation';
import 'swiper/css/pagination';

export default {
  props: {
    arrayFromFirestore: { type: Array }
  },
  
  components: {
    LoadingSpinner,
    Swiper,
    SwiperSlide
  },

  data() {
    return {
      pathUrl: '/review-serie/',
      // Swiper configuration
      swiperOptions: {
        modules: [Navigation, Pagination, Autoplay],
        slidesPerView: 5,
        spaceBetween: 10,
        navigation: true,
        breakpoints: {
          320: {
            slidesPerView: 2,
            spaceBetween: 5
          },
          640: {
            slidesPerView: 4,
            spaceBetween: 8
          },
          1024: {
            slidesPerView: 5,
            spaceBetween: 10
          }
        }
      }
    }
  }
}
</script>

<template>
  <div class="font-poppinsLight text-white flex flex-col">
    <h2 v-if="arrayFromFirestore.length > 0" class="text-lg lg:pl-20 pl-5">Top Reviews</h2>
    
    <div class="flex flex-row gap-4 w-full h-auto">
      <!-- Swiper Component -->
      <div v-if="arrayFromFirestore.length > 0" class="w-full lg:px-20 px-5">
        <swiper
          :modules="swiperOptions.modules"
          :slides-per-view="swiperOptions.slidesPerView"
          :space-between="swiperOptions.spaceBetween"
          :navigation="swiperOptions.navigation"
          :breakpoints="swiperOptions.breakpoints"
          class="mySwiper"
        >
          <swiper-slide
            v-for="serie in arrayFromFirestore"
            :key="serie.serieId"
            class="p-3"
          >
            <RouterLink
              :to="pathUrl + serie.serieId"
              class="block overflow-hidden shadow-xl shadow-black/40 hover:shadow-xl hover:shadow-black/60 rounded-xl transition-all duration-300 ease-in-out"
            >
              <div class="aspect-[2/3] relative">
                <img
                  :src="serie.srcImage"
                  :alt="serie.serieName"
                  class="w-full h-full object-cover hover:scale-105 transition-transform duration-300 ease-in-out"
                />
              </div>
            </RouterLink>
          </swiper-slide>
        </swiper>
      </div>

      <!-- Spinner si no hay datos -->
      <div v-else class="flex h-80 flex-col justify-center items-center w-full">
        <LoadingSpinner />
      </div>
    </div>
  </div>
</template>

<style>
/* Custom Swiper Navigation Styles */
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

/* Add these new styles */
.swiper-slide {
  height: auto;
}

.swiper {
  padding: 15px 0;
  margin: 0 -15px;
  width: calc(100% + 30px);
}
</style>
