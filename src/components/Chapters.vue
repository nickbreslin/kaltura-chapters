<template>
  <div class="card card-body">
    <div
      v-for="(chapter, i) in chapters"
      :key="i"
      class="mb-3"
      @click="setSeconds(chapter.seconds)"
    >
      <div class="btn btn-primary w-100 fw-bold h2">{{ chapter.label }}</div>
      <img class="img-fluidd d-none" :src="getURL(chapter.seconds)" />
    </div>
  </div>
</template>

<script>
export default {
  name: "Chapters",
  components: {},
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
