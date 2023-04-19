<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading data...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || !results.length)">No data found.</p>
      <ul v-else-if="!isLoading && results && results.length">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: true,
      error: null
    }
  },
  methods: {
    async loadExperiences() {
      await fetch('https://form-firebase-e5c9a-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json')
        .then(res => res.json())
        .then(data => {
          const results = []
          for(const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating
            })
            
          }
          this.results = results
          this.isLoading = false
        })
        .catch(error => {
          console.log(error)
          this.isLoading = false
          this.error = 'fetch data error. please try again'
        })
    }
  },
  mounted() {
    this.loadExperiences()
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>