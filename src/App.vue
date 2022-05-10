<template>
  <div class="container my-5">
    <TheHeader />
    <router-view />
    <TheFooter />
  </div>
</template>

<script>
import TheHeader from "@/components/layout/TheHeader";
import TheFooter from "@/components/layout/TheFooter";

export default {
  name: "Home",
  components: { TheHeader, TheFooter },
};
</script>
