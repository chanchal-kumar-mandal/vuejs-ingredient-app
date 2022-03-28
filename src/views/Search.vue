<template>
  <div class="search">
    <img
      alt="Dotdash Meredith Logo"
      src="../assets/dotdash-meredith-logo.png"
    />
    <Index msg="Search Ingredient Manager" />
  </div>
</template>

<script>
// @ is an alias to /src
import Index from "@/components/Index.vue";

export default {
  name: "Search",
  components: {
    Index,
  },
};
</script>

<style scoped>
img {
  height: 100px;
}
</style>
