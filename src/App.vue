<template>
  <div class="home">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-lg-6 col-md-8 mt-5">
          <div class="h1">Video Complete</div>
          <div class="card card-body bg-secondary">
            <div class="mb-5">
              <div class="h4 fw-bold">Script length</div>
              <div class="card card-body boxshadow">
                <div class="btn-group mb-3" role="group">
                  <button
                    type="button"
                    v-bind:class="{ active: this.btnGroups.words === 0 }"
                    @click="setActiveCount(0)"
                    class="btn btn-secondary"
                  >
                    Word Count
                  </button>
                  <button
                    type="button"
                    v-bind:class="{ active: this.btnGroups.words === 1 }"
                    @click="setActiveCount(1)"
                    class="btn btn-secondary"
                  >
                    Script
                  </button>
                </div>

                <!-- Word Count -->
                <div
                  class="copy-script-wrapper"
                  v-if="this.btnGroups.words === 0"
                >
                  <input
                    class="form-control form-control-lg"
                    type="number"
                    v-model="this.script.words"
                  />
                </div>

                <!-- Textarea -->
                <div
                  class="copy-script-wrapper"
                  v-if="this.btnGroups.words === 1"
                >
                  <textarea
                    class="form-control"
                    rows="5"
                    v-model="script.copy"
                  />

                  <div class="total mt-3 h4 mb-0">
                    Total word count:
                    <span class="badge badge-primary">{{
                      scriptWordCount
                    }}</span>
                  </div>
                </div>
              </div>
            </div>

            <div class="mb-5">
              <div class="h4 fw-bold">Speaking rate</div>
              <div class="card card-body boxshadow">
                <div class="btn-group mb-3" role="group">
                  <button
                    type="button"
                    v-bind:class="{ active: this.btnGroups.rate === 0 }"
                    @click="setActiveWpm(0)"
                    class="btn btn-secondary"
                  >
                    Standard
                  </button>
                  <button
                    type="button"
                    v-bind:class="{ active: this.btnGroups.rate === 1 }"
                    @click="setActiveWpm(1)"
                    class="btn btn-secondary"
                  >
                    Personalized
                  </button>
                </div>

                <!-- Word Count -->

                <div
                  class="copy-script-wrapper"
                  v-if="this.btnGroups.rate === 0"
                >
                  <span>Words per minute:</span>
                  <select class="form-control" v-model="ratePicker">
                    <option value="110">110 WPM (Slow)</option>
                    <option value="130">130 WPM (Average)</option>
                    <option value="150">150 WPM (Fast)</option>
                  </select>
                </div>

                <!-- Textarea -->
                <div
                  class="copy-script-wrapper"
                  v-if="this.btnGroups.rate === 1"
                >
                  <p>
                    Press <b>Start Timer</b> and read the following sentence,
                    then press <b>Stop Timer</b>:
                  </p>
                  <div class="alert alert-light">
                    <p class="m-0">
                      Modern readability tests are designed to indicate
                      comprehension difficulty when reading a passage of
                      contemporary academic English.
                    </p>
                  </div>

                  <!-- Start Timer-->
                  <button
                    v-bind:class="{
                      'btn-secondary': timer.isRunning,
                      'btn-success': !timer.isRunning,
                    }"
                    @click="startTimer()"
                    class="btn mr-3"
                    :disabled="timer.isRunning"
                  >
                    Start timer
                  </button>

                  <!-- Stop Timer -->
                  <button
                    v-bind:class="{
                      'btn-secondary': !timer.isRunning,
                      'btn-danger': timer.isRunning,
                    }"
                    @click="stopTimer()"
                    class="btn"
                    :disabled="!timer.isRunning"
                  >
                    Stop timer
                  </button>

                  <span class="float-right">
                    Duration:
                    <span class="badge badge-primary">{{ getDuration }}</span>
                  </span>

                  <div class="total mt-3 h4 pt-3 mb-0 border-top">
                    Words per minute:
                    <span class="badge badge-primary">{{
                      getSpeakingRate ? getSpeakingRate : `n/a`
                    }}</span>
                  </div>
                </div>
              </div>
            </div>

            <div class="">
              <div class="h4 fw-bold">Summary</div>
              <div class="card card-body boxshadow">
                <p v-if="getRawSeconds" class="mb-0">
                  With a word count of
                  <span class="fw-bold text-primary border-bottom">{{
                    getWordCount
                  }}</span>
                  and a speaking rate of
                  <span class="fw-bold text-primary border-bottom">{{
                    getSpeakingRate
                  }}</span>
                  words per minute, this narration is estimated to take
                  <span class="fw-bold text-primary border-bottom">{{
                    getCalculatedMinutes
                  }}</span>
                  minutes
                  <span class="fw-bold text-primary border-bottom">{{
                    getCalculatedSeconds
                  }}</span>
                  seconds.
                </p>
                <p v-else class="mb-0">
                  Fill out the details above to get the summary.
                </p>
              </div>
            </div>
          </div>
          <!-- card -->
          <div class="footer border-top mb-5 pt-3">
            <p class="m-0">
              Narrator made by
              <a href="https://www.nickbreslin.com">Nick Breslin</a>.
            </p>
            <p class="m-0">
              Proudly built with
              <a href="https://https://vuejs.org/">Vue</a> and
              <a href="https://getbootstrap.com/">Bootstrap</a>.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//import WPM from "@/components/WPM";
