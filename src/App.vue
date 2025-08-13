<template>
  <div class="container">
    <h1>MediClassAI</h1>
    <p>Classification de textes médicaux</p>
    
    <textarea v-model="inputText" placeholder="Collez un compte-rendu médical ici..."></textarea>
    <button @click="classifyText">Analyser</button>
    
    <div v-if="result" class="result">
      <h2>Résultats:</h2>
      <p><strong>Spécialité(s) détectée(s):</strong> {{ result.detected_fields.join(', ') }}</p>
      <div v-if="result.entities.length">
        <h3>Entités reconnues:</h3>
        <ul>
          <li v-for="(entity, index) in result.entities" :key="index">
            {{ entity[0] }} ({{ entity[1] }})
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputText: '',
      result: null
    }
  },
  methods: {
    async classifyText() {
      try {
        const response = await fetch('https://medi-class-ai-back.onrender.com/classify', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ text: this.inputText })
        });
        this.result = await response.json();
      } catch (error) {
        console.error('Erreur:', error);
      }
    }
  }
}
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}
textarea {
  width: 100%;
  height: 200px;
  margin: 10px 0;
}
button {
  padding: 10px 20px;
  background: #42b983;
  color: #004563;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.result {
  margin-top: 20px;
  padding: 15px;
  background: #f5f5f5;
  border-radius: 4px;
}
</style>