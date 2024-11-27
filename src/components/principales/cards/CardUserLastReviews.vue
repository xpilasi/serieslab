<script >
import { Star, Edit, Trash2 } from 'lucide-vue-next'

export default {

    props: {
        totalSeries:{tupe:Number},
        idSerie:{type:Number},
        nombreSerie:{type:String},
        srcImg:{type: String},
        rating:{type: Number},
        anyoEmision:{type: String},
        descripcionSerie:{type:String}
    
    },

  //Aquí llamamos a todos los componentes
    components:{
     Star
    },
    data(){
        return{
        }
    },
}
</script>

<template>


    <div class="bg-white bg-opacity-5 rounded-2xl flex flex-row ">
        <div class="col-1 w-2/3 3xl:w-3/4 flex flex-col p-4">
            <h4 class="text-sm mb-1 h-10">{{ nombreSerie }}</h4>
            <div class="flex flex-row items-center justify-center  ">

                <Star color="#dcd62e" strokeWidth={3} fill="#dcd62e" width="50px"/>
                <div class=" text-xl text-gray-400  font-poppinsRegular ">{{ parseFloat(rating.toFixed(1))}}</div>
            </div>
        </div>
        <div class="w-1/3 3xl:w-1/4 flex justify-end">
            <img :src="srcImg" alt="" class="w-full h-full object-cover aspect-square rounded-2xl">
        </div>
    </div>

</template>
