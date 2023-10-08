<script setup lang="ts">
import { ref } from "vue";
import { invoke } from "@tauri-apps/api/tauri";

const greetMsg = ref("");
const greetMsg2 = ref("");
const name = ref("");

async function greet() {
  // Learn more about Tauri commands at https://tauri.app/v1/guides/features/command
  greetMsg.value = await invoke("greet", { name: name.value });
  greetMsg2.value = await invoke("bingo", { name: name.value });

  // 调用命令
  // 在应用窗口中右键，打开开发者工具
  // 你会看到控制台上输出了 "Hello, World!"！
  invoke('greet1', { name: name.value })
    // `invoke` 返回的是一个 Promise
    .then((response) => console.log(response))
}
</script>

<template>
  <form class="row" @submit.prevent="greet">
    <input id="greet-input" v-model="name" placeholder="Enter a name..." />
    <button type="submit">Greet</button>
  </form>

  <p>{{ greetMsg2 }}</p>
  <p>{{ greetMsg }}</p>
</template>
