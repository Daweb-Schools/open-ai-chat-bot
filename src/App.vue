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
        :time="message.time"
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
import { ref } from "vue";
import ChatBlock, { type Props } from "./components/ChatBlock.vue";

const messages = ref<Props[]>([
  {
    message: "Hi there! How can I help you?",
    time: new Date(new Date().setMinutes(-10)),
    isUser: false,
  },
  {
    message: "I would like to know more about your services.",
    time: new Date(new Date().setMinutes(-5)),
    isUser: true,
  },
]);
const message = ref("");

const sendMessage = () => {
  messages.value.push({
    message: message.value,
    time: new Date(),
    isUser: true,
  });
  message.value = "";
  const chatWrapper = document.querySelector(".chat-wrapper");
  setTimeout(() => {
    if (chatWrapper) chatWrapper.scrollTop = chatWrapper.scrollHeight;
  }, 0);

  setTimeout(() => {
    messages.value.push({
      message: "Sure, I can help you with that.",
      time: new Date(),
      isUser: false,
    });
  }, 1000);
};
</script>

<style scoped></style>
