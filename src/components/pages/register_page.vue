<template>
  <div
    class="w-screen flex items-center justify-center h-screen bg-[url(/8d6d087ef401236547d2541aabe8cf84.png)] bg-no-repeat bg-cover"
  >
    <form
      @submit.prevent="insert"
      class="flex flex-col gap-6 items-center w-3xl bg-white/55 rounded-3xl py-20 px-5"
    >
      <div class="text-5xl font-extrabold text-green-800">Halaman Register</div>
      <input
        @input="(e) => (nama_lengkap = e.target.value)"
        required
        type="text"
        placeholder="Username..."
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-3xl px-3"
      />
      <input
        @input="(e) => (alamat = e.target.value)"
        required
        type="text"
        placeholder="Address..."
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-3xl px-3"
      />
      <input
        @input="(e) => (email = e.target.value)"
        required
        type="text"
        placeholder="Email..."
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-3xl px-3"
      />
      <input
        @input="(e) => (password = e.target.value)"
        required
        type="password"
        placeholder="Password..."
        min="6"
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-3xl px-3"
      />
      <input
        @input="(e) => (tanggal_lahir = e.target.value)"
        required
        type="date"
        placeholder="Tanggal lahir..."
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-gray-400 text-3xl px-3"
      />
      <div class="flex w-[80%] justify-between items-center">
        <router-link
          to="/login"
          class="bg-white border-2 rounded-4xl border-success text-gray-400 text-3xl px-5 py-2"
          >Login?</router-link
        >
        <button
          class="bg-white border-2 rounded-4xl border-success text-gray-400 text-3xl px-5 py-2"
        >
          Register
        </button>
      </div>
    </form>
  </div>
</template>
<script>
import { supabase } from "../../lib/supabaseClient";

export default {
  data() {
    return {
      nama_lengkap: "",
      alamat: "",
      email: "",
      password: "",
      tanggal_lahir: null,
    };
  },
  methods: {
    async insert() {
      const { error: ErrorInsert } = await supabase.from("users").insert({
        nama: this.nama_lengkap,
        alamat: this.alamat,
        email: this.email,
        password: this.password,
        tanggal_lahir: this.tanggal_lahir,
      });

      if (ErrorInsert) {
        alert("gagal membuat akun");
        return;
      }

      this.$router.push("/login");
    },
  },
};
</script>
