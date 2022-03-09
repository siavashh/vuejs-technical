<template>
  <div class="calculator">
    <header class="page-header">
      <h1 class="page-title">{{ data.title }}</h1>
      <p class="page-desc">{{ data.desc }}</p>
    </header>
    <div class="grid">
      <div class="slider-box col-desk-8 col-tab-7 col-mob-4">
        <Slider
          v-for="(value, key) in data.calculators"
          @slider-data="onSliderChange"
          :key="key"
          v-bind="value"
        />
      </div>
      <div class="result-box col-desk-4 col-tab-5 col-mob-4">
        <div class="mtb1">
          <h3 class="label">Hours saved</h3>
          <h3 class="result-value">{{ hoursCalc }}</h3>
        </div>
        <div class="mtb1">
          <h3 class="label">Money saved</h3>
          <h3 class="result-value">${{ moneyCalc }}</h3>
        </div>
        <button
          class="button button-primary button-big mt2 mb1 center fillparent"
          @click="onClick()"
        >
          Try waybook now
        </button>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, type PropType } from "vue";
import Slider from "@/components/TheSlider.vue";
import json from "../content/calculator.json";

export default defineComponent({
  data: () => {
    return {
      data: json,
      calculated: {
        salary: 0,
        people: 0,
        week: 0,
      },
    };
  },
  components: {
    Slider,
  },
  computed: {
    hoursCalc() {
      const { people, week } = this.calculated;
      return Math.round(people * week * 7 * 8);
    },
    moneyCalc() {
      const { salary, people, week } = this.calculated;
      return Math.round((salary / 48) * week * people * 1000)
        .toString()
        .replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  methods: {
    onSliderChange(sliderData: { id: any; value: any; type: any }) {
      const { id, value, type } = sliderData;
      if (type == "people") this.calculated.people = value;
      if (type == "salary") this.calculated.salary = value;
      if (type == "week") this.calculated.week = value;
    },
    onClick() {
      console.log("I think this is working good ;)");
    },
  },
});
</script>
<style scope lang="scss">
@import "@/assets/base.scss";
.page-header {
  text-align: center;
  margin: 0 auto;
  h1 {
    font-size: 3rem;
  }
  .page-desc {
    padding: 1em 0;
  }
}
.grid {
  background: cv("grey-25");
  justify-content: space-between;
  border-radius: $border-radius-default;
}
.slider-box {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  margin-right: -1em;
  padding: 2em;
}
.result-box {
  @include card(
    $padding: 1em 2em,
    $borderColor: cv("color-cyan"),
    $borderWidth: 2px,
    $background: cv("color-white")
  );
  .label {
    color: cv("color-black-mute");
    font-weight: 100;
    font-size: 0.9rem;
  }
  .result-value {
    font-size: 2rem;
  }
}
@media all and (max-width: 1090px) {
  .result-box {
    padding: 1em;
    .button {
      font-size: 1rem;
      line-height: 2rem;
    }
  }
}
@media all and (max-width: map-get($grid-breakpoints, md)) {
  .page-header {
    h1 {
      font-size: 2rem;
    }
  }
  .result-box {
    max-width: 60%;
    margin: 2em auto;
  }
  .slider-box {
    margin-right: 0;
  }
}

@media all and (max-width: map-get($grid-breakpoints, sm)) {
  .result-box {
    max-width: 100%;
    margin: 2em auto;
  }
}
</style>
