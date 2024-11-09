


<template>
  <div class="file-color-picker">
    <input type="file" @change="handleFileChange" />
    <div class="color-container">
      <div v-if="colors.light" class="color-box">
        <h3>Extracted light Colors:</h3>
        <ul>
          <li v-for="(color, index) in colors.light" :key="index">
            <span>{{ index }} </span><span :style="{ color: color }">{{ color }}</span>
          </li>
        </ul>
      </div>

      <div v-if="colors.dark" class="color-box">
        <h3>Extracted dark Colors:</h3>
        <ul>
          <li v-for="(color, index) in colors.dark" :key="index">
            <span>{{ index }} </span><span :style="{ color: color }">{{ color }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
// importing as materialDynamicColors
import materialDynamicColors from "material-dynamic-colors";
export default {
  data() {
    return {
      colors: []
    };
  },
  methods: {
    async handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        try {
          const colors = await this.materialDynamicColors2(file);
          console.log(colors);

          this.colors = colors;
        } catch (error) {
          console.error("Error getting colors:", error);
        }
      } else {
        console.error("No file selected");
      }
    },
    async materialDynamicColors2(file) {
      // 这里需要替换为你的 materialDynamicColors 函数实现
      // 例如：return await someLibrary.materialDynamicColors(file);
      // 目前返回一个模拟的颜色数组
      let colors = await materialDynamicColors(file);
      return colors;
    }
  }
};
</script>
<style scoped>
.color-container {
  display: flex;
}

.color-box {
  margin-right: 20px;
  /* 可根据需要调整间距 */
}
</style>