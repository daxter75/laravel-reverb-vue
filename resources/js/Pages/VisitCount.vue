<script setup>
import { ref } from 'vue';

const messages = ref([]);
const message = ref("");

Echo.private("messages").listen("MessageReceived", (e) => {
  if(!messages.value.find((m) => m.id === e.id)) {
    messages.value.push(e);
  }  
})

function handleSubmit() {
  const msg = {
    id: crypto.randomUUID(),
    message: message.value
  }
  messages.value.push({
    ...msg,
    who: "Me",
  });
  axios.post("/messages", msg);
}

</script>

<template>
  <ul>
    <li v-for="message in messages" :key="message.id">
      {{ message.message }} - {{ message.who }}
    </li>
  </ul>
  <form @submit.prevent="handleSubmit">
    <textarea v-model="message" cols="30" rows="10"></textarea>
    <button>Send Message</button>
  </form>
</template>