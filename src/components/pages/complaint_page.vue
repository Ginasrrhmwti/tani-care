<template>
  <BaseLayout>
    <div class="flex flex-col items-center gap-10">
      <div class="text-5xl text-black font-bold">Form Pengaduan Petani</div>
      <form
        @submit.prevent="complaint"
        class="p-16 flex flex-col gap-7 border-2 border-stone-600 rounded-2xl w-full"
      >
        <input
          @input="(e) => (nama = e.target.value)"
          required
          type="text"
          placeholder="Nama..."
          class="input w-full bg-white border-stone-600 border-2 rounded-xl placeholder-gray-400 text-3xl px-3"
        />
        <input
          @input="(e) => (alamat = e.target.value)"
          required
          type="text"
          placeholder="Alamat..."
          class="input w-full bg-white border-stone-600 border-2 rounded-xl placeholder-gray-400 text-3xl px-3"
        />
        <input
          @input="(e) => (jenis_pengaduan = e.target.value)"
          required
          type="text"
          placeholder="Jenis pengaduan..."
          class="input w-full bg-white border-stone-600 border-2 rounded-xl placeholder-gray-400 text-3xl px-3"
        />
        <textarea
          @input="(e) => (deskripsi = e.target.value)"
          type="text"
          placeholder="Deskripsi..."
          class="border-2 border-stone-600 rounded-xl placeholder-gray-400 text-3xl px-3 text-black"
        />
        <input
          @change="handleInputImage"
          required
          type="file"
          name="foto"
          accept="image/*"
          id="foto"
          class="hidden"
        />
        <label
          for="foto"
          class="w-full border-2 border-stone-600 text-gray-400 rounded-xl text-3xl px-3"
        >
          {{ labelFoto }}
        </label>
        <input
          @input="(e) => (lokasi = e.target.value)"
          required
          type="text"
          placeholder="Lokasi..."
          class="input w-full bg-white border-stone-600 border-2 rounded-xl placeholder-gray-400 text-3xl px-3"
        />
        <div class="w-full flex items-center justify-between">
          <router-link
            to="/"
            class="bg-stone-600 text-white text-3xl p-2 rounded-2xl"
            >Kembali</router-link
          >
          <button class="bg-stone-600 text-white text-3xl p-2 rounded-2xl">
            Kirim
          </button>
        </div>
      </form>
    </div>
  </BaseLayout>
</template>
<script>
import BaseLayout from "../layouts/base_layout.vue";
import { supabase } from "../../lib/supabaseClient";

export default {
  data() {
    return {
      nama: "",
      alamat: "",
      jenis_pengaduan: "",
      deskripsi: "",
      foto: null,
      nama_foto: "",
      lokasi: "",
      labelFoto: "Foto",
    };
  },
  components: {
    BaseLayout,
  },
  methods: {
    async complaint() {
      let { error: insertError } = await supabase.from("pengaduan").insert({
        nama: this.nama,
        alamat: this.alamat,
        jenis_pengaduan: this.jenis_pengaduan,
        deskripsi: this.deskripsi,
        foto:
          "https://zlcwuuievxmuwjlksztx.supabase.co/storage/v1/object/public/images/" +
          this.nama_foto,
        lokasi: this.lokasi,
      });

      if (insertError) {
        alert("gagal membuat pengaduan");
        return;
      }

      let { data, error: uploadError } = await supabase.storage
        .from("images")
        .upload(this.nama_foto, this.foto, {
          upsert: false,
        });

      if (uploadError) {
        alert("gagal mengirim gambar");
        return;
      }

      this.$router.push("/report");
    },
    handleInputImage(event) {
      const files = event.target.files;
      if (files.length != 0) {
        const srandom = Math.random().toString(36).substring(2, 7);

        this.nama_foto = srandom + files[0].name;
        this.foto = files[0];
        this.labelFoto = files[0].name;
      }
    },
  },
};
</script>
