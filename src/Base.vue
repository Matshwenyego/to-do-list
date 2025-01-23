<template>
  <div id="app">
    <h1>Random API Data Fetcher</h1>

    <div class="actions">
      <button @click="fetchData">Fetch Random Data</button>
    </div>

    <div v-if="loading" class="loading">Loading...</div>

    <div v-if="error" class="error">Error: {{ error }}</div>

    <div v-if="data" class="data-display">
      <h2>Fetched Data</h2>
      <pre>{{ data }}</pre>
    </div>
  </div>
</template>

<script>
import { fetchRandomData } from './api'

export default {
  data() {
    return {
      data: null,
      loading: false,
      error: null,
    }
  },
  methods: {
    async fetchData() {
      this.loading = true
      this.error = null
      this.data = null
      try {
        this.data = await fetchRandomData()
      } catch (err) {
        this.error = err.message
      } finally {
        this.loading = false
      }
    },
  },
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}

.actions {
  margin-bottom: 20px;
}

.loading {
  font-weight: bold;
  color: blue;
}

.error {
  color: red;
  font-weight: bold;
}

.data-display {
  background: #f4f4f4;
  padding: 15px;
  border-radius: 5px;
  border: 1px solid #ddd;
}

pre {
  overflow-x: auto;
  white-space: pre-wrap;
  word-wrap: break-word;
}
</style>
