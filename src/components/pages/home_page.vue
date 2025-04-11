<template>
  <BaseLayout>
    <div class="flex justify-center">
      <div class="container flex flex-col items-center gap-10">
        <div class="text-black text-3xl">Selamat Datang di Pengaduan Tani!</div>
        <div class="grid grid-cols-2 gap-10">
          <img v-bind:src="leftImagePath" class="h-96 rounded-2xl" />
          <img v-bind:src="rightImagePath" class="h-96 rounded-2xl" />
        </div>
        <div class="text-black text-3xl">
          Kami hadir untuk membantu petani dalam menyampaikan keluhan, masalah,
          dan aspirasi terkait pertanian. Dengan aplikasi ini, Anda bisa
          melaporkan kendala yang dihadapi dan mendapatkan solusi dengan cepat
          serta tepat.
        </div>
        <router-link
          to="/complaint"
          class="bg-stone-600 text-white text-3xl p-2 rounded-2xl"
          >Buat Pengaduan</router-link
        >
      </div>
    </div>
  </BaseLayout>
</template>

<script>
import BaseLayout from "../layouts/base_layout.vue";

export default {
  data() {
    return {
      leftImagePath: "01c42f932351a109f49d0fa7accd029f.jpeg",
      rightImagePath: "7650fb13387b806b4be9e50c82650242.jpeg",
    };
  },
  components: {
    BaseLayout,
  },
};
</script>
