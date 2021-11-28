<template>
  <div id="myEmbedTarget" style="width: 100%; height: 600px"></div>

  <div class="mb-3 mt-3 border rounded bg-secondary p-3">
    <div class="row">
      <EventMarker
        class="col"
        v-for="event in events"
        :key="event.id"
        v-bind="event"
      />
    </div>
  </div>
  <div class="row">
    <!-- <div class="col">
      <p>Duration: {{ duration }}</p>
    </div>-->
    <div class="col">
      <div class="card">
        <div class="card-header">Total watch time</div>
        <div class="card-body">{{ totalWatchTime }} seconds</div>
      </div>
    </div>
    <div class="col">
      <div class="card">
        <div class="card-header">Thoroughness</div>
        <div class="card-body">
          {{ uniqueWatchTime }} seconds ({{ uniquePercent }}%)
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EventMarker from "@/components/EventMarker.vue";

export default {
  name: "Viewer",
  components: {
    EventMarker,
  },
  data: function () {
    return {
      duration: 0,
      isPlaying: false,
      startFrom: 0,
      watches: [],
      events: [
        {
          id: "firstPlay",
          label: "First Play",
          fired: false,
        },
        {
          id: "firstQuartile",
          label: "25%",
          fired: false,
        },
        {
          id: "secondQuartile",
          label: "50%",
          fired: false,
        },
        {
          id: "thirdQuartile",
          label: "75%",
          fired: false,
        },
        {
          id: "playerPlayEnd",
          label: "100%",
          fired: false,
        },
      ],
    };
  },
  computed: {
    totalWatchTime() {
      if (this.watches.length == 0) {
        return Number(0).toPrecision(2);
      }

      if (this.watches.length == 1) {
        return this.watches[0].duration.toPrecision(2);
      }

      let map = this.watches.map((e) => e.duration);

      let total = map.reduce((sum, add) => {
        return sum + add;
      });

      return total.toPrecision(2);
    },
    uniqueWatchTime() {
      let watches = [...this.watches];
      watches.sort((a, b) => (a.start > b.start ? 1 : -1));

      let startFrom = 0;
      let duration = 0;
      watches.forEach((watch) => {
        if (startFrom < watch.start) {
          startFrom = watch.start;
        }

        let endTime = watch.start + watch.duration;

        if (endTime > startFrom) {
          duration += endTime - startFrom;
          startFrom = endTime;
        }
      });
      return duration.toPrecision(2);
    },
    uniquePercent() {
      return Math.round((100 / this.duration) * this.uniqueWatchTime);
    },
  },
  methods: {
    setEvent(id) {
      let index = this.events.findIndex((event) => event.id == id);

      if (index >= 0) {
        this.events[index].fired = true;
      }
    },
    setStart(timestamp) {
      this.startFrom = timestamp;
    },
    setEnd(timestamp) {
      this.watches.push({
        start: this.startFrom,
        duration: timestamp - this.startFrom,
      });
    },
    setDuration(duration) {
      this.duration = duration;
    },
    setPlayState(value) {
      this.isPlaying = value;
    },
  },
  mounted() {
    let partnerID = 391241;
    let uiconfID = 22119142;
    let entryId = "1_9y74c1on"; //"0_tc7ecjwi";

    let recaptchaScript = document.createElement("script");
    recaptchaScript.setAttribute(
      "src",
      `https://cdnapisec.kaltura.com/p/${partnerID}/sp/${partnerID}00/embedIframeJs/uiconf_id/${uiconfID}/partner_id/${partnerID}`
    );
    document.head.appendChild(recaptchaScript);

    let ref = this;
    setTimeout(() => {
      window.kWidget.embed({
        targetId: "myEmbedTarget",
        wid: `_${partnerID}`,
        uiconf_id: `${uiconfID}`,
        entry_id: `${entryId}`,
        flashvars: {
          // flashvars allows you to set runtime uiVar configuration overrides.
          autoPlay: false,
        },
        params: {
          // params allows you to set flash embed params such as wmode, allowFullScreen etc
          wmode: "transparent",
        },
        readyCallback: function (playerId) {
          var kdp = document.getElementById(playerId);

          ref.setStart(0);
          ref.setDuration(kdp.evaluate("{duration}"));

          //---------------
          ref.events.forEach((event) => {
            kdp.kBind(event.id, function () {
              ref.setEvent(event.id);
            });
          });

          //--------------

          kdp.kBind("doPlay", function () {
            let timestamp = kdp.evaluate("{video.player.currentTime}");
            ref.setStart(timestamp);
            ref.setPlayState(true);
          });

          kdp.kBind("playerPlayEnd", function () {
            let timestamp = kdp.evaluate("{video.player.currentTime}");
            if (ref.isPlaying) {
              ref.setEnd(timestamp);
              ref.setPlayState(false);
            }
          });
          kdp.kBind("durationChange", function (e) {
            console.log(e);
            ref.setDuration(e.newValue);
          });

          kdp.kBind("pause", function () {
            let timestamp = kdp.evaluate("{video.player.currentTime}");
            if (ref.isPlaying) {
              ref.setEnd(timestamp);
              ref.setPlayState(false);
            }
          });

          kdp.kBind("playerPlayEnd", function () {
            ref.setEnd(ref.duration);
          });

          kdp.kBind("play", function () {
            let timestamp = kdp.evaluate("{video.player.currentTime}");
            ref.setStart(timestamp);
            ref.setPlayState(true);
          });

          kdp.kBind("seek", function (timestamp) {
            if (ref.isPlaying) {
              ref.setEnd(timestamp);
            }
          });

          kdp.kBind("seeked", function (timestamp) {
            ref.setStart(timestamp);
          });
        },
      });
    }, 1000);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
