<script setup lang="ts">
import { provide, ref } from "vue";
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import type User from "./types/User";
import type Entry from "./types/Entry";
import { userInjectionKey } from "./InjectionKeys";
const user: User = {
  id: 1,
  username: "johndoe",
  settings: [],
};

provide(userInjectionKey, user);

const entryList = ref<Entry[]>([]);

const handleCreateEntry = (entry: Entry) => {
  // add entry to list
  entryList.value.unshift(entry);
};
</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry" />
    <ul>
      <li v-for="entry in entryList" :key="entry.id">
        <EntryCard :entry="entry" />
      </li>
    </ul>
  </main>
</template>
