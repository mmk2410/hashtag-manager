<template>
  <v-app>
    <v-layout>
      <app-bar
        @upload-file="upload"
        @download-file="download"
      ></app-bar>

      <v-main>
        <v-container>
            <loading-screen v-if="!loaded"></loading-screen>

            <no-data-screen v-if="noData"></no-data-screen>

            <v-expansion-panels mandatory v-model="openpanel">
              <general-information
                v-if="loaded && !noData"
                v-model:title="title"
                v-model:location="location"
                v-model:date="date"
              ></general-information>

              <hashtag-platform
                v-if="loaded && !noData"
                v-for="platform in hashtagData"
                v-model:primary="primary"
                :key="platform"
                :platform="platform"
                @use="useHashtags"
                @unuse="unuseHashtags"
              ></hashtag-platform>

              <generated-output
                v-if="loaded && !noData"
                :platforms="hashtagData"
                :output="output"
                @generate="generateOutput"
              ></generated-output>
            </v-expansion-panels>
        </v-container>
      </v-main>
    </v-layout>
  </v-app>
</template>

<script setup lang="ts">
  import AppBar from "./components/AppBar.vue";
  import LoadingScreen from "./components/LoadingScreen.vue";
  import NoDataScreen from "./components/NoDataScreen.vue";
  import GeneralInformation from "./components/GeneralInformation.vue"
  import GeneratedOutput from "./components/GeneratedOutput.vue";
  import HashtagPlatform from "./components/HashtagPlatform.vue";

  import { ref, watch, onBeforeMount } from 'vue';

  const hashtagData = ref([]);
  const loaded = ref(false);
  const noData = ref(false);
  const openpanel = ref(0);

  const title = ref('');
  const location = ref('');
  const date = ref('');

  const primary = ref('');
  const used = ref([]);

  const output = ref({});

  const useHashtags = (platform: string, category: string) => {
    used.value.push(`${ platform }/${ category }`);
  };

  const unuseHashtags = (platform: string, category: string) => {
    used.value = used.value.filter((item) => {
      return item !== `${ platform }/${ category }`
    });
  };

  const generateOutput = () => {
    let text = `${ title.value } (${ location.value } | ${ date.value })\n\n`;

    let hashtagsToUse = [...used.value];
    let firstHashtags = true;

    if (primary.value !== '') {
      hashtagData.value.forEach((platform) => {
        if (platform.name === primary.value) {
          platform.categories.forEach((category) => {
            const key = `${ platform.name }/${ category.name }`;
            if (hashtagsToUse.includes(key)) {
              hashtagsToUse = hashtagsToUse.filter((item) => item !== key);

              if (!firstHashtags) {
                text = text.concat(' ')
              } else {
                firstHashtags = false;
              }

              text = text.concat(category.hashtags.join(' '));
            }
          });
        }
      });
    }

    hashtagData
      .value
      .filter((platform) => platform.isPlatform)
      .forEach(
        (platform) => {
          let platformText = text.valueOf();
          let platformFirstHashtags = true;
          platform.categories.forEach(
            (category) => {
              const key = `${ platform.name }/${ category.name }`;
              if (hashtagsToUse.includes(key)) {
                hashtagsToUse = hashtagsToUse.filter((item) => item !== key);

                if (!firstHashtags || !platformFirstHashtags) {
                  platformText = platformText.concat(' ');
                } else {
                  platformFirstHashtags = false;
                }

                platformText = platformText.concat(category.hashtags.join(' '));
              }
            }
          )
          const key = platform.name.replace(' ', '-').toLowerCase();
          output.value[key] = platformText;
        }
      );
  };

  onBeforeMount(() => {
    const data = localStorage.getItem("hashtag-data");
    loaded.value = true;

    if (data === null) {
      noData.value = true;
    } else {
      hashtagData.value = JSON.parse(data);
    }
  });

  const upload = () => {
    const fileUploadElement: HTMLInputElement = document.getElementById("file-upload") as HTMLInputElement;

    fileUploadElement
      .files[0]
      .text()
      .then((contents) => {
        const data = JSON.parse(contents);
        localStorage.setItem("hashtag-data", JSON.stringify(data));
        hashtagData.value = data;
        noData.value = false;
      });
  }

  const download = () => {
    const blob = new Blob([JSON.stringify(hashtagData.value)], {
      type: "application/json"
    });
    const url = URL.createObjectURL(blob);
    window.open(url, "_blank");
    URL.revokeObjectURL(url);
  }
</script>
