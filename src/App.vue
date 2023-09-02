<script setup lang="ts">
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import { userInjectionKey } from "./injectionKeys";
import { reactive, provide } from "vue";

//import type
import type User from "./types/User";
import type Entry from "./types/Entry";

const entries: Entry[] = reactive([]);

const user: User = reactive({
  id: 1,
  username: "Yvan Sabay",
  settings: [],
});

provide(userInjectionKey, user);

const handleCreateEntry = (entry: Entry) => {
  entries.push(entry);
};
</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @create="handleCreateEntry" />
    <ul>
      <li>
        <EntryCard v-for="(entry, i) in entries" :key="i" :entry="entry" />
      </li>
    </ul>
  </main>
</template>
