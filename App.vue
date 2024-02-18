<template>
  <div>
    <div class="color-inputs">
      <ColorInput label="Foreground Color " v-bind:color="foreground" @update:color="updateForeground" />
      <ColorInput label="Background Color " v-bind:color="background" @update:color="updateBackground" />
    </div>
    <div class="results">
      <div class="ratio">
        <div class="headers">Contrast Ratio</div>
        <p class="answers">{{ checkContrastRatio() }}:1</p>
      </div>
      <div class="headers">WCAG Levels</div>
      <div class="wcag-levels">
        <div class="wcag">
          <p class="answers">AA: {{ checkWCAG_AA() ? '✓' : '✗' }}</p>
        </div>
        <div class="wcag">
          <p class="answers">AAA: {{ checkWCAG_AAA() ? '✓' : '✗' }}</p>
        </div>
      </div>
    </div>
    <div class="preview-section">
      <div class="preview" v-bind:style="{ color: foreground, backgroundColor: background }">
        This is just preview TEXT
      </div>
    </div>
  </div>
</template>

<script>
import ColorInput from './components/ColorInput.vue';
import { wcagContrastChecker } from '@mdhnpm/wcag-contrast-checker';
import { colorContrastRatioCalculator } from '@mdhnpm/color-contrast-ratio-calculator';

export default {
  components: {
    ColorInput,
  },
  data() {
    return {
      foreground: '#FFFFFF',
      background: '#000000',
      contrastResults: null,
    };
  },
  methods: {
    updateForeground(color) {
      this.foreground = color;
      this.checkWCAG_AA()
      this.checkWCAG_AAA()
    },
    updateBackground(color) {
      this.background = color;
      this.checkWCAG_AA()
      this.checkWCAG_AAA()
    },
    checkWCAG_AA() {
      const WCAG_AA = wcagContrastChecker(this.foreground, this.background).regularText.aa;
      return WCAG_AA;
    },
    checkWCAG_AAA() {
      const WCAG_AAA = wcagContrastChecker(this.foreground, this.background).regularText.aaa;
      return WCAG_AAA;
    },
    checkContrastRatio() {
      const ContrastRatio = colorContrastRatioCalculator(this.foreground, this.background);
      return ContrastRatio;
    }

  }
}
</script>

<style scoped>
.color-inputs {
  display: flex;
}

.color-inputs>div {
  flex: 1;
  margin-right: 10px;
}

.preview-section {
  margin-top: 20px;
}

.preview {
  padding: 20px;
  font-size: 40px;
  text-align: center;
  border-radius: 10px;
}

.results {
  margin-top: 30px;
  padding-bottom: 30px;
}

.headers {
  text-align: center;
  font-size: 30px;
  padding-bottom: 10px;
}

.answers {
  text-align: center;
  font-size: 60px;
  line-height: 1;
}

.ratio {
  margin-bottom: 60px;
}

.wcag-levels {
  display: flex;
  justify-content: space-between;
}

.wcag {
  width: 50%;
  text-align: center;
}
</style>
