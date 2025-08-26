<template>
  <div class="container">
    <h1>MediClassAI</h1>
    <textarea v-model="inputText" placeholder="Collez un compte-rendu médical..."></textarea>
    <button @click="classifyText">Analyser</button>
    
    <div v-if="loading" class="loading">Analyse en cours...</div>
    
    <div v-if="result" class="result">
      <h2>Résultats :</h2>
      <p>Spécialité(s) : {{ result.detected_fields.join(', ') }}</p>
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
.loading {
  color: #42b983;
  font-weight: bold;
}

.result {
  margin-top: 20px;
  padding: 15px;
  border-left: 4px solid #42b983;
  background: #f8f9fa;
}
</style>