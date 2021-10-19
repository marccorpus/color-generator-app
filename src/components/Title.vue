<template>
  <b-row class="text-center mb-4">
    <b-col :style="titleStyle">
      <h1>Color Generator</h1>
      <h5>
        RGB: {{ rgbValue }}
        <b-button
          v-b-tooltip.hover
          title="Copy to clipboard"
          size="sm"
          class="btn shadow-none"
          :style="btnStyle"
          @click="copyToClipboard({ type: 'RGB', value: rgbValue })"
        >
          <b-icon icon="files" />
        </b-button>
      </h5>

      <h5>
        Hex: {{ hexValue }}
        <b-button
          v-b-tooltip.hover
          title="Copy to clipboard"
          size="sm"
          class="btn shadow-none"
          :style="btnStyle"
          @click="copyToClipboard({ type: 'Hex', value: hexValue })"
        >
          <b-icon icon="files" />
        </b-button>
      </h5>
    </b-col>
  </b-row>
</template>

<script>
export default {
  props: ["rgb", "lightOrDark"],
  computed: {
    titleStyle() {
      const color = this.lightOrDark === "light" ? "#000" : "#fff";
      return { color };
    },
    btnStyle() {
      let backgroundColor = "#fff";
      let color = "#000";

      if (this.lightOrDark === "light") {
        backgroundColor = "#000";
        color = "#fff";
      }

      return { backgroundColor, color };
    },
    rgbValue() {
      const { red, green, blue } = this.rgb;

      return `rgb(${red}, ${green}, ${blue})`;
    },
    hexValue() {
      const { red, green, blue } = this.rgb;

      return `#${this.getHex(red)}${this.getHex(green)}${this.getHex(blue)}`;
    },
  },
  methods: {
    toast({ title, message, variant }) {
      this.$bvToast.toast(message, {
        title,
        variant,
        solid: true,
        autoHideDelay: 2000,
      });
    },
    copyToClipboard({ type, value }) {
      navigator.clipboard
        .writeText(value)
        .then(() => {
          const title = "Success";
          const message = `${type} color has been copied to clipboard.`;
          const variant = "warning";

          this.toast({ title, message, variant });
        })
        .catch(() => {
          const title = "Error";
          const message = "Something went wrong.";
          const variant = "danger";

          this.toast({ title, message, variant });
        });
    },
    getHex(color) {
      let hex = Number(color).toString(16);

      if (hex.length < 2) {
        hex = "0" + hex;
      }
      return hex;
    },
  },
};
</script>

<style scoped>
.btn {
  border: none;
}
</style>
