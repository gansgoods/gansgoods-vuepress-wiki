<template>
  <div class="personality-cloud">
    <h2>我的性格关键词</h2>
    <div ref="container"></div>
  </div>
</template>

<script>
import {WordCloud} from '@antv/g2plot'; // 引入G2Plot的WordCloud组件

export default {
  name: 'PersonalityCloud',
  data() {
    return {
      personalityData: [], // 用于存储性格数据
    };
  },
  mounted() {
    this.fetchPersonalityData();
  }
  ,
  methods: {
    async fetchPersonalityData() {
      try {
        const response = await fetch('https://fc-mp-76e7e9ef-6ceb-4633-9fab-0d63c711a900.next.bspapp.com/getpersonlitydata');
        const data = await response.json();
        if (data.code === 200 && data.data) {
          this.personalityData = data.data.map(item => ({
            word: item.description, // 或者任何你希望显示的字段
            weight: 5, // 假设你有一个权重字段
            // 可以添加其他需要的字段
          }));
          this.renderWordCloud(); // 获取数据后渲染词云图
        } else {
          console.error('Failed to fetch personality data:', data.msg);
        }
      } catch (error) {
        console.error('Error fetching personality data:', error);
      }
    },
    renderWordCloud() {
      const chart = new WordCloud(this.$refs.container, {
        data: this.personalityData,
        wordField: 'word',
        weightField: 'weight',
        // 以下是一些词云图的配置项，可以根据需要调整
        wordStyle: {
          fontFamily: 'Verdana',
          fontSize: [8, 32],
          rotation: 0,
        },
        random: () => 0.5,
      });
      chart.render();
    },
  }
}
</script>

<style scoped>
.personality-cloud {
  /* 样式根据需要添加 */
}
</style>