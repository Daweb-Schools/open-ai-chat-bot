<template>
  <div
    class="flex w-full mt-2 max-w-xs"
    :class="{
      'ml-auto justify-end': isUser,
    }"
  >
    <div
      class="flex-shrink-0 h-10 w-10 rounded-full bg-gray-300"
      :class="{
        'order-last ml-3': isUser,
        'mr-3': !isUser,
      }"
    ></div>
    <div>
      <div :class="classes">
        <p class="text-sm">
          {{ message }}
        </p>
      </div>
      <span class="text-xs text-gray-500 leading-none">{{ timeSent }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import { formatDistanceToNow } from "date-fns";
export interface Props {
  message: string;
  isUser?: boolean;
  time: Date;
}

const props = defineProps<Props>();

const isUser = ref(props.isUser);
const time = ref(props.time);

const classes = computed(() => {
  return {
    "bg-blue-600 text-white p-3 rounded-l-lg rounded-br-lg": isUser.value,
    "bg-gray-300 p-3 rounded-r-lg rounded-bl-lg": !isUser.value,
  };
});

const timeSent = computed(() => {
  return formatDistanceToNow(time.value, { addSuffix: true });
});
</script>

<style scoped></style>
