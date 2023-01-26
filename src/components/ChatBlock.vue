<template>
  <div
    class="flex w-full mt-2 max-w-xs"
    :class="{
      'ml-auto justify-end': isUser,
    }"
  >
    <img class="flex-shrink-0 h-10 w-10 rounded-full bg-gray-100"
      :class="{
        'order-last ml-3': isUser,
        'mr-3': !isUser,
      }" :src="getPfp" alt="">
    <div>
      <div :class="classes">
        <p class="text-sm">
          {{ message }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import { formatDistanceToNow } from "date-fns";
import dawebPfp from "@/assets/images/daweb-pfp.png";
import userPfp from "@/assets/images/user-pfp.png";



export interface Props {
  message: string;
  isUser?: boolean;
}

const props = defineProps<Props>();

const isUser = ref(props.isUser);

const classes = computed(() => {
  return {
    "bg-blue-600 text-white p-3 rounded-l-lg rounded-br-lg": isUser.value,
    "bg-gray-300 p-3 rounded-r-lg rounded-bl-lg": !isUser.value,
  };
});

const getPfp = computed(() => {
  return isUser.value
    ? userPfp
    : dawebPfp;
}); 
</script>

<style scoped></style>
