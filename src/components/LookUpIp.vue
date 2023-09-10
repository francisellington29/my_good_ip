<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

const ipAddresses = ref('')

const ipInfoList = ref([])

const lookupIpAddresses = async () => {
  const ips = ipAddresses.value
    .trim()
    .split('\n')
    .map((ip) => ip.trim())

  const infoList = []

  for (const ip of ips) {
    try {
      const response = await axios.get(`https://ipinfo.io/${ip}/json`)

      infoList.push(response.data)
    } catch (error) {
      console.error(`查询IP地址 ${ip} 失败`, error)
    }
  }

  // 使用类型断言
  ipInfoList.value = infoList
}
</script>
<template>
  <div class="container">
    <h2>IP查询</h2>
    <div class="input-container">
      <textarea v-model="ipAddresses" placeholder="在此处输入IP地址列表，每行一个"></textarea>
      <button @click="lookupIpAddresses">查询</button>
    </div>
    <div class="ip_table" v-if="ipInfoList.length > 0">
      <v-table>
        <thead>
          <tr>
            <th class="text-center">IP</th>
            <th class="text-center">country</th>
            <th class="text-center">city</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(info, index) in ipInfoList" :key="index">
            <td class="text-center" v-if="ipAddresses">{{ info.ip }}</td>
            <td class="text-center" v-else>当前IP：{{ info.ip }}</td>
            <td class="text-center">{{ info.country }}</td>
            <td class="text-center">{{ info.city }}</td>
          </tr>
        </tbody>
      </v-table>
    </div>
    <div v-else>
      <p>--------某查到---------</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 600px;
  display: flex;
  flex-direction: column;
}
.input-container {
  margin-top: 20px;
}

textarea {
  width: 100%;
  height: 100px;
  padding: 10px;
  border: 1px solid #ccc;
  resize: none;
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
