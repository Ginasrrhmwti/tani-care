<template>
  <BaseLayout>
    <div class="flex flex-col items-center gap-10">
      <div class="text-5xl text-black font-bold">Laporan Pengaduan Petani</div>
      <div class="overflow-x-auto w-full border-2 border-stone-400 rounded-2xl">
        <table class="table text-2xl">
          <!-- head -->
          <thead>
            <tr>
              <th>Nama</th>
              <th>Alamat</th>
              <th>Jenis pengaduan</th>
              <th>Deskripsi</th>
              <th>Foto</th>
              <th>Lokasi</th>
            </tr>
          </thead>
          <tbody v-if="data.length != 0">
            <tr v-for="item in data" :key="item.id">
              <td>{{ item.nama }}</td>
              <td>{{ item.alamat }}</td>
              <td>{{ item.jenis_pengaduan }}</td>
              <td>{{ item.deskripsi }}</td>
              <td>
                <img :src="item.foto" class="w-16 aspect-square object-cover" />
              </td>
              <td>{{ item.lokasi }}</td>
            </tr>
          </tbody>
        </table>
      </div>
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
import BaseLayout from "../layouts/base_layout.vue";
import { supabase } from "../../lib/supabaseClient";

export default {
  data() {
    return {
      data: [],
    };
  },
  components: {
    BaseLayout,
  },
  async created() {
    const { data, error } = await supabase.from("pengaduan").select();
    if (error) {
      alert("gagal menampilkan laporan");
      return;
    }
    this.data = data;
  },
};
</script>
