<script>
import { mapState } from "pinia";
import { Star, Edit, Trash2 } from "lucide-vue-next";
import { useReviewsFirestore } from "../../../../stores/reviewsFirestore.js";
import DeleteModal from "./DeleteModal.vue";
import { useUserStore } from "../../../../stores/userStore.js";
import { deleteDoc, doc } from "firebase/firestore";
import { db } from "../../../../firebase/firebase.js";
import UpdateReview from "./UpdateReview.vue";
import { onMounted, } from 'vue';

export default {
  props: {
    serieId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isDeleting: false,
      showDeleteModal: false,
      reviewToDelete: null,
      isUpdating: false,
      showEditModal: false,
      reviewToUpdate: null,
      selectedReview: null,
      formattedDate: new Date().toLocaleDateString("en-US", {
        month: "2-digit",
        day: "2-digit",
        year: "numeric",
      }),
    };
  },
  computed: {
    ...mapState(useReviewsFirestore, ["reviewsFirestore"]),
    ...mapState(useUserStore, ["isLoggedIn", "userEmail"]),
    filteredReviews() {
      if (!this.reviewsFirestore || !Array.isArray(this.reviewsFirestore)) {
        return [];
      }
      const searchId = Number(this.serieId);
      return this.reviewsFirestore.filter((review) =>  {
        return review.serieId === Number(this.serieId);
      }
      );
    },
  },
  methods: {
    confirmDelete(id) {
      this.reviewToDelete = id;
      this.showDeleteModal = true;
    },
    async handleDelete() {
      if (!this.reviewToDelete) {
        console.log("No hay ID para eliminar");
        return;
      }

      this.isDeleting = true;

      try {
        console.log("Intentando eliminar review:", this.reviewToDelete);
        const reviewRef = doc(db, "all-reviews-series", this.reviewToDelete);
        await deleteDoc(reviewRef);
        console.log("Review eliminada correctamente");

        // Actualizar el estado local después de eliminar
        const store = useReviewsFirestore();
        await store.readReviews();
        this.showDeleteModal = false;
      } catch (error) {
        console.error("Error al borrar:", error);
      } finally {
        this.isDeleting = false;
        this.reviewToDelete = null;
      }
    },
    closeDeleteModal() {
      this.showDeleteModal = false;
      this.reviewToDelete = null;
    },
    updateReview(review) {
      this.reviewToUpdate = review.id;
      this.selectedReview = review;
      this.showEditModal = true;
    },
    async handleUpdate(updatedData) {
      if (!this.reviewToUpdate) {
        console.log("No hay ID para actualizar");
        return;
      }

      this.isUpdating = true;

      try {
        const store = useReviewsFirestore();
        await store.UpdateReview(this.reviewToUpdate, {
          comment: updatedData.comment,
          rating: updatedData.rating,
          lastUpdate: this.formattedDate,
          userEmail: this.userEmail,
        });
        this.showEditModal = false;
      } catch (error) {
        console.error("Error al actualizar:", error);
      } finally {
        this.isUpdating = false;
        this.reviewToUpdate = null;
        this.selectedReview = null;
      }
    },
    closeEditModal() {
      this.showEditModal = false;
      this.reviewToUpdate = null;
      this.selectedReview = null;
    },
  },
  mounted() {
    const store = useReviewsFirestore();
    store.readReviews();

  },
  components: {
    Star,
    Edit,
    Trash2,
    DeleteModal,
    UpdateReview,
  },
  
};
</script>

<template>
  <div>
    <div
      v-for="review in filteredReviews"
      :key="review.id"
      class="pt-4 h-auto lg:px-20 mx-5 lg:mx-0 "
    >
      <div
        class="reviewrounded-2xl bg-secondaryBackground rounded-2xl h-full flex flex-col p-6"
      >
        <div class="flex flex-col md:flex-row justify-between w-full  items-center mb-4">

          <!-- nota -->
          <div class="flex flex-col md:flex-row items-center gap-2">
            <div class="flex justify-center gap-3">
              <button>
              <Star color="#dcd62e" strokeWidth="{3}" fill="#dcd62e" />
            </button>
            <span class="font-medium">{{ review.rating }}/10</span>
            </div>
            
            <div class="text-primary text-sm pl-5">
              {{ review.userEmail.length > 20 ? review.userEmail.slice(0,20) : review.userEmail }}
            </div>
            <span class="text-gray-400 px-4">
              <data value="dateNow">{{ review.lastUpdate }}</data>
            </span>
            
          </div>

          <div class="flex flex-row gap-2">
            <button
              v-if="isLoggedIn && userEmail === review.userEmail"
              @click="updateReview(review)"
              class="p-2 hover:bg-gray-700/30 rounded-full transition-colors"
            >
              <Edit class="w-5 h-5 stroke-white" />
            </button>

            <button
              v-if="isLoggedIn && userEmail === review.userEmail"
              @click="confirmDelete(review.id)"
              :disabled="isDeleting"
              class="p-2 hover:bg-gray-700/30 rounded-full transition-colors"
            >
              <Trash2 class="w-5 h-5 stroke-white" />
            </button>
          </div>
        </div>

        <div class="text-gray-300 text-sm leading-relaxed">
          {{ review.comment }}
        </div>
      </div>
    </div>
    <DeleteModal
      :is-open="showDeleteModal"
      :is-deleting="isDeleting"
      @close="closeDeleteModal"
      @confirm="handleDelete"
    />
    <UpdateReview
      :is-open="showEditModal"
      :is-updating="isUpdating"
      :review="selectedReview"
      @close="closeEditModal"
      @confirm="handleUpdate"
    />
  </div>
</template>