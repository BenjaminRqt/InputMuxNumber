<template>
  <div class="input-nux-number">
    <label class="input-nux-number_label" :for="name" v-if="label"> {{ label }}</label>
    <input
      class="input-nux-number_input"
      type="text"
      :title="name"
      :name="name"
      :id="name"
      :value="valueSuffixed"
    >
    <span class="input-nux-number_icons">
            <button
              class="input-nux-number_min-button"
              @mousedown="down"
              @touchstart="down"
              @mouseleave="stop"
              @mouseup="stop"
              @touchend="stop"
              @touchcancel="stop"
            >-</button>
            <button
              class="input-nux-number_max-button"
              @mousedown="up"
              @touchstart="up"
              @mouseleave="stop"
              @mouseup="stop"
              @touchend="stop"
              @touchcancel="stop"
            >+</button>
        </span>
    <span class="input-nux-number_help" v-if="hasHelp">{{ textHelp }}</span>
  </div>
</template>
<script lang="ts">
/* eslint-disable */

  export default {
    name: 'InputMuxNumber',
    props: {
      name: {
        type: String,
        required: true,
      },
      min: {
        type: Number,
      },
      max: {
        type: Number,
      },
      label: {
        type: String,
      },
      speed: {
        type: Number,
        default: 100
      },
      help: {
        type: Boolean,
        default: true
      },
      suffix: {
        type: String
      },
      step: {
        type: Number,
        default: 1
      }
    },
    data() {
      return {
        value: 0,
        interval: false,
        counter: 0,
        hasHelp: this.help
      }
    },
    created() {
      if (this.min) {
        this.value = this.min;
      }
    },
    methods: {
      down() {
        if (!this.interval) {
          this.interval = setInterval(() => {
            if (this.respectMin()) {
              this.value -= this.getMultiplier();
              ++this.counter;
            } else {
              this.value = this.min;
            }
          }, this.speed);
        }
      },
      up() {
        if (!this.interval) {
          this.interval = setInterval(() => {
            if (this.respectMax()) {
              this.value += this.getMultiplier();
              ++this.counter;
            } else {
              this.value = this.max;
            }
          }, this.speed);
        }
      },
      stop() {
        clearInterval(this.interval);
        this.interval = false;
        this.counter = 0;
      },
      respectMin() {
        let value = this.value - this.getMultiplier();

        return this.min < value;
      },
      respectMax() {
        let value = this.value + this.getMultiplier();

        return this.max > value;
      },
      getMultiplier() {
        if (this.counter < 10) {
          return 1 * this.step;
        } else if (this.counter >= 10 && this.counter < 30) {
          return 10 * this.step;
        } else if (this.counter >= 30 && this.counter < 50) {
          return 100 * this.step
        } else {
          return 1000 * this.step;
        }
      }
    },
    computed: {
      textHelp() {
        if (this.min && !this.max) {
          return `+ de ${this.min}`;
        } else if (!this.min && this.max) {
          return `- de ${this.max}`;
        } else if (this.min && this.max) {
          return `entre ${this.min} et ${this.max}`;
        } else {
          this.hasHelp = false;
          return '';
        }
      },
      valueSuffixed() {
        return this.value + this.suffix
      }
    }
  };
</script>

<style lang="css" scoped>
  .input-nux-number > .icons i {
    font-weight: bold;
    cursor: pointer;
    font-size: 1.5rem;
    padding: 4px;
  }
</style>
