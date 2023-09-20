<template>
  <v-app-bar>
    <v-app-bar-title text="Hashtag Manager"></v-app-bar-title>

    <template v-slot:append>
    <v-btn icon @click="upload = true">
      <v-icon aria-label="Upload hashtag data">mdi-upload</v-icon>
    </v-btn>

    <v-btn icon @click="$emit('download-file')">
      <v-icon aria-label="Download hashtag data">mdi-download</v-icon>
    </v-btn>

    <v-btn icon @click="about = true">
      <v-icon aria-label="Show about dialog">mdi-information</v-icon>
    </v-btn>
    </template>
  </v-app-bar>

  <v-dialog width="auto" v-model="upload">
    <v-card>
      <v-card-title>
        Upload Hashtag Data File
      </v-card-title>
      <v-card-text>
        <p>
          Documentation about the file format will be published soon.
        </p>
        <p class="mt-3">
          <strong>Warning:</strong>
          As of know there is no input check. If the uploaded data is not correct, the application will not work anymore. You may manually clear the local storage to get it running again.
        </p>
        <v-file-input
          class="mt-3"
          label="Hashtag Data File"
          id="file-upload"
        ></v-file-input>
      </v-card-text>
      <v-card-actions>
        <v-btn color="primary" @click="$emit('upload-file'); upload = false">Import</v-btn>
        <br>
        <v-btn color="primary" @click="upload = false">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <v-dialog width="auto" v-model="about">
    <v-card>
      <v-card-title>
        About Hashtag Manager
      </v-card-title>
      <v-card-text>
        <p>
          Hashtag Manager is a simple helper for preparing caption text for sharing images on social networks.
        </p>
        <p class="mt-3">
          The hashtag data is only stored locally inside the browser and never transmitted anywhere else.
        </p>
        <p class="mt-3">
          Thank you very much for using!
        </p>
        <v-divider class="mt-3"></v-divider>
        <p class="mt-3">
          Created by
          <a target="blank" href="https://mmk2410.org">Marcel Kapfer</a>
          with
          <a target="blank"  href="https://vuejs.org">Vue.js</a>
          and
          <a target="blank"  href="https://vuetifyjs.com">Vuetify</a>
          using
          <a target="blank" href="https://www.gnu.org/software/emacs/">GNU Emacs</a>.
        </p>
        <p class="mt-3">
          Browse the source code at
          <a target="blank" href="https://git.mmk2410.org/mmk2410/hashtag-manager">mmk2410 Git</a>,
          <a target="blank" href="https://gitlab.com/mmk2410/hashtag-manager">GitLab.com</a>, or
          <a target="blank" href="https://github.com/mmk2410/hashtag-manager">GitHub</a>,
        </p>
        <p class="mt-3">
          Discover my photography on
          <a target="blank" href="https://pixelfed.social/mmk2410">Pixelfed</a>,
          <a target="blank" href="https://vero.co/mmk2410">VERO</a>,
          <a target="blank" href="https://www.flickr.com/photos/marcelkapfer/">Flickr</a>,
          <a target="blank" href="https://instagram.com/mmk2410">Instagram</a>,
          and <a target="blank" href="https://marcelkapfer.photography">my website.</a>
        </p>
        <v-divider class="mt-3"></v-divider>
        <p class="mt-3">
          2023 © Marcel Kapfer
          <br>
          Licensed under BSD 2-clause
          <br>
          <a href="https://mmk2410.org/imprint/">
            Imprint and Privacy Policy
          </a>
        </p>
      </v-card-text>
      <v-card-actions>
        <v-btn color="primary" block @click="about = false">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
  import { ref } from 'vue';

  defineEmits(['download-file', 'upload-file']);

  const about = ref(false);
  const upload = ref(false);
</script>
