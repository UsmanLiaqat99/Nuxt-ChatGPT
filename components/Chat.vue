<template>
  <div class="main">
    <!-- loader -->

    <div class="chat-container">
      <div v-if="loading" class="loader">
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
        <div class="loader__circle"></div>
      </div>
      <header class="chat-header">Chat with Nuxt-ChatGPT</header>
      <div class="message-display">
        <div v-if="!answers.length" class="message-bubble">
          Hi, I'm Nuxt ChatGPT. I'm a chatbot powered by OpenAI's GPT-3. Ask me
          anything!
        </div>
        <div class="message-bubble" v-for="(answer, i) in answers" :key="i">
          {{ answer }}
        </div>
      </div>
      <div class="input-section">
        <input
          v-model="question"
          type="text"
          placeholder="Type your message here..."
        />
        <button @click="sendMessage">Send</button>
      </div>
    </div>
  </div>
</template>

<script>
import { Configuration, OpenAIApi } from "openai";
import { CHATGPT_API_KEY } from "@/utils/local.js";
export default {
  data() {
    return {
      loading: false,
      question: "",
      answers: [],
    };
  },
  methods: {
    async sendMessage() {
      this.loading = true;
      const configuration = new Configuration({
        apiKey: CHATGPT_API_KEY,
      });
      const openai = new OpenAIApi(configuration);

      const response = await openai.createCompletion({
        model: "text-davinci-003",
        prompt: this.question,
        max_tokens: 100,
        temperature: 0.5,
      });
      this.loading = false;
      this.question = "";
      this.answers.push(response.data.choices[0].text);
    },
  },
};
</script>

<style scoped>
/* Loader */
.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  top: 50%;
  left: 0;
  z-index: 9999;
}

.loader__circle {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: #007bff;
  margin: 0 5px;
  animation: loader 1s infinite;
}

.loader__circle:nth-child(2) {
  animation-delay: 0.1s;
}

.loader__circle:nth-child(3) {
  animation-delay: 0.2s;
}

.loader__circle:nth-child(4) {
  animation-delay: 0.3s;
}

.loader__circle:nth-child(5) {
  animation-delay: 0.4s;
}

.loader__circle:nth-child(6) {
  animation-delay: 0.5s;
}

.loader__circle:nth-child(7) {
  animation-delay: 0.6s;
}

.loader__circle:nth-child(8) {
  animation-delay: 0.7s;
}

@keyframes loader {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}

.main {
  display: grid;
  place-items: center;
  height: 100vh;
}
.chat-container {
  width: 100%;
  height: 90%;
  max-width: 800px;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.2);
  padding: 10px;
}

.chat-header {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  margin: 10px 0px 20px 0px;
}

.message-display {
  height: 80%;
  overflow-y: scroll;
}

.message-bubble {
  width: 95%;
  background-color: #f2f2f2;
  border-radius: 10px;
  padding: 10px;
  margin-bottom: 10px;
}

.input-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px 10px 10px 10px;
}

input[type="text"] {
  width: 80%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

button {
  width: 15%;
  padding: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

/* Media Queries */
@media (max-width: 500px) {
  .chat-container {
    height: 100%;
  }

  .message-bubble {
    width: 90%;
  }

  input[type="text"] {
    width: 70%;
  }

  button {
    width: 20%;
  }
}
</style>
