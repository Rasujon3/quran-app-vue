<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      suras: [],
      currentSura: [],
      loading: true,
    };
  },
  mounted() {
    axios.get("https://api.alquran.cloud/v1/surah").then((res) => {
      this.suras = res.data.data;
    });

    this.querySpecificSura(1);
  },
  methods: {
    getSpecificSura(e) {
      this.querySpecificSura(e.target.value);
    },
    querySpecificSura(suraNumber) {
      this.loading = true;
      axios
        .get(`https://api.alquran.cloud/v1/surah/${suraNumber}`)
        .then((res) => {
          this.currentSura = res.data.data;
          this.loading = false;
        });
    },
  },
};
</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container py-6">
      <div class="bg-white p-4 shadow rounded">
        <div class="flex mx-4 items-center mb-6">
          <div class="flex-2 px-4">
            <select @change="getSpecificSura" class="quran-input" name="" id="">
              <option value="">Select Sura</option>
              <option v-for="sura in suras" :value="sura.number">
                {{ sura.number }} - {{ sura.englishName }} - {{ sura.name }}
              </option>
            </select>
          </div>

          <div class="flex-1 px-4 text-center">
            <h3 class="font-bold mb-1 text-lg">
              {{ currentSura.name }} - {{ currentSura.englishName }}
            </h3>
            <p>
              {{ currentSura.englishNameTranslation }} - Ayahs -
              {{ currentSura.numberOfAyahs }}
            </p>
          </div>
          <!-- <div class="flex-1 px-4">
            <select class="quran-input" name="" id="">
              <option value="">Select Sura</option>
            </select>
          </div> -->
        </div>

        <div v-if="loading" class="flex">
          <p>Loading ...</p>
        </div>

        <div v-else class="text-4xl">
          <p
            class="flex items-center mb-6"
            v-if="currentSura.hasOwnProperty('ayahs')"
            v-for="ayah in currentSura.ayahs"
          >
            <span
              class="text-xs inline-block w-5 h-5 flex items-center justify-center border rounded-full mr-4"
              >{{ ayah.numberInSurah }}
            </span>
            {{ ayah.text }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}
</style>
