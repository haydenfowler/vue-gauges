<script>
  export default {
    name: 'Gauge',
    props: {
      faceColor: {
        default: 'lightgrey',
        type: String,
      },
      finishOffset: {
        default: 30,
        type: Number,
      },
      gaugeColor: {
        default: 'blue',
        type: String,
      },
      max: {
        type: [Number, String],
        required: true,
      },
      min: {
        type: [Number, String],
        required: true,
      },
      startOffset: {
        default: 30,
        type: Number,
      },
      value: {
        type: [Number, String],
        required: true,
      },
    },
    mounted() {
      this.drawMarkers();
    },
    computed: {
      gaugeRange() {
        return 360 - (this.finishOffset + this.startOffset);
      },
      gaugeRotation() {
        const { min, max, value } = this;
        const percentage = (value - min) / (max - min);
        return this.startOffset + (this.gaugeRange * percentage);
      }
    },
    methods: {
      drawMarkers() {
        const { markers } = this.$refs;
        markers.height = markers.clientHeight;
        markers.width = markers.clientWidth;
        const radius = markers.height / 2;
        const context = markers.getContext("2d");

        const defaultMarketLength = radius / 15;
        const mediumMarkerLength = radius / 10;
        const largeMarkerLength = radius / 7;

        context.font = radius * 0.06 + "px arial";
        context.textBaseline = "middle";
        context.textAlign = "center";
        context.translate(markers.width / 2, markers.height / 2);
        context.rotate(Math.PI);

        for(let num = this.min; num <= this.max; num++){
          const percentage = ((num / (this.max - this.min)) * this.gaugeRange) + this.startOffset ;
          const angle = percentage * Math.PI / 180;

          // Rotate to the right position & move the cursor outwards
          context.rotate(angle);
          context.translate(0, -radius);

          // Draw the marker
          let markerLength = defaultMarketLength;
          if (num % 10 === 0) {
            markerLength = largeMarkerLength;
            context.fillText(num.toString(), 0, markerLength * 1.25);
          } else if (num % 5 === 0) {
            markerLength = mediumMarkerLength;
          }
          context.fillRect(0, 0, 1, markerLength);

          // Rotate back to the default position
          context.translate(0, radius);
          context.rotate(-angle);

          // const ang = num * Math.PI / 12;
          // context.rotate(ang);
          // context.translate(0, -radius);
          // context.rotate(-ang);
          // context.fillText(num.toString(), 0, 0);
          // context.rotate(ang);
          // context.translate(0, radius);
          // context.rotate(-ang);
        }
      },
    },
  }
</script>

<template>
  <div class="container">
    <div
        class="face"
        v-bind:style="{ backgroundColor: faceColor }">
      <div class="gauge-container">
        <canvas class="markers" ref="markers" />
        <div
            class="gauge"
            v-bind:style="{ transform: `rotateZ(${this.gaugeRotation}deg)` }"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
}

.face {
  position: relative;
  border-radius: 50%;
  border-width: 1px;
  height: 100%;
  width: 100%;
}

.face:after {
  content: "";
  position: absolute;
  background: black;
  border-radius: 50%;
  width: 5%;
  height: 5%;
  top: 47.5%;
  left: 47.5%;
}

.gauge-container {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.markers {
  position: relative;
  width: 100%;
  height: 100%;
}

.gauge {
  position: absolute;
  background: #000;
  height: 44%;
  left: 49%;
  top: 50%;
  width: 2%;
  transform-origin: 50% 0%;
  transition: all 0.1s ease;
  clip-path: polygon(0 0, 100% 0, 60% 100%, 40% 100%);
}
</style>