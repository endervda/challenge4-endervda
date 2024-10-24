<script setup>
import { reactive, onMounted, ref } from "vue";

const name = "User";
const messages = reactive([]);
const message = ref("");

onMounted(() => {
  fetch("https://lab5-p379.onrender.com/api/v1/messages/")
    .then((res) => res.json())
    .then((data) => messages.push(...data));
});

// Verzend nieuw bericht
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
      if (data.status === "success" && data.data) {
        messages.push(data.data); // Voeg het nieuwe bericht toe
        message.value = "";
      }
    }
  } catch (error) {
    console.error("Error:", error);
  }
};
</script>

<template>
  <div class="chat-container">
    <div class="comments">
      <ul>
        <li v-for="msg in messages" :key="msg._id">
          <strong>{{ msg.user }}:</strong>
          <p>{{ msg.text }}</p>
        </li>
      </ul>
    </div>
    <div class="comment-section">
      <label for="message">{{ name }}:</label>
      <input
        v-model="message"
        type="text"
        placeholder="Type your message here..."
      />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style scoped>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
  background-color: #f5f5f5;
}

.chat-container {
  min-height: 78vh;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: rgb(226, 226, 226);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.comments {
  flex-grow: 1;
  overflow-y: auto;
  max-height: 620px;
  margin-bottom: 20px;
  padding-right: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

li strong {
  color: #4a4e69;
  font-size: 16px;
}

li p {
  margin: 5px 0 0 0;
  font-size: 14px;
  color: #333;
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #ff3b5c;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #6c757d;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.comment-section {
  display: flex;
  gap: 10px;
  align-items: center;
  margin-top: 10px;
}

input[type="text"] {
  flex-grow: 1;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 16px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease;
}

input[type="text"]:focus {
  border-color: #4a4e69;
  outline: none;
}
</style>
