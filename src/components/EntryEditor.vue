<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import { ref, computed } from "vue";

//Events emited by component
defineEmits<{
  (e: "@create", entry: { emoji: Emoji | null; text: string }): void;
}>();

const emoji = ref<Emoji | null>(null);
const charCount = computed(() => text.value.length);

//Textarea Logic
const text = ref("");
const maxChars = 280;
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    text.value = textarea.value;
  } else {
    text.value = textarea.value.substring(0, maxChars);
  }
};
</script>
<template>
  <form class="entry-form" @submit.prevent="$emit('@create', { text, emoji })">
    <textarea
      @keyup="handleTextInput"
      v-model="text"
      placeholder="New Journal Entry for danielkelly_io"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
