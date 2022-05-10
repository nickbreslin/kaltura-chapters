<template>
  <div class="h1 border-bottom mb-5">{{ title }}</div>
</template>

<script>
import PackageJSON from "../../../package.json";

export default {
  name: "Footer",
  data() {
    return {
      title: PackageJSON.title
    };
  }
};
</script>
