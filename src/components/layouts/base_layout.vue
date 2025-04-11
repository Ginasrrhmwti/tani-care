<template>
  <!-- sidebar -->
  <div className="drawer drawer-end z-50">
    <input id="my-drawer-4" type="checkbox" className="drawer-toggle" />
    <div className="drawer-side">
      <label
        htmlFor="my-drawer-4"
        aria-label="close sidebar"
        className="drawer-overlay"
      ></label>
      <ul className="menu bg-base-200 text-base-content min-h-full w-80 p-4">
        <li class="w-full h-36 flex items-center justify-center">
          <img :src="userPath" class="w-32" />
        </li>
        <li><router-link to="/profile">Edit profile</router-link></li>
        <li v-if="username == admin">
          <router-link to="/report">Laporan</router-link>
        </li>
        <li><a @click="logout">Logout</a></li>
      </ul>
    </div>
  </div>
  <div class="flex flex-col gap-10 w-screen h-screen bg-white">
    <!-- topbar -->
    <div class="bg-stone-600 flex justify-center">
      <div class="flex items-center py-1 justify-between container">
        <router-link to="/">
          <img v-bind:src="logoPath" class="w-24" />
        </router-link>
        <div class="text-white text-5xl">TANI CARE</div>
        <label htmlFor="my-drawer-4">
          <!-- <v-icon name="HiSolidMenu" /> -->
          <img v-bind:src="menuImagePath" class="w-12" />
        </label>
      </div>
    </div>
    <div class="flex flex-col items-center">
      <div class="container">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
import { supabase } from "../../lib/supabaseClient";

export default {
  data() {
    return {
      logoPath: "636e37f68142fe0d73e6fdc1a75ff952.png",
      menuImagePath: "menu.svg",
      userPath: "user.png",
      username: "",
    };
  },
  async created() {
    const { data, error } = await supabase
      .from("users")
      .select()
      .eq("id", this.$cookies.get("auth_token"));

    if (error) {
      this.$cookies.remove("auth_token");
      this.$router.push("/login");
      return;
    }

    this.username = data[0].nama.toLowerCase();
  },
  props: ["path"],
  methods: {
    logout() {
      this.$cookies.remove("auth_token");
      this.$router.push("/login");
    },
  },
};
</script>
