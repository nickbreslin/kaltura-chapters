<template>
  <div class="row">
    <div class="col-4">
      <Chapters :chapters="chapters" @setSeconds="setSeconds($event)" />
    </div>
    <div class="col-8">
      <Viewer :seconds="seconds" />
      <textarea
        class="form-control mt-5"
        rows="10"
        v-model="description"
      ></textarea>
      <small>Each chapter needs the format:<br />00:00 Chapter Title</small>
    </div>
  </div>
</template>

<script>
import Chapters from "@/components/Chapters";

import Viewer from "@/components/Viewer";

export default {
  name: "Home",
  components: { Chapters, Viewer },
  data() {
    return {
      description: "",
      seconds: 0
    };
  },
  computed: {
    validLines() {
      let lines = this.description.split("\n");

      lines = lines.filter((line) => {
        let timestamp = line.split(" ");
        if (timestamp.length < 2) {
          return false;
        }

        if (!/[0-9]{1,2}:[0-9]{1,2}/.test(line)) {
          return false;
        }

        return true;
      });

      return lines;
    },
    chapters() {
      let chapters = this.validLines.map((line) => {
        const parts = line.split(" ");
        let timestamp = parts.shift();
        let label = parts.join(" ");
        label = label.trim();

        const timeParts = timestamp.split(":");

        let minutes = timeParts[0];
        let seconds = timeParts[1];

        let totalSeconds = parseInt(minutes) * 60 + parseInt(seconds);

        return {
          seconds: totalSeconds,
          label: label,
          timestamp: timestamp
        };
      });

      return chapters;
    }
  },
  methods: {
    setSeconds(seconds) {
      this.seconds = seconds;
    }
  }
};
</script>
