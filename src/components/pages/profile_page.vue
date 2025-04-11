<template>
  <BaseLayout>
    <div class="flex flex-col gap-10">
      <form
        @submit.prevent="update"
        class="border-2 border-stone-400 flex flex-col px-16 py-7 items-center gap-10"
      >
        <img :src="userPath" class="w-24" />
        <div class="flex w-full items-center">
          <div class="w-1/4">Nama lengkap:</div>
          <input
            class="w-3/4 input"
            :value="user != null ? user.nama : ''"
            type="text"
            @input="(e) => (user.nama = e.target.value)"
          />
        </div>
        <div class="flex w-full items-center">
          <div class="w-1/4">Alamat:</div>
          <input
            class="w-3/4 input"
            :value="user != null ? user.alamat : ''"
            type="text"
            @input="(e) => (user.alamat = e.target.value)"
          />
        </div>
        <div class="flex w-full items-center">
          <div class="w-1/4">Email:</div>
          <input
            class="w-3/4 input"
            :value="user != null ? user.email : ''"
            type="email"
            @input="(e) => (user.email = e.target.value)"
          />
        </div>
        <div class="flex w-full items-center">
          <div class="w-1/4">Tanggal Lahir:</div>
          <input
            class="w-3/4 input"
            type="date"
            :value="user != null ? user.tanggal_lahir : ''"
            @input="(e) => (user.tanggal_lahir = e.target.value)"
          />
        </div>
        <button class="bg-stone-600 text-white text-3xl p-2 rounded-2xl">
          Simpan
        </button>
      </form>
      <div class="flex justify-start w-full">
        <router-link
          to="/"
          class="bg-stone-600 text-white text-3xl p-2 rounded-2xl"
          >kembali</router-link
        >
      </div>
    </div>
  </BaseLayout>
</template>
<script>
import { supabase } from "../../lib/supabaseClient";
import BaseLayout from "../layouts/base_layout.vue";
export default {
  components: {
    BaseLayout,
  },
  data() {
    return {
      userPath: "user.png",
      user: null,
    };
  },
  async created() {
    const { data, error: ErrorGetUser } = await supabase
      .from("users")
      .select()
      .eq("id", this.$cookies.get("auth_token"));

    if (ErrorGetUser) {
      console.log(ErrorGetUser);
    }
    this.user = data[0];
  },
  methods: {
    async update() {
      const { error: ErrorUpdateUser } = await supabase
        .from("users")
        .update({
          nama: this.user.nama,
          email: this.user.email,
          alamat: this.user.alamat,
          tanggal_lahir: this.user.tanggal_lahir,
        })
        .eq("id", this.user.id);

      if (ErrorUpdateUser) {
        alert("gagal update profile");
        return;
      }

      location.reload();
    },
  },
};
</script>
