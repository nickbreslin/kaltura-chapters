<template>
  <div>Viewer.</div>
  <div id="myEmbedTarget" style="width: 400px; height: 330px"></div>

  <!-- <iframe
    id="kaltura_player"
    src="https://cdnapisec.kaltura.com/p/391241/sp/39124100/embedIframeJs/uiconf_id/22119142/partner_id/391241?iframeembed=true&playerId=kaltura_player&entry_id=0_tc7ecjwi&flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;flashvars[hotspots.plugin]=1&amp;flashvars[Kaltura.addCrossoriginToIframe]=true&amp;&wid=1_yxporfuz"
    width="480"
    height="304"
    allowfullscreen
    webkitallowfullscreen
    mozAllowFullScreen
    allow="autoplay *; fullscreen *; encrypted-media *"
    sandbox="allow-forms allow-same-origin allow-scripts allow-top-navigation allow-pointer-lock allow-popups allow-modals allow-orientation-lock allow-popups-to-escape-sandbox allow-presentation allow-top-navigation-by-user-activation"
    frameborder="0"
    title="Kaltura Player"
  ></iframe>-->
  <!--<script src="http://cdnapi.kaltura.com/p/{partner_id}/sp/{partnerId}00/embedIframeJs/uiconf_id/{uiconf_id}/partner_id/{partnerId}"></script>-->
</template>

<script>
export default {
  name: "Viewer",
  props: {
    title: {
      type: String,
    },
  },
  mounted() {
    let partnerID = 391241;
    let uiconfID = 22119142;
    let entryId = "0_tc7ecjwi";

    let recaptchaScript = document.createElement("script");
    recaptchaScript.setAttribute(
      "src",
      `https://cdnapisec.kaltura.com/p/${partnerID}/sp/${partnerID}00/embedIframeJs/uiconf_id/${uiconfID}/partner_id/${partnerID}`
    );
    document.head.appendChild(recaptchaScript);

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
          console.log("kWidget player ready: " + playerId);
          var kdp = document.getElementById(playerId);
          kdp.kBind("doPlay", function () {
            console.log("doPlay called on  " + playerId);
          });
        },
      });
    }, 1000);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
