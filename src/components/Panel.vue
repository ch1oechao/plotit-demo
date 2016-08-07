<template>
  <div class="panel-container">
    <div class="panel-content">
    </div>
    <div v-if="!image" class="upload-group">
      <span class="upload-btn">上传图片</span>
      <input type="file" @change="onFileChange">
    </div>
    <div v-else class="upload-group">
      <button @click="resetImage">还原图片</button>
      <button @click="removeImage">移除图片</button>
      <div class="filter-btn-group">
        <button data-filter="moon" @click="renderFilter">Moon</button>
        <button data-filter="toaster" @click="renderFilter">Toaster</button>
        <button data-filter="_1977" @click="renderFilter">1977</button>
        <button data-filter="aden" @click="renderFilter">Aden</button>
        <button data-filter="earlybird" @click="renderFilter">Earlybird</button>
        <button data-filter="walden" @click="renderFilter">Walden</button>
        <button data-filter="xpro2" @click="renderFilter">X-pro II</button>
        <button data-filter="lofi" @click="renderFilter">Lo-Fi</button>
      </div>
      <ul class="adjuster-input-group">
        <li class="adjuster-item">
          <span class="adjuster-label">brightness: </span><input type="range" data-adjuster="brightness" v-model="vBrightness" step="10" min="-50" max="50" />
        </li>
        <li class="adjuster-item">
          <span class="adjuster-label">saturation: </span><input type="range" data-adjuster="saturation" v-model="vSaturation" step="10" min="-50" max="50" />
        </li>
        <li class="adjuster-item">
          <span class="adjuster-label">contrast: </span><input type="range" data-adjuster="contrast" v-model="vContrast" step="10" min="-50" max="50" />
        </li>
        <li class="adjuster-item">
          <span class="adjuster-label">sepia: </span><input type="range" data-adjuster="sepia" v-model="vSepia" step="1" min="0" max="100" />
        </li>
        <li class="adjuster-item">
          <span class="adjuster-label">noise: </span><input type="range" data-adjuster="noise" v-model="vNoise" step="1" min="0" max="10" />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

import Plotit from '../libs/plotit.js';

export default {
  data() {
    return {
      image: '',
      vBrightness: 0,
      vSaturation: 0,
      vContrast: 0,
      vSepia: 0,
      vNoise: 0
    }
  },
  watch: {
    vBrightness: function(newVal, oldVal) {
      let val = 0;
      val = +newVal === 0 ? 0 : newVal - oldVal;
      this.renderAdjuster('brightness', val)
    },
    vSaturation: function(newVal, oldVal) {
      let val = 0;
      val = +newVal === 0 ? 0 : oldVal - newVal;
      this.renderAdjuster('saturation', val)
    },
    vContrast: function(newVal, oldVal) {
      let val = 0;
      val = +newVal === 0 ? 0 : newVal - oldVal;
      this.renderAdjuster('contrast', val)
    },
    vSepia: function(newVal, oldVal) {
      let val = 0;
      val = +newVal === 0 ? 0 : newVal - oldVal;
      this.renderAdjuster('sepia', val)
    },
    vNoise: function(newVal, oldVal) {
      this.renderAdjuster('noise', newVal)
    }
  },
  methods: {
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length)
        return;
      this.createImage(files[0]);
    },
    createImage(file) {
      var image = new Image();
      var reader = new FileReader();
      var vm = this;

      this.plotit = new Plotit({
        selector: '.panel-content'
      });

      reader.onload = (e) => {
        vm.plotit.renderImage(e.target.result)
        vm.image = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    removeImage: function(e) {
      this.image = '';
      this.plotit.removeImage();
    },
    renderFilter: function(e) {
      let plotit = this.plotit,
          filter = e.currentTarget.dataset.filter;
      plotit.processFilter(filter, plotit.$canvas);
    },
    renderAdjuster: function(adjuster, val) {
      let plotit = this.plotit;
      plotit.processPixel(adjuster, val);
    },
    resetImage: function() {
      this.vBrightness = 0;
      this.vSaturation = 0;
      this.vContrast = 0;
      this.vSepia = 0;
      this.vNoise = 0;      
      this.plotit.resetImage();
    }
  }
}
</script>

<style scoped>
.panel-container {
  margin: 20px auto;
  width: 80%;
  height: 100%;
}
.panel-content {
  width: 100%;
  max-height: 500px;
  max-width: 650px;
  margin: auto;
  display: block;
}
input[type="file"],
button {
  margin: 10px;
  padding: 10px 15px;
  border: 0;
  border-radius: 5px;
  outline: none;
  color: #FFF;
  font-size: 14px;
  background-color: #FC5185;
}
input[type="file"] {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  opacity: 0;
}
.upload-group {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  margin: 30px auto;
}
.upload-btn {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  display: inline-block;
  width: 240px;
  height: 30px;
  line-height: 30px;
  padding: 5px 8px;
  text-align: center;
  border-radius: 5px;
  font-size: 14px;
  color: #FFF;
  background-color: #FC5185;
}
.filter-btn-group {
  margin: 25px auto;
}
.filter-btn-group button {
  margin: 5px;
  padding: 5px 10px;
  color: #303841;
  background-color: #08D9D6;
}
.filter-btn-group button:active {
  color: #FFF;
  background-color: #08D9D6;
}
.adjuster-input-group {
  display: block;
  list-style: none;
}
.adjuster-item {
  width: 100%;
}
.adjuster-label {
  display: inline-block;
  width: 80px;
  text-align: right;
  margin-right: 1em;
  line-height: 2em;
}
input[type=range] {
  position: relative;
  top: -3px;
  width: 300px;
  background-color: #4D606E;
  height: 1px;
  vertical-align: 1px;
  -webkit-appearance: none;
  outline: none;
  border: none;

  &:hover {
    cursor: pointer;
  }
}

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  border: none;
  height: .7em;
  width: .7em;
  background: #08D9D6;
  cursor: pointer;
}
</style>