<script setup>
import { ref } from 'vue';
import html2canvas from 'html2canvas';
const game_name = ref(null);
const maker_name = ref('作者名');

const rows = 5;
const cols = 5;

// 初始化表格数据
const tableData = ref(
    Array.from({ length: rows }, () => Array.from({ length: cols }, () => ''))
);
const exportCardAsImage = () => {
  const cardElement = document.getElementById('element');
  if (!cardElement) return;

  // 创建一个新的canvas元素
  const canvas = document.createElement('canvas');
  const ctx = canvas.getContext('2d');

  // 设置canvas的大小与卡片相同
  canvas.width = cardElement.offsetWidth;
  canvas.height = cardElement.offsetHeight;

  // 将卡片内容绘制到canvas上
  ctx.fillStyle = 'white'; // 设置背景色
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  html2canvas(cardElement).then((canvas) => {
    // 将canvas转换为图片
    const dataUrl = canvas.toDataURL('image/png');
    // 创建一个链接并触发下载
    const link = document.createElement('a');
    link.href = dataUrl;
    link.download = 'bingo-card.png';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  });
};

</script>

<template>
  <h1>宾果游戏表格生成器</h1>
  <v-text-field label="游戏主题名" variant="outlined" v-model="game_name"></v-text-field>
  <v-text-field label="作者名" variant="outlined" v-model="maker_name"></v-text-field>
  <v-card
      variants="outlined"
      align="left"
      id="element"
      min-height="300px"
      min-width="950px"
  >
    <h3 style="text-indent: 1em;">{{game_name}}宾果游戏 (五个连成线你这辈子就有了)</h3>
    <div class="table-container">
      <table>
        <tbody>
        <tr v-for="row in rows" :key="`row-${row}`">
          <td v-for="col in cols" :key="`col-${col}`">
            <textarea v-model="tableData[row - 1][col - 1]" rows="1" @input="resizeTextarea($event)"></textarea>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
    <br>
    <div style="display: flex; justify-content: space-between; align-items: center;">
      <span>制作:{{maker_name}}</span>
      <p>Powered by Bingo Game Generator</p>
    </div>
  </v-card>
  <br>
  <v-btn @click="exportCardAsImage">导出为图片</v-btn>
  <br><br>
  晚江右海作品 2022-{{new Date().getFullYear()}} 基于MIT协议开源<br>
  <a href="https://github.com/YuxiangWang0525/bingo-game-generator" target="_blank">GitHub</a>
  <br>
</template>

<style scoped>
.table-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

table {
  border-collapse: collapse;
  border: 2px solid black; /* 为表格添加黑色边框 */
}

td {
  border: 2px solid black; /* 将边框设置为黑色并加粗 */
  padding: 8px;
  text-align: center;
}

textarea {
  width: 100%;
  box-sizing: border-box;
  border: none;
  outline: none;
  text-align: center;
  resize: none; /* 禁止用户调整大小 */
  overflow: hidden; /* 隐藏滚动条 */
}
</style>

<script>
export default {
  methods: {
    resizeTextarea(event) {
      event.target.style.height = 'auto';
      event.target.style.height = `${event.target.scrollHeight}px`;
    }
  }
}
</script>
