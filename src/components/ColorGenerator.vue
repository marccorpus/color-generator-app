<template>
  <b-container>
    <Title :rgb="rgb" :light-or-dark="lightOrDark" />

    <b-row>
      <b-col sm="10" offset-sm="1" md="8" offset-md="2">
        <b-card class="card">
          <b-row class="my-3">
            <b-col cols="2">
              <label>Red</label>
            </b-col>
            <b-col cols="10">
              <b-form-input
                v-model="rgb.red"
                type="range"
                min="0"
                max="255"
                class="range"
              ></b-form-input>
            </b-col>
          </b-row>

          <b-row class="my-3">
            <b-col cols="2">
              <label>Green</label>
            </b-col>
            <b-col cols="10">
              <b-form-input
                v-model="rgb.green"
                type="range"
                min="0"
                max="255"
                class="range"
              ></b-form-input>
            </b-col>
          </b-row>

          <b-row class="my-3">
            <b-col cols="2">
              <label>Blue</label>
            </b-col>
            <b-col cols="10">
              <b-form-input
                v-model="rgb.blue"
                type="range"
                min="0"
                max="255"
                class="range"
              ></b-form-input>
            </b-col>
          </b-row>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import Title from "./Title.vue";

export default {
  data() {
    return {
      rgb: {
        red: 0,
        green: 0,
        blue: 0,
      },
    };
  },
  methods: {
    init() {
      const red = this.generateRandomNumber();
      const green = this.generateRandomNumber();
      const blue = this.generateRandomNumber();

      this.rgb = {
        red,
        green,
        blue,
      };
    },
    generateRandomNumber() {
      return Math.floor(Math.random() * 255) + 0;
    },
    setBackgroundColor({ red, green, blue }) {
      document.body.style.backgroundColor = `rgb(${red}, ${green}, ${blue})`;
    },
  },
  computed: {
    lightOrDark() {
      let color = `rgb(${this.rgb.red}, ${this.rgb.green}, ${this.rgb.blue})`;

      let r, g, b, hsp;

      if (color.match(/^rgb/)) {
        color = color.match(
          /^rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*(\d+(?:\.\d+)?))?\)$/
        );

        r = color[1];
        g = color[2];
        b = color[3];
      } else {
        color = +(
          "0x" + color.slice(1).replace(color.length < 5 && /./g, "$&$&")
        );

        r = color >> 16;
        g = (color >> 8) & 255;
        b = color & 255;
      }

      hsp = Math.sqrt(0.299 * (r * r) + 0.587 * (g * g) + 0.114 * (b * b));

      if (hsp > 127.5) {
        return "light";
      } else {
        return "dark";
      }
    },
  },
  watch: {
    rgb: {
      handler(value) {
        this.setBackgroundColor(value);
      },
      deep: true,
    },
  },
  components: {
    Title,
  },
  created() {
    this.init();
  },
};
</script>

<style scoped>
.card {
  box-shadow: 20px 20px 30px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
}
.range {
  width: 100%;
  margin-top: 4px;
}
</style>
