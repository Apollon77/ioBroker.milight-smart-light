<template>
  <v-container fill-height fluid grid-list-xl>
    <v-layout column>
      <v-flex xs6>
        <v-layout row>
          <v-flex xs6 text-xs-center>
            <msl-slider
              style="padding: 0;"
              class="d-inline-flex"
              :value="whiteTemperature"
              v-bind="optionsWhiteTemperature"
              @callback="wt"
            />
          </v-flex>
          <v-flex xs6 text-xs-center>
            <msl-slider
              style="padding: 0;"
              class="d-inline-flex"
              :value="brightness"
              v-bind="optionsBrightness"
              @callback="b"
            />
          </v-flex>
        </v-layout>
      </v-flex>

      <v-flex xs3 d-inline-flex align-center>
        <v-layout row justify-space-around text-xs-center>
          <v-flex xs6>
            <v-btn
              round
              large
              color="white black--text"
              @click="whiteMode()"
            >
              <v-icon left dark>
                wb_sunny
              </v-icon>
              white
            </v-btn>
          </v-flex>
          <v-flex xs6>
            <v-btn
              round
              large
              color="black white--text"
              @click="nightMode()"
            >
              <v-icon left dark>
                cloud
              </v-icon>
              night
            </v-btn>
          </v-flex>
        </v-layout>
      </v-flex>

      <v-flex xs3 d-inline-flex align-end>
        <msl-on-off />
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import mslSlider from 'vue-slider-component'
import mslOnOff from '@/components/OnOff'
import { mapGetters, mapActions } from 'vuex'

export default {
  components: {
    mslSlider,
    mslOnOff
  },
  data() {
    return {
      optionsWhiteTemperature: {
        width: 90,
        height: 280,
        dotHeight: 32,
        dotWidth: 86,
        min: 2700,
        max: 6500,
        interval: 100,
        tooltip: false,
        direction: 'vertical',
        processStyle: {
          backgroundColor: 'transparent'
        },
        bgStyle: {
          borderRadius: '5px',
          backgroundImage: '-webkit-linear-gradient(bottom, #ffffff, #0000ff)',
          boxShadow: 'inset 0.5px 0.5px 3px 1px rgba(0,0,0,.36)'
        },
        sliderStyle: {
          borderRadius: '5px',
          backgroundColor: 'transparent',
          boxShadow: '1px 1px 8px 1px rgba(0, 0, 0, 0.6)'
        }
      },
      optionsBrightness: {
        width: 90,
        height: 280,
        dotHeight: 32,
        dotWidth: 86,
        tooltip: false,
        direction: 'vertical',
        processStyle: {
          backgroundColor: 'transparent'
        },
        bgStyle: {
          borderRadius: '5px',
          backgroundImage: '-webkit-linear-gradient(bottom, #000000, #ffffff)',
          boxShadow: 'inset 0.5px 0.5px 3px 1px rgba(0,0,0,.36)'
        },
        sliderStyle: {
          borderRadius: '5px',
          backgroundColor: 'transparent',
          boxShadow: '1px 1px 8px 1px rgba(0, 0, 0, 0.6)'
        }
      }
    }
  },
  computed: {
    ...mapGetters(['LOADED_ZONE', 'DPS']),
    brightness() {
      return this.DPS[this.LOADED_ZONE._id + '.brightness'].val
    },
    whiteTemperature() {
      return this.DPS[this.LOADED_ZONE._id + '.whiteTemperature'].val
    }
  },
  methods: {
    ...mapActions(['UPDATE_DP_FROM_CLIENT', 'UPDATE_DP']),
    b(options) {
      this.UPDATE_DP_FROM_CLIENT({
        value: options,
        dp: 'brightness',
        delay: 500
      })
    },
    wt(options) {
      this.UPDATE_DP_FROM_CLIENT({
        value: options,
        dp: 'whiteTemperature',
        delay: 500
      })
    },
    nightMode() {
      this.UPDATE_DP_FROM_CLIENT({ value: true, dp: 'nightMode', delay: 500 })
    },
    whiteMode() {
      this.UPDATE_DP_FROM_CLIENT({ value: true, dp: 'whiteMode', delay: 500 })
    }
  }
}
</script>

<style scoped>
#slider-white-temperature .rs-bg-color {
  background-color: #303030;
}

#slider-white-temperature .rs-handle {
  background-color: #bbdefb;
  padding: 7px;
  border: 2px solid #c2e9f7;
}

#slider-white-temperature .rs-handle.rs-focus {
  border-color: #c2e9f7;
}

#slider-white-temperature .rs-handle:after {
  border-color: #1e88e5;
  background-color: #1e88e5;
}

#slider-white-temperature .rs-border {
  border-color: #1e88e5;
}

#slider-white-temperature .rs-range-color {
  background-color: #1e88e5;
}

#slider-white-temperature .slider-tooltip {
  color: #1e88e5;
}
</style>
