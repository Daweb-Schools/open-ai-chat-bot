<template>
  <div class="flex flex-col flex-grow w-full max-w-xl">
    <div
      class="flex flex-col flex-grow bg-white shadow-xl rounded-lg overflow-hidden"
    >
      <div class="flex flex-col flex-grow h-0 p-4 overflow-auto chat-wrapper">
        <ChatBlock
          v-for="(message, index) in messages"
          :key="index"
          :message="message.message"
          :isUser="message.isUser"
        />

        <div v-if="loading" class="message">
          <div class="typing typing-1"></div>
          <div class="typing typing-2"></div>
          <div class="typing typing-3"></div>
        </div>
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
    <img
      src="@/assets/images/daweb-logo.png"
      class="h-10 mt-5 w-full object-contain"
      alt="daweb logo"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import ChatBlock, { type Props } from "./components/ChatBlock.vue";

const messages = ref<Props[]>([
  {
    message: "Hi there! How can I help you?",
    isUser: false,
  },
  {
    message: "I would like to know more about your services.",
    isUser: true,
  },
]);
const message = ref("");
const loading = ref(false);

const sendMessage = () => {
  loading.value = true;
  messages.value.push({
    message: message.value,
    isUser: true,
  });
  message.value = "";
  const chatWrapper = document.querySelector(".chat-wrapper");
  setTimeout(() => {
    if (chatWrapper) chatWrapper.scrollTop = chatWrapper.scrollHeight;
  }, 0);

  setTimeout(() => {
    loading.value = false;
    messages.value.push({
      message: "Sure, I can help you with that.",
      isUser: false,
    });
    setTimeout(() => {
      if (chatWrapper) chatWrapper.scrollTop = chatWrapper.scrollHeight;
    }, 0);
  }, 1000);
};
</script>

<style scoped></style>
