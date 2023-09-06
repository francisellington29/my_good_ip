<script setup lang="ts">
import { ref } from 'vue'

const inputMsg = ref('')
const ip = ref('')
const newIp = ref('')
const replacedMsg = ref('')

// const replaceIp = () => {
//   // 使用正则表达式进行全局替换
//   const regex = new RegExp(ip.value, 'g');
//   replacedMsg.value = inputMsg.value.replace(regex, newIp.value);
// }

const replaceIp = () => {
  // 按行分割文本并存储在数组中
  const nodeList = inputMsg.value.split('\n');
  
  // 遍历数组并替换每个 IP
  const replacedNodes = nodeList.map(node => {
    // 使用正则表达式进行单行替换
    return node.split(ip.value).join(newIp.value)
  });

  // 将替换后的文本数组合并为一个字符串
  replacedMsg.value = replacedNodes.join('\n');
}
</script>

<template>
  <div class="container">
    <textarea v-model="inputMsg" placeholder="输入原始消息" rows="10"></textarea>
    <input v-model="ip" placeholder="输入需替换的ip" />
    <input v-model="newIp" placeholder="输入新ip" />
    <button @click="replaceIp">转换</button>
    <p>替换后的消息：</p>
    <textarea class="result">{{ replacedMsg }}</textarea>
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
  font-weight: bold;
}
</style>
