<template>
    <div class="input-nux-number">
        <label :for="name" v-if="label"> {{ label }}</label>
        <input
                type="text"
                :title="name"
                :name="name"
                :id="name"
                :value="value"
        >
        <span class="icons">
            <button
                    @mousedown="down"
                    @touchstart="down"
                    @mouseleave="stop"
                    @mouseup="stop"
                    @touchend="stop"
                    @touchcancel="stop"
            >-</button>
            <button
                    @mousedown="up"
                    @touchstart="up"
                    @mouseleave="stop"
                    @mouseup="stop"
                    @touchend="stop"
                    @touchcancel="stop"
            >+</button>
        </span>
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
  },
  data() {
    return {
        value: 0,
        interval:false,
    }
  },
    created() {
        if(this.min) {
            this.value = this.min;
        }
    },
  methods: {
      down() {
          if(!this.interval){
              this.interval = setInterval(() => {
                  if(this.respectMin()) {
                      --this.value;
                  }
              }, 30);
          }
      },
      up(){
          if(!this.interval){
              this.interval = setInterval(() => {
                  if(this.respectMax()) {
                      ++this.value;
                  }
              }, 30);
          }
      },
      stop(){
          clearInterval(this.interval);
          this.interval = false
      },
      respectMin() {
          return this.min < this.value;
      },
      respectMax() {
          return this.max > this.value;
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
