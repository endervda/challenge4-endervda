<script setup>
import { ref, reactive, onMounted } from "vue";

const name = "User";
const message = ref("");
const messages = reactive([]);

// Functie om berichten op te halen
const fetchMessages = async () => {
  try {
    const res = await fetch("https://lab5-p379.onrender.com/api/v1/messages/");
    const data = await res.json();
    messages.splice(0, messages.length, ...data); 
  } catch (error) {
    console.error("Failed to fetch messages:", error);
  }
};

// Direct bijladen na een nieuw bericht
const sendMessage = async () => {
  if (!message.value.trim()) return;

  const newMessage = { user: name, text: message.value };

  try {
    const response = await fetch(
      "https://lab5-p379.onrender.com/api/v1/messages/",
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newMessage),
      }
    );

    if (response.ok) {
      const data = await response.json();
      messages.push(data); // Voeg het nieuwe bericht direct toe
      message.value = ""; // Clear het invoerveld
    } else {
      console.error("Failed to send message.");
    }
  } catch (error) {
    console.error("Error:", error);
  }
};

// Laad berichten op het moment van mount
onMounted(fetchMessages);
</script>

<template>
  <div>
    <!-- Input veld en verzendknop -->
    <div>
      <input
        v-model="message"
        type="text"
        placeholder="Type your message here..."
      />
      <button @click="sendMessage">Send</button>
    </div>

    <!-- Berichten lijst -->
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
  </div>
</template>

<style scoped>


</style>
