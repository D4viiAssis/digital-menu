<script setup>
import { ref, computed } from 'vue'
import CardItem from './CardItem.vue'

const props = defineProps({
  produtos: Array
})

const categoriaAtiva = ref('TODAS')

const produtosFiltrados = computed(() => {
  if (categoriaAtiva.value === 'TODAS') {
    return props.produtos
  }
  // Filtra ignorando maiúsculas/minúsculas para evitar erros
  return props.produtos.filter(p => p.categoria.toUpperCase() === categoriaAtiva.value)
})

// 4. COMPUTED (Resumo): Faz as contas do Requisito 4 automaticamente
const totalItens = computed(() => produtosFiltrados.value.length)

const totalDisponiveis = computed(() => {
  return produtosFiltrados.value.filter(p => p.disponivel).length
})

const precoMedio = computed(() => {
  if (totalItens.value === 0) return 0
  const soma = produtosFiltrados.value.reduce((acc, p) => acc + p.preco, 0)
  return soma / totalItens.value
})
</script>

<template>
  <section class="menu-container">
    <header class="menu-header">
      <h2>MENU</h2>
      
      <div class="categories">
        <button 
          v-for="cat in ['TODAS', 'LANCHE', 'BEBIDA', 'SOBREMESA']" 
          :key="cat"
          :class="{ active: categoriaAtiva === cat }"
          @click="categoriaAtiva = cat"
        >
          {{ cat === 'TODAS' ? cat : cat + 'S' }}
        </button>
      </div>
    </header>

    <div class="grid-cardapio">
      <CardItem 
        v-for="item in produtosFiltrados" 
        :key="item.id" 
        :produto="item"
      />
    </div>

    <footer class="resumo">
      <div class="card-resumo">
        <p>Total Exibido: <strong>{{ totalItens }}</strong></p>
        <p>Disponíveis: <strong>{{ totalDisponiveis }}</strong></p>
        <p>Preço Médio: <strong>R$ {{ precoMedio.toFixed(2) }}</strong></p>
      </div>
    </footer>
  </section>
</template>

<style scoped>
.menu-container {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.menu-header h2 {
  font-size: 2rem;
  color: #ff4d00;
  margin-bottom: 20px;
}

.categories {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.categories button {
  background: transparent;
  border: 1px solid #ff4d00;
  color: white;
  padding: 8px 20px;
  border-radius: 20px;
  cursor: pointer;
  transition: 0.3s;
  font-weight: bold;
  font-size: 0.8rem;
}

/* Requisito 3: Destaque visual do botão ativo */
.categories button.active {
  background: #ff4d00;
  box-shadow: 0 0 10px rgba(255, 77, 0, 0.5);
}

.grid-cardapio {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.resumo {
  margin-top: auto;
  padding-top: 30px;
}

.card-resumo {
  background: rgba(255, 255, 255, 0.05);
  padding: 15px;
  border-radius: 10px;
  display: flex;
  justify-content: space-around;
  border-left: 4px solid #ff4d00;
}

.card-resumo p {
  margin: 0;
  font-size: 0.9rem;
  color: #ccc;
}

.card-resumo strong {
  color: #fff;
  display: block;
  font-size: 1.1rem;
}
</style>