//import Textarea from "@/components/Textarea";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      btnGroups: {
        words: 0,
        rate: 0,
      },
      ratePicker: 130,
      timer: {
        start: 0,
        end: 0,
        duration: 0, // In milliseconds
        isRunning: false,
      },
      script: {
        words: 0,
        copy: "",
      },
      wpmType: 130,
    };
  },
  methods: {
    setActiveCount(id) {
      this.btnGroups.words = id;
    },
    setActiveWpm(id) {
      this.btnGroups.rate = id;
    },
    startTimer() {
      this.timer.isRunning = true;
      this.timer.start = Math.round(new Date().getTime());
      this.timer.duration = 0;
    },
    stopTimer() {
      this.timer.isRunning = false;
      this.timer.end = Math.round(new Date().getTime());
      this.timer.duration = this.timer.end - this.timer.start;
    },
    convertDuration(mts) {
      let duration = mts / 1000; // convert from milliseconds to seconds
      duration = duration.toFixed(2);

      return duration;
    },
  },
  computed: {
    scriptWordCount() {
      if (this.script.copy.length === 0) {
        return 0;
      }

      let words = this.script.copy.split(" ");
      words = words.filter((e) => e.trim().length);
      return words.length;
    },
    getDuration() {
      if (!this.timer.duration) {
        return "n/a";
      }

      let duration = this.convertDuration(this.timer.duration);

      return `${duration} seconds`;
    },
    getWordCount() {
      if (this.btnGroups.words === 0) {
        // Script Length - Word Count
        return parseInt(this.script.words);
      }

      return this.scriptWordCount;
    },
    getSpeakingRate() {
      if (this.btnGroups.rate === 0) {
        // Speaking rate - Standard
        return this.ratePicker;
      }

      // Any recording?
      if (!this.timer.duration) {
        return 0;
      }

      let duration = this.convertDuration(this.timer.duration);
      duration = (60 / duration) * 14;
      duration = Math.round(duration);
      return duration;
    },
    getCalculatedSeconds() {
      let seconds = this.getRawSeconds;

      seconds = seconds % 60;
      return seconds;
    },
    getCalculatedMinutes() {
      let seconds = this.getRawSeconds;

      let minutes = seconds / 60;
      minutes = Math.floor(minutes);
      return minutes;
    },
    getRawSeconds() {
      if (!this.getSpeakingRate) {
        return 0;
      }

      let length = (this.getWordCount / this.getSpeakingRate) * 60;
      length = Math.round(length);
      return length;
    },
  },
};
</script>
