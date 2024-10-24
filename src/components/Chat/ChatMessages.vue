<script setup>
import { ref, reactive, onMounted } from "vue";

const messages = reactive([]);

const fetchMessages = async () => {
  try {
    const res = await fetch("https://lab5-p379.onrender.com/api/v1/messages/");
    const data = await res.json();
    messages.splice(0, messages.length, ...data); 
  } catch (error) {
    console.error("Failed to fetch messages:", error);
  }
};

onMounted(fetchMessages);
</script>


<template>
  <div class="comments">
    <ul>
      <li v-for="message in messages" :key="message._id">
        <strong
          ><p>{{ message.user }}</p></strong
        >
        <p>{{ message.text }}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
