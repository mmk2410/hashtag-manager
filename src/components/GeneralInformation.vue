<template>
  <v-expansion-panel>
    <v-expansion-panel-title>
      General Information
    </v-expansion-panel-title>

    <v-expansion-panel-text>
      <v-text-field
        v-model="title"
        label="Title"
      ></v-text-field>

      <v-text-field
        v-model="location"
        label="Location"
      ></v-text-field>

      <v-text-field
        v-model="date"
        label="Date"
      ></v-text-field>
    </v-expansion-panel-text>
  </v-expansion-panel>
</template>

<script setup lang="ts">
  import { computed } from 'vue';

  const props = defineProps(['title', 'location', 'date']);
  const emit = defineEmits(['update:title', 'update:location', 'update:date']);

  const title = computed({
    get() {
      return props.title;
    },
    set(value) {
      emit('update:title', value);
    }
  });

  const location = computed({
    get() {
      return props.location;
    },
    set(value) {
      emit('update:location', value);
    }
  });

  const date = computed({
    get() {
      return props.date;
    },
    set(value) {
      emit('update:date', value);
    }
  });
</script>
