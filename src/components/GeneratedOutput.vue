<template>
  <v-expansion-panel>
    <v-expansion-panel-title>
      Output
    </v-expansion-panel-title>

    <v-expansion-panel-text>
      <v-btn
        variant="tonal"
        block
        style="margin-bottom: 20px;"
        @click="$emit('generate')"
      >
        Generate
      </v-btn>

      <div
        v-for="platform in platforms.filter((platform) => platform.isPlatform)"
        :key="platform.name"
      >
        <v-textarea
          :label="platform.name"
          v-model="output[platform.name.replace(' ', '-').toLowerCase()]"
          no-resize
          disabled
        ></v-textarea>

        <v-btn
          style="margin-bottom: 20px;"
          variant="text"
          @click="copy(platform.name)"
        >
          Copy
        </v-btn>
      </div>
    </v-expansion-panel-text>
  </v-expansion-panel>
</template>

<script setup lang="ts">
  const props = defineProps(['platforms', 'output']);
  defineEmits(['generate']);

  const copy = (platform) => {
    navigator
      .clipboard
      .writeText(props.output[platform.replace(' ', '-').toLowerCase()]);
  }
</script>
