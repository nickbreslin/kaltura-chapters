<template>
  <div id="myEmbedTarget" style="width: 100%; height: 600px"></div>
</template>

<script>
export default {
  name: "Viewer",
  components: {},
  data: function () {
    return {
      kdp: null,
    };
  },
  props: {
    seconds: Number,
  },
  watch: {
    seconds() {
      console.log("calling...", this.seconds);
      this.kdp.sendNotification("doSeek", this.seconds);
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
          ref.kdp = document.getElementById(playerId);
        },
      });
    }, 1000);
  },
};
</script>
