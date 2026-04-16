<script setup>
import { ref, onMounted, watch } from 'vue'
import Form from './components/Form.vue'
import Menu from './components/Menu.vue'

const products = ref(JSON.parse(localStorage.getItem('my_digital_menu')) || [])
const nextId = ref(Date.now())

const addProduct = (newItem) => {
  products.value.push({ ...newItem, id: nextId.value++ })
}

const removeProduct = (id) => {
  products.value = products.value.filter(p => p.id !== id)
}

watch(
  products, 
  (newList) => {
    localStorage.setItem('my_digital_menu', JSON.stringify(newList))
  }, 
  { deep: true }
)
</script>

<template>
  <div class="glow-1"></div>
  <div class="glow-2"></div>
  <div class="app-container">
    <main class="main-layout">
      <Menu :products="products" @delete-item="removeProduct" />
      <Form @send-item="addProduct" />
    </main>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap');

:root {
  --primary: #ff4d00;
  --primary-glow: rgba(255, 77, 0, 0.4);
  --bg-dark: #050508;
  --card-bg: #12121a;
  --glass-border: rgba(255, 255, 255, 0.05);
  --orange: #ff4d00;
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
  background: radial-gradient(circle at 0% 0%, #090812, #0a071a 40%, #000000 80%);
}

.main-layout {
  display: grid;
  grid-template-columns: 1fr 400px;
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 40px;
  gap: 30px;
  align-items: flex-start;
  box-sizing: border-box;
}

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
    grid-template-columns: 1fr;
    justify-items: center;
  }
}
</style>