<script setup>
import { ref } from 'vue'

import CardItem from './components/CardItem.vue'
import Form from './components/Form.vue'
import Menu from './components/Menu.vue'

const itensNoCardapio = ref([
  { 
    id: 1, 
    nome: 'Bam-Bam-BAM', 
    preco: 235, 
    categoria: 'Lanche', 
    disponivel: true,
    imagem: '/a.jpeg' 
  }
])

const adicionarAoCardapio = (novoItem) => {

  const itemComId = { ...novoItem, id: Date.now() }
  itensNoCardapio.value.push(itemComId)
}
</script>

<template>
  <main class="container">
    <Menu :produtos="itensNoCardapio" />

    <Form @enviarItem="adicionarAoCardapio" />
  </main>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}

.container {
  display: flex;
  justify-content: space-between; 
  gap: 40px;
  padding: 40px;
  background-color: #0b0b13; 
  color: white;
  min-height: 100vh;
}
</style>