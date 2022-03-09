<template>
  <header class="header">
    <div class="wrapper">
      <nav>
        <RouterLink to="/home">Home</RouterLink>
        <RouterLink to="/">Calculator</RouterLink>
      </nav>
      <a href="#" @click="darkModeToggle()" class="moooon"> </a>
    </div>
  </header>

  <RouterView />
</template>
<script lang="ts">
import { RouterLink, RouterView } from "vue-router";
import { defineComponent } from "vue";
export default defineComponent({
  data() {
    return {
      userTheme: "light-mode",
    };
  },
  components: {},
  methods: {
    mounted() {
      const initUserTheme = this.getMediaPreference();
      this.setTheme(initUserTheme);
    },
    darkModeToggle() {
      const activeTheme = localStorage.getItem("user-theme");
      if (activeTheme === "light-mode") {
        this.setTheme("dark-mode");
      } else {
        this.setTheme("light-mode");
      }
    },
    setTheme(theme: string) {
      localStorage.setItem("user-theme", theme);
      this.userTheme = theme;
      document.documentElement.className = theme;
    },
    getMediaPreference() {
      const hasDarkPreference = window.matchMedia(
        "(prefers-color-scheme: dark)"
      ).matches;
      if (hasDarkPreference) {
        return "dark-mode";
      } else {
        return "light-mode";
      }
    },
  },
});
</script>
<style lang="scss">
@import "@/assets/base.scss";
@import "@/assets/app.scss";
@import "@/assets/responsive.scss";
</style>
