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
        <button data-filter="_1977" @click="renderFilter">_1977</button>
        <button data-filter="aden" @click="renderFilter">aden</button>
        <button data-filter="earlybird" @click="renderFilter">earlybird</button>
        <button data-filter="walden" @click="renderFilter">walden</button>
        <button data-filter="xpro2" @click="renderFilter">xpro2</button>
        <button data-filter="lofi" @click="renderFilter">lofi</button>
      </div>
    </div>
  </div>
</template>

<script>

import Plotit from '../libs/plotit.js';

export default {
  data() {
    return {
      image: ''
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
        vm.image = vm.plotit.image;
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
    resetImage: function(e) {
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
  .filter-btn-group button {
    margin: 5px;
    padding: 5px 10px;
    color: #303841;
    background-color: #08D9D6;
  }
</style>