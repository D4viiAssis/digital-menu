<script setup>
import { ref } from 'vue'
import Form from './components/Form.vue'
import Menu from './components/Menu.vue'

// Estado dos produtos começa VAZIO, sem o "testa brilhante"
const produtos = ref([])

const adicionarProduto = (novo) => {
  produtos.value.push({ ...novo, id: Date.now() })
}

const removerProduto = (id) => {
  produtos.value = produtos.value.filter(p => p.id !== id)
}
</script>

<template>
  <div class="app-background">
    <main class="main-layout">
      <Menu :produtos="produtos" @deletar-item="removerProduto" />
      <Form @enviar-item="adicionarProduto" />
    </main>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');

:root {
  --orange: #ff4d00;
  --dark-bg: #0b0b13;
  --card-bg: #161625;
  --text: #ffffff;
}

body {
  margin: 0;
  padding: 0;
  background-color: var(--dark-bg);
  font-family: 'Inter', sans-serif;
  color: var(--text);
  -webkit-font-smoothing: antialiased;
}

.app-background {
  min-height: 100vh;
  position: relative;
  background: radial-gradient(circle at top right, rgba(255, 77, 0, 0.1), transparent 40%);
}

.main-layout {
  display: flex;
  max-width: 1400px;
  margin: 0 auto;
  padding: 40px;
  gap: 50px;
}

@media (max-width: 1024px) {
  .main-layout {
    flex-direction: column-reverse;
    padding: 20px;
  }
}
</style>