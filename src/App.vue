<script setup>
import { ref, onMounted, watch } from 'vue'
import Form from './components/Form.vue'
import Menu from './components/Menu.vue'

// 1. Iniciamos tentando pegar os dados do localStorage
// Se não existir nada, ele começa como um array vazio []
const produtos = ref(JSON.parse(localStorage.getItem('meu_cardapio_digital')) || [])
const nextId = ref(1)

const adicionarProduto = (novo) => {
  produtos.value.push({ ...novo, id: nextId.value++ })
}

const removerProduto = (id) => {
  produtos.value = produtos.value.filter(p => p.id !== id)
}

// 2. O "Watch" vigia a variável 'produtos'. 
// Sempre que você adicionar ou remover algo, ele dispara essa função.
watch(
  produtos, 
  (novaLista) => {
    // Salvamos a lista convertida em String (JSON)
    localStorage.setItem('meu_cardapio_digital', JSON.stringify(novaLista))
  }, 
  { deep: true } // O 'deep' serve para ele vigiar dentro do array (se um preço mudar, por exemplo)
)
</script>

<template>
  <div class="glow-1"></div>
  <div class="glow-2"></div>
  
  <div class="app-container">
    <main class="main-layout">
      <Menu :produtos="produtos" @deletar-item="removerProduto" />
      <Form @enviar-item="adicionarProduto" />
    </main>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap');

:root {
  --primary: #ff4d00;
  --primary-glow: rgba(255, 77, 0, 0.4);
  --bg-dark: #050508; /* Mais escuro para destacar o degrade */
  --card-bg: #12121a;
  --glass-border: rgba(255, 255, 255, 0.05);
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  background-color: var(--bg-dark);
  font-family: 'Plus Jakarta Sans', 'Inter', sans-serif;
  color: #ffffff;
  min-height: 100vh;
  overflow-x: hidden;
}

/* Efeito de degradê atmosférico no fundo */
.glow-1 {
  position: fixed;
  top: -10%;
  right: -5%;
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, var(--primary-glow) 0%, transparent 70%);
  z-index: -1;
  filter: blur(80px);
  opacity: 0.4;
}

.glow-2 {
  position: fixed;
  bottom: 0%;
  left: -5%;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(82, 35, 255, 0.15) 0%, transparent 70%);
  z-index: -1;
  filter: blur(80px);
}

.app-container {
  width: 100%;
  min-height: 100vh;
  /* Degradê sutil de fundo */
  background: linear-gradient(135deg, #050508 0%, #0d0d16 100%);
}

.main-layout {
  display: grid; /* Mudamos de flex para grid */
  grid-template-columns: 1fr 400px; /* Menu ganha o que sobrar, Form fixa em 400 */
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 40px;
  gap: 30px;
  align-items: flex-start;
  box-sizing: border-box; /* Garante que o padding não aumente o tamanho total */
}

/* Scrollbar estilizada para combinar */
::-webkit-scrollbar {
  width: 8px;
}
::-webkit-scrollbar-track {
  background: var(--bg-dark);
}
::-webkit-scrollbar-thumb {
  background: #222;
  border-radius: 10px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--primary);
}

@media (max-width: 1100px) {
  .main-layout {
    grid-template-columns: 1fr; /* Em telas menores, um embaixo do outro */
    justify-items: center;
  }
}
</style>