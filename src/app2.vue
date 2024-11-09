<template>
  <div id="app">
    <input type="file" ref="inputfile" @change="fileChange" />
    <img :src="imgUrl" style="height: 300px;width: 300px;" @load="onImageLoad" />
  </div>

  <div v-if="showTheme" class="color-box">
    <h3>Extracted light Colors:</h3>
    <ul>
      <li v-for="(color, index) in showTheme.schemes.light.toJSON()" :key="index">
        <span>{{ index }} </span> : <span :style="{ color: toHex(color) }">{{ toHex(color) }}</span>

      </li>
    </ul>
  </div>
</template>

<script setup  lang="ts">
import { onMounted, ref } from 'vue';
import {argbFromHex, themeFromImage,themeFromSourceColor} from "@material/material-color-utilities";

const imgUrl = ref<string>('');
const showTheme = ref();
const imgLoaded = ref(false);

const fileChange = (event: Event) => {
  const input = event.target as HTMLInputElement;
  if (input.files && input.files[0]) {
    const file = input.files[0];
    const reader = new FileReader();

    reader.readAsDataURL(file);
    reader.onloadend = (e) => {
      const target = e.target as FileReader;
      imgUrl.value = target.result as string;
      imgLoaded.value = false;
    };
  }
};

const onImageLoad = () => {
  imgLoaded.value = true;
  getTheme();
};

//根据图片获取主题
const getTheme = async () => {
  // 获取 img 对象  
  const img = document.querySelector('img');

  if (img && imgLoaded.value) {
    try {
      const theme = await themeFromImage(img, []);
      showTheme.value = theme;
    } catch (error) {
      console.error('Error generating theme from image:', error);
    }
  } else {
    console.error('Image not loaded or not found.');
  }
};

onMounted(() => {
  const theme = themeFromSourceColor(argbFromHex('#FF00FF'), []);
  showTheme.value = theme;
});


//十进制转换为十六进制，在前面加一个#号
const toHex = (color: number) => {
  return '#' + color.toString(16);
};
</script>

<style scoped>
/* Add your styles here */
</style>
