<template>
  <div class="row justify-content-center">
    <div class="col-lg-6 col-md-8 mt-5">
      <div class="footer border-top mb-5 pt-3">
        <p class="m-0">
          {{ title }} ({{ version }}) made by
          <a rel="noopener" target="_blank" href="https://www.nickbreslin.com"
            >Nick Breslin</a
          >.
        </p>
        <p class="m-0">
          Proudly built with
          <a rel="noopener" target="_blank" href="https://https://vuejs.org/"
            >Vue</a
          >
          and
          <a rel="noopener" target="_blank" href="https://getbootstrap.com/"
            >Bootstrap</a
          >.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import PackageJSON from "../../../package.json";

export default {
  name: "Footer",
  data() {
    return {
      title: PackageJSON.title,
      version: PackageJSON.version
    };
  }
};
</script>
