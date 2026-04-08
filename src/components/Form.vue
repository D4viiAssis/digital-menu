<script setup>
import { ref } from 'vue'

// O 'emit' é o microfone do componente. 
// Ele permite que este filho avise ao App.vue: "Ei, tem item novo!"
const emit = defineEmits(['enviarItem'])

// Essas são as "caixas" onde guardamos temporariamente o que você digita.
// O 'ref' faz com que o Vue fique vigiando essas caixas.
const nome = ref('')
const preco = ref(0)
const categoria = ref('Lanche')
const disponivel = ref(true)

// Mudamos de 'handleSubmit' para 'cadastrarItem'
const cadastrarItem = () => {
  // Criamos um "pacote" (objeto) com tudo o que está nas caixas.
  // Usamos .value porque, no script, para mexer em uma ref, tem que usar .value.
  const novoItem = {
    nome: nome.value,
    preco: preco.value,
    categoria: categoria.value,
    disponivel: disponivel.value
  }
  
  // O componente "grita" o evento e manda o pacote junto.
  emit('enviarItem', novoItem)

  // Limpamos as caixas para o próximo cadastro.
  nome.value = ''
  preco.value = 0
}
</script>

<template>
  <section class="registro">
    <h2>REGISTER ITEM</h2>
    
    <div class="form-group">
      <label>ITEM NAME</label>
      <input v-model="nome" type="text" placeholder="Ex: Bam-Burguer">
    </div>

    <div class="form-group">
      <label>PRICE (R$)</label>
      <input v-model.number="preco" type="number">
    </div>

    <div class="form-group">
      <label>CATEGORY</label>
      <select v-model="categoria">
        <option value="Lanche">Lanche</option>
        <option value="Bebida">Bebida</option>
        <option value="Sobremesa">Sobremesa</option>
      </select>
    </div>

    <div class="form-group">
      <label>AVAILABLE</label>
      <input v-model="disponivel" type="checkbox">
    </div>

    <button @click="cadastrarItem" class="btn-submit">SUBMIT</button>
  </section>
</template>

<style scoped>
/* Estilos visuais para ficar parecido com seu Canva */
.registro {
  background: #1a1a2e;
  padding: 30px;
  border-radius: 20px;
  border: 1px solid #ff4d00; 
  width: 350px;
}
.form-group {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}
.btn-submit {
  background: linear-gradient(to right, #ff4d00, #ff8c00);
  border: none;
  color: white;
  padding: 10px;
  width: 100%;
  cursor: pointer;
  font-weight: bold;
}
</style>