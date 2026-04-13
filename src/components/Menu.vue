<script setup>
import { ref, computed } from 'vue'
import CardItem from './CardItem.vue'

const props = defineProps({ produtos: Array })
const emit = defineEmits(['deletar-item'])
const categoriaAtiva = ref('TODAS')

const filtrados = computed(() => {
  if (categoriaAtiva.value === 'TODAS') return props.produtos
  return props.produtos.filter(p => p.categoria.toUpperCase() === categoriaAtiva.value)
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
    <h1 class="glow-title">MENU</h1>
    
    <div class="filter-bar">
      <button v-for="c in ['TODAS', 'LANCHE', 'BEBIDA', 'SOBREMESA']" 
        :key="c" :class="{ active: categoriaAtiva === c }" @click="categoriaAtiva = c">
        {{ c }}
      </button>
    </div>

    <div class="grid">
      <CardItem v-for="p in filtrados" :key="p.id" :produto="p" @remover="(id) => $emit('deletar-item', id)" />
    </div>

    <div class="stats-bar">
      <div class="stat"><span>TOTAL</span><strong>{{ stats.total }}</strong></div>
      <div class="stat"><span>DISPONÍVEIS</span><strong>{{ stats.disp }}</strong></div>
      <div class="stat"><span>MÉDIA</span><strong>R$ {{ stats.media.toFixed(2) }}</strong></div>
    </div>
  </section>
</template>

<style scoped>
.menu-side { flex: 1; }
.glow-title { font-size: 4rem; font-weight: 900; color: var(--orange); margin: 0 0 30px; text-shadow: 0 0 20px rgba(255, 77, 0, 0.3); }

.filter-bar { display: flex; gap: 10px; margin-bottom: 40px; }
.filter-bar button {
  background: transparent; border: 1px solid var(--orange); color: white;
  padding: 10px 25px; border-radius: 50px; cursor: pointer; font-weight: bold; transition: 0.3s;
}
.filter-bar button:hover { background: rgba(255, 77, 0, 0.2); }
.filter-bar button.active { background: var(--orange); box-shadow: 0 0 15px var(--orange); }

.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 25px; }

.stats-bar {
  margin-top: 50px; background: var(--card-bg); padding: 20px; border-radius: 20px;
  display: flex; justify-content: space-around; border-bottom: 4px solid var(--orange);
}
.stat span { display: block; font-size: 0.7rem; color: #888; margin-bottom: 5px; }
.stat strong { font-size: 1.3rem; }
</style>