<script setup lang="ts">
import { ref } from 'vue'

const inputMsg = ref('')
const ip = ref('')
const newIp = ref('')
const replacedMsg = ref('')
const encodedString = ref('')

// const replaceIp = () => {
//   // 使用正则表达式进行全局替换
//   const regex = new RegExp(ip.value, 'g');
//   replacedMsg.value = inputMsg.value.replace(regex, newIp.value);
// }

const replaceIp = () => {
  // 按行分割文本并存储在数组中
  const nodeList = inputMsg.value.split('\n')

  // 遍历数组并替换每个 IP
  const replacedNodes = nodeList.map((node) => {
    // 使用正则表达式进行单行替换
    return node.split(ip.value).join(newIp.value)
  })

  // 将替换后的文本数组合并为一个字符串
  replacedMsg.value = replacedNodes.join('\n')
}

const transformer64 = () => {
  encodedString.value = btoa(inputMsg.value)
}

// 复制文本到剪贴板
const copyToClipboard = (text: string) => {
  const input = document.createElement('input')
  input.value = text
  document.body.appendChild(input)
  input.select()
  document.execCommand('copy')
  document.body.removeChild(input)
}
</script>

<template>
  <div class="container">
    <textarea v-model="inputMsg" placeholder="输入原始消息" rows="10"></textarea>
    <input v-model="ip" placeholder="输入需替换的ip" />
    <input v-model="newIp" placeholder="输入新ip" />
    <button @click="replaceIp">转换</button>
    <!-- <p>替换后的消息：</p> -->
    <div class="bubble-container">
      <textarea class="result" disabled>{{ replacedMsg }}</textarea>
      <button class="bubble" @click="copyToClipboard(encodedString)">复制</button>
    </div>

    <button @click="transformer64">Base64</button>

    <div class="bubble-container">
      <textarea class="result base64" disabled>{{ encodedString }}</textarea>
      <button class="bubble" @click="copyToClipboard(encodedString)">复制</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 20px;
}

textarea {
  width: 600px;
  height: 500px;
  padding: 10px;
  resize: vertical;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.result {
  font-family: monospace; /* 使用等宽字体以保留换行符等格式 */
  white-space: pre-wrap; /* 处理换行符 */
}
.base64 {
  height: 100px;
}

/* 添加冒泡效果的样式 */
.bubble-container {
  position: relative;
  display: inline-block;
}

.bubble {
  position: absolute;
  bottom: 10px;
  right: 10px;
  background-color: #007bff;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.bubble-container:hover .bubble {
  opacity: 1;
}
</style>
