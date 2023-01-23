<template>
  <div
    class="flex flex-col flex-grow w-full max-w-xl bg-white shadow-xl rounded-lg overflow-hidden"
  >
    <div class="flex flex-col flex-grow h-0 p-4 overflow-auto chat-wrapper">
      <ChatBlock
        v-for="(message, index) in messages"
        :key="index"
        :message="message.message"
        :isUser="message.isUser"
      />
    </div>

    <div class="bg-gray-300 p-4">
      <input
        class="flex items-center h-10 w-full rounded px-3 text-sm"
        type="text"
        placeholder="Type your message…"
        v-model="message"
        @keyup.enter="sendMessage"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import ChatBlock, { type Props } from "./components/ChatBlock.vue";

const URL = "https://api.openai.com/v1/completions";
const TOKEN = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";
const prompt = ref(`
Our company is called Daweb Computers, we are located at 20 Regent St., St. James's, London SW1Y 4PH, United Kingdom.

We are open from Monday to Friday, from 9am until 7pm.

We sell computer parts. To get prices and availability for them, customers have to look on our website at www.dawebcomputers.com/products

We also fix computers and recover memory drives. For that service, customers have to book an appointment at www.dawebcomputers.com/book-appointment  before they come in the store, to avoid having to wait a long time in line.

our website is www.dawebcomputers.com

If customers have questions that can't be answered by you, tell them to call us at +44 20 2222 2222

Reply to our customers in our website's chat and give them instructions. Be friendly and try to help them as much as possible.

Provide short answer to fit a chat box.
`);

const messages = ref<Props[]>([
  {
    message: "Hi there! How can I help you?",
    isUser: false,
  },
]);
const message = ref("");

const sendMessage = async () => {
  messages.value.push({
    message: message.value,
    isUser: true,
  });
  const chatWrapper = document.querySelector(".chat-wrapper");
  setTimeout(() => {
    if (chatWrapper) chatWrapper.scrollTop = chatWrapper.scrollHeight;
  }, 0);

  prompt.value += `
  Human: ${message.value}
  AI:`;

  message.value = "";

  await fetch(URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${TOKEN}`,
    },
    body: JSON.stringify({
      prompt: prompt.value,
      model: "text-davinci-003",
      max_tokens: 150,
      temperature: 0.9,
      top_p: 1,
      frequency_penalty: 0.0,
      presence_penalty: 0.6,
      stop: [" Human:", " AI:"],
    }),
  })
    .then(async (res) => {
      const { choices } = await res.json();

      if (choices && choices.length) {
        messages.value.push({
          message: choices[0].text,
          isUser: false,
        });

        prompt.value += `\n${choices[0].text}`;

        setTimeout(() => {
          if (chatWrapper) chatWrapper.scrollTop = chatWrapper.scrollHeight;
        }, 0);
      }
    })
    .catch((err) => {
      console.log(err);
    });
};
</script>

<style scoped></style>
