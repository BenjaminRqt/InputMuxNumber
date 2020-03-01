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
        <span class="help" v-if="hasHelp">{{ textHelp }}</span>
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
                default: 60
            },
            help: {
                type: Boolean,
                default: true
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
                        if (!this.respectMin(this.value - this.getMultiplier())) {
                            this.value = this.min;
                        } else if (this.respectMin()) {
                            this.value -= this.getMultiplier();
                            ++this.counter;
                        }
                    }, this.speed);
                }
            },
            up() {
                if (!this.interval) {
                    this.interval = setInterval(() => {
                        if (!this.respectMax(this.value + this.getMultiplier())) {
                            this.value = this.max;
                        } else if (this.respectMax()) {
                            this.value += this.getMultiplier();
                            ++this.counter;
                        }
                    }, this.speed);
                }
            },
            stop() {
                clearInterval(this.interval);
                this.interval = false;
                this.counter = 0;
            },
            respectMin(value = null) {
                if (value) {
                    return this.min < value;
                }
                return this.min < this.value;
            },
            respectMax(value = null) {
                if (value) {
                    return this.max > value;
                }
                return this.max > this.value;
            },
            getMultiplier() {
                if (this.counter < 10) {
                    return 1;
                } else if (this.counter >= 10 && this.counter < 30) {
                    return 10;
                } else if (this.counter >= 30 && this.counter < 50)  {
                    return 100
                } else {
                    return 1000;
                }
            }
        },
        computed: {
            textHelp() {
                if(this.min && !this.max) {
                    return `+ de ${this.min}`;
                } else if(!this.min && this.max) {
                    return `- de ${this.max}`;
                } else if(this.min && this.max) {
                    return `entre ${this.min} et ${this.max}`;
                } else {
                    this.hasHelp = false;
                    return '';
                }
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
