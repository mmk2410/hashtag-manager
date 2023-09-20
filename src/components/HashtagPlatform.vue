<template>
  <v-expansion-panel>
    <v-expansion-panel-title>
      {{ platform.name }}
    </v-expansion-panel-title>

    <v-expansion-panel-text>
      <v-switch v-if="!platform.isPlatform" v-model="primarySwitch" label="Primary"></v-switch>

      <hashtag-category
        v-for="category in platform.categories"
        :key="category"
        :category="category"
        @use="(categoryName) => $emit('use', platform.name, categoryName)"
        @unuse="(categoryName) => $emit('unuse', platform.name, categoryName)"
      ></hashtag-category>
    </v-expansion-panel-text>
  </v-expansion-panel>
</template>

<script setup lang="ts">
  import HashtagCategory from "./HashtagCategory.vue";

  import { ref, watch } from 'vue';

  const props = defineProps(['platform', 'primary']);
  const emit = defineEmits(['update:primary', 'use', 'unuse']);

  const primarySwitch = ref(false);

  watch(primarySwitch, (newVal, oldVal) => {
    if (newVal) {
      emit('update:primary', props.platform.name);
    }
  });

  watch(
    () => props.primary,
    (primary) => {
      if (props.platform.name !== primary) {
        primarySwitch.value = false;
      }
    }
  );
</script>
