<template>
  <div
    v-for="(chapter, i) in chapters"
    :key="i"
    class="mb-3 clickable"
    @click="setSeconds(chapter.seconds)"
  >
    <div class="card">
      <div class="card-header d-flex justify-content-between">
        <div class="fw-bold">{{ chapter.label }}</div>
        <span class="badge bg-primary pt-2 pb-2">{{ chapter.timestamp }}</span>
      </div>
      <div class="card-body">
        <img class="img-fluid" :src="getURL(chapter.seconds)" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Chapters",
  props: {
    chapters: Array
  },
  computed: {},
  methods: {
    getURL(seconds) {
      let partnerID = 391241;
      let entryId = "1_9y74c1on";
      return `https://cdnsecakmi.kaltura.com/p/${partnerID}/thumbnail/entry_id/${entryId}/vid_sec/${seconds}/width/200`;
    },
    setSeconds(seconds) {
      this.$emit("setSeconds", seconds);
    }
  }
};
</script>

<style scoped>
.clickable {
  cursor: pointer;
}
</style>
