<template>
  <h3>{{ category.name }}</h3>

  <v-chip-group>
    <v-chip
      v-for="hashtag in category.hashtags"
      :key="hashtag"
    >
      {{ hashtag }}
    </v-chip>
  </v-chip-group>

  <v-checkbox v-model="use" label="Use Hashtags"></v-checkbox>
</template>

<script setup lang="ts">
  import { ref, watch } from 'vue';

  const props = defineProps(['category']);
  const emit = defineEmits(['use', 'unuse']);

  const use = ref(false);

  watch(use, (newVal, oldVal) => {
    if (newVal) {
      emit('use', props.category.name);
    } else {
      emit('unuse', props.category.name);
    }
  });
</script>
