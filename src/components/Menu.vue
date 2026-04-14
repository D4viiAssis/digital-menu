<script setup>
import { ref, computed } from 'vue'
import CardItem from './CardItem.vue'

const props = defineProps({ produtos: Array })
const emit = defineEmits(['deletar-item'])
const categoriaAtiva = ref('ALL')
const categorias = ['ALL', 'LUNCH', 'BEVERAGE', 'DESSERT', 'COMBO']

const filtrados = computed(() => {
  if (categoriaAtiva.value === 'ALL') return props.produtos
  return props.produtos.filter(item => item.categoria.toUpperCase() === categoriaAtiva.value)
})

const stats = computed(() => {
  const total = filtrados.value.length
  const disp = filtrados.value.filter(p => p.disponivel).length
  const media = total ? filtrados.value.reduce((acc, p) => acc + p.preco, 0) / total : 0
  return { total, disp, media }
})
</script>

<template>
  <section class="menu-side">
    <header class="menu-header">
      <h1 class="glow-title">MENU</h1>
      
      <div class="filter-bar">
        <button v-for="category in categorias" 
          :key="category" :class="{ active: categoriaAtiva === category }" @click="categoriaAtiva = category">
          {{ category }}
        </button>
      </div>

      <h2 class="category-indicator">
        {{ categoriaAtiva === 'ALL' ? 'ALL' : categoriaAtiva }}
      </h2>
    </header>

    <div class="grid">
      <CardItem 
        v-for="item in filtrados" 
        :key="item.id" 
        :produto="item" 
        @remover="(id) => $emit('deletar-item', id)" 
      />
    </div>

    <div class="stats-bar">
      <div class="stat">
        <span>TOTAL</span>
        <strong>{{ stats.total }}</strong>
      </div>
      <div class="stat">
        <span>AVAILABLE</span>
        <strong>{{ stats.disp }}</strong>
      </div>
      <div class="stat">
        <span>AVERAGE PRICE</span>
        <strong>R$ {{ stats.media.toFixed(2) }}</strong>
      </div>
    </div>
  </section>
</template>

<style scoped>
.menu-side { 
  flex: 1; 
  display: flex;
  flex-direction: column;
}

.glow-title { 
  font-size: 5rem; 
  font-weight: 900; 
  margin: 0;
  /* Degradê no texto igual ao Canva */
  background: linear-gradient(to bottom, #ff4d00, #ff8c00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  line-height: 1;
}

.category-indicator {
  font-size: 2rem;
  font-weight: 800;
  margin: 20px 0;
  color: #fff;
  letter-spacing: -1px;
}

.filter-bar { 
  display: flex; 
  gap: 12px; 
  margin-top: 15px; 
}

.filter-bar button {
  background: transparent; 
  border: 1.5px solid var(--orange); 
  color: white;
  padding: 8px 25px; 
  border-radius: 50px; 
  cursor: pointer; 
  font-weight: 700; 
  font-size: 0.75rem;
  transition: all 0.3s ease;
  letter-spacing: 0.5px;
}

.filter-bar button:hover { 
  background: rgba(255, 77, 0, 0.1); 
}

.filter-bar button.active { 
  background: var(--orange); 
  box-shadow: 0 0 20px rgba(255, 77, 0, 0.4); 
}

/* Grid com cards menores */
.grid { 
  display: grid; 
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); 
  gap: 20px; 
  margin-bottom: 40px;
}

.stats-bar {
  margin-top: auto; 
  background: rgba(22, 22, 37, 0.6); 
  backdrop-filter: blur(10px);
  padding: 25px; 
  border-radius: 15px;
  display: flex; 
  justify-content: space-around; 
  /* Borda lateral estilosa do design */
  border-left: 5px solid var(--orange);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.stat {
  text-align: center;
}

.stat span { 
  display: block; 
  font-size: 0.65rem; 
  color: #777; 
  font-weight: 800;
  margin-bottom: 8px;
  letter-spacing: 1px;
}

.stat strong { 
  font-size: 1.5rem; 
  color: #fff;
  font-weight: 800;
}
</style>