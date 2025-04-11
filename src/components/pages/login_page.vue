<template>
  <div
    class="w-screen flex items-center justify-center h-screen bg-[url(/8d6d087ef401236547d2541aabe8cf84.png)] bg-no-repeat bg-cover"
  >
    <form
      @submit.prevent="login"
      class="flex flex-col gap-16 items-center w-3xl bg-white/55 rounded-3xl py-28 px-5"
    >
      <div class="text-5xl font-extrabold text-green-800">Halaman Login</div>
      <input
        @input="(e) => (email = e.target.value)"
        type="email"
        placeholder="Email..."
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-3xl px-3"
        required
      />
      <input
        @input="(e) => (password = e.target.value)"
        type="password"
        placeholder="Password..."
        min="6"
        class="input w-[80%] bg-white input-success border-2 rounded-4xl placeholder-gray-400 text-3xl px-3"
        required
      />
      <div class="flex w-[80%] justify-between items-center">
        <router-link
          to="/register"
          class="bg-white border-2 rounded-4xl border-success text-gray-400 text-3xl px-5 py-2"
          >Register?</router-link
        >
        <button
          class="bg-white border-2 rounded-4xl border-success text-gray-400 text-3xl px-5 py-2"
        >
          Login
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
      email: "",
      password: "",
    };
  },
  methods: {
    async login() {
      const { data, error } = await supabase
        .from("users")
        .select()
        .eq("email", this.email)
        .eq("password", this.password);

      if (error) {
        alert("username atau password salah");
        return;
      }

      this.$cookies.set("auth_token", data[0].id);
      localStorage.setItem("credentials", JSON.stringify(data[0]));
      this.$router.push("/");
    },
  },
};
</script>
