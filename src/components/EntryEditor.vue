<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import { ref, computed, onMounted } from "vue";
import type Entry from "@/types/Entry";

//Events emited by component
const emit = defineEmits<{
  (e: "@create", entry: Entry): void;
}>();

//Template ref
const textarea = ref<HTMLTextAreaElement | null>(null);
onMounted(() => textarea.value?.focus());

const emoji = ref<Emoji | null>(null);
const charCount = computed(() => body.value.length);

//Textarea Logic
const body = ref("");
const maxChars = 280;
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    body.value = textarea.value;
  } else {
    body.value = textarea.value.substring(0, maxChars);
  }
};

const submitTheEntry = () => {
  emit("@create", {
    body: body.value,
    emoji: emoji.value,
    createdAt: new Date(),
    id: new Date().getTime(),
    userId: new Date().getTime(),
  });
  emoji.value = null;
  body.value = "";
};
</script>
<template>
  <form class="entry-form" @submit.prevent="submitTheEntry">
    <textarea
      ref="textarea"
      @keyup="handleTextInput"
      v-model="body"
      placeholder="New Journal Entry for danielkelly_io"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
