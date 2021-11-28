<template>
  <div>
    <div class="d-flex flex-row">
      <div class="">
        <i v-if="!fired" class="bi bi-check-circle text-muted"></i>
        <i v-else class="bi bi-check-circle-fill text-success"></i>
      </div>
      <div class="ms-3 align-middle">
        <div
          class="text-start label"
          :class="{ 'text-muted': !fired, 'text-success': fired }"
        >
          {{ label }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EventMarker",
  props: {
    id: String,
    label: String,
    fired: Boolean,
  },
};
</script>

<style scoped>
.bi,
.label {
  font-size: 2em;
}
</style>
