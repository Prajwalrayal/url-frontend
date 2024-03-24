<script setup>
import { ref } from 'vue'
const url = ref('https://example.com')

const shortenEndpoint = 'https://url-backend-2lee.onrender.com/shorten'

const shortenedUrl = ref('')

const shortenUrl = () => {
  //post request to shorten url
  console.log(url.value)
  fetch(shortenEndpoint, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ OriginalURL: url.value })
  })
    .then((response) => response.json())
    .then((data) => {
      shortenedUrl.value = data.short_code
      console.log(data)
    })
    .catch((error) => {
      console.error('Error:', error)
    })
}
</script>

<template>
  <div id="app">
    <header>
      <h1>Url Shortner</h1>
    </header>
    <form @submit.prevent="shortenUrl">
      <div class="form-group">
        <input v-model="url" type="url" id="url" name="url" required />
        <button @click="shortenUrlHandler" type="submit">Shorten</button>
      </div>
    </form>
    <div v-if="shortenedUrl" class="shortenedUrl">
      <p>
        Shortened URL: <a :href="shortenedUrl" target="_blank">{{ shortenedUrl }}</a>
      </p>
    </div>
  </div>
</template>

<style scoped>
body {
  margin: 0;
  font-family: 'Open Sans', sans-serif;
}

#app {
  background: linear-gradient(to right, #f0f8ff, #e6e9f2);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.title {
  font-size: 2em;
  margin-bottom: 20px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
}

header {
  text-align: center;
}

.form-group {
  display: flex;
  width: 100%; /* Make the form group take full width */
}

input {
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  flex: 1; /* Allow input to grow and shrink with available space */
  font-size: 16px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #28a745; /* Adjust based on your color scheme */
  color: white;
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
  margin-left: 10px; /* Add margin for spacing between input and button */
}

button:hover {
  background-color: #218c3d; /* Adjust based on your color scheme */
  transform: scale(1.02); /* Slight scale up on hover */
  transition: transform 0.2s ease-in-out;
}

.shortenedUrl {
  margin-top: 20px;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 400px; /* Adjust width as needed */
  text-align: center;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.2s ease-in-out;
  background-color: #f5f5f5; /* Light gray background for result box */
}

.shortenedUrl:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transform: scale(1.01); /* Slight scale up on hover */
  transition: transform 0.2s ease-in-out;
}

@media only screen and (max-width: 768px) {
  #app {
    padding: 20px;
  }

  .form-group,
  .shortenedUrl {
    width: 100%; /* Make form and result box take full width on smaller screens */
  }
}
</style>
