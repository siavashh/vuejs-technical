<template>
  <div class="slider-wrapper">
    <div class="slider-header flex">
      <h2 class="slider-title fill">{{ title }}</h2>
      <h3 class="slider-value">
        {{ type == "salary" ? `$${value}k` : value }}
      </h3>
    </div>
    <Slider
      v-model="value"
      :tooltips="tooltip"
      :min="min"
      :max="max"
      :step="step"
      @update="onSliderChange(value)"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Slider from "@vueform/slider";

export default defineComponent({
  name: "TheSlider",
  emits: ["slider-data"],
  props: {
    id: Number,
    title: String,
    type: String,
    symbol: String,
    min: Number,
    max: Number,
    step: Number,
    defaultValue: Number,
  },
  data() {
    return {
      value: this.defaultValue,
      tooltip: false,
    };
  },
  components: {
    Slider,
  },
  methods: {
    onSliderChange(value: any) {
      sendData(this);
    },
  },
  mounted: function () {
    sendData(this);
  },
});

const sendData = (slideData: any) => {
  slideData.$emit("slider-data", {
    id: slideData.$props.id,
    value: slideData.value,
    type: slideData.$props.type,
  });
};
</script>
<style src="@vueform/slider/themes/default.css"></style>
<style scope lang="scss">
@import "@/assets/base.scss";
.slider-header {
  align-items: center;
  .slider-title {
    font-size: 1rem;
    padding: 1em 0;
    font-weight: 700;
  }
  .slider-value {
    font-weight: 700;
  }
}
.slider-base {
  background: cv("grey-100");
}
.slider-connect {
  background: cv("color-cyan");
}
@media all and (max-width: map-get($grid-breakpoints, md)) {
  .slider-title {
    font-size: 1em;
  }
}
</style>
