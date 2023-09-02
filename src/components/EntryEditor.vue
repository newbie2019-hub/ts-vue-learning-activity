<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import { ref, computed, onMounted, inject } from "vue";
import { userInjectionKey } from "@/injectionKeys";
import type Entry from "@/types/Entry";

// import type { Ref } from "vue";
// const emoji: Ref<Emoji | null> = ref(null);

const text = ref("");
const textarea = ref<HTMLTextAreaElement | null>(null);
const emoji = ref<Emoji | null>(null);

const injectedUser = inject(userInjectionKey);

onMounted(() => {
  textarea.value?.focus();
});

//if you want to set explicit type you can do it by generic argument but is not
//necessary for short or simple computed because it relies on the infered type
const charCount = computed(() => text.value.length);
const maxChars = 280;

//macro and doesnt run in the browser
// defineEmits(["create"]);
// can be written as
const emit = defineEmits<{
  (e: "create", entry: Entry): void;
  // (e: "new-event", entry: { text: string }): void;
}>();

const handleTextInput = (e: Event) => {
  //Using as for type assertion to be more explicit
  const textarea = e.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    text.value = textarea.value;
  } else {
    text.value = textarea.value = textarea.value.substring(0, 280);
  }
};

const handleSubmit = () => {
  emit("create", {
    body: text.value,
    emoji: emoji.value,
    createdAt: new Date(),
    userId: 1,
    id: Math.random(),
  });

  text.value = "";
  emoji.value = null;
};
</script>
<template>
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea
      :value="text"
      @keyup="handleTextInput"
      ref="textarea"
      :placeholder="`New Journal Entry for ${injectedUser?.username}`"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
