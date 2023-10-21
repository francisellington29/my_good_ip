<script setup lang="ts">
import { ref } from 'vue'

const inputDomain = ref('')
const domain = ref()

const domainArray = (domainStr: string)=>{
    return  domainStr.split(',\n')
}

const removeDuplicates = (domainArr: Array<string>) =>{
  return Array.from(new Set(domainArr));
}

const configDomain = () =>{
    domain.value = removeDuplicates(domainArray(inputDomain.value)).join(',\n')
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
    <textarea v-model="inputDomain" placeholder="输入域名" rows="10"></textarea>
    <button @click="configDomain">过滤</button>
    <!-- <p>替换后的消息：</p> -->
    <div class="bubble-container">
      <textarea class="result" disabled>{{ domain }}</textarea>
      <button class="bubble" @click="copyToClipboard(domain)">复制</button>
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
input {
  border: 1px solid #ccc;
}
textarea {
  width: 600px;
  height: 200px;
  padding: 10px;
  resize: vertical;
  border: 1px solid #ccc;
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
