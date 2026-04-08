<script setup>
defineProps({
  produto: Object
})

defineEmits(['remover'])
</script>

<template>
  <div class="card" :class="{ 'indisponivel': !produto.disponivel }">
    <button class="btn-remove" @click="$emit('remover', produto.id)">&times;</button>

    <div class="image-section">
      <div class="white-circle">
        <img 
          src="@/assets/images/a.jpeg" 
          alt="Lanche" 
          class="food-img"
        >
      </div>
    </div>

    <div class="content">
      <span class="category">{{ produto.categoria }}</span>
      <h3 class="name">{{ produto.nome }}</h3>
      <p class="price">R$ {{ produto.preco.toFixed(2) }}</p>
    </div>

    <div v-if="!produto.disponivel" class="off-badge">INDISPONÍVEL</div>
  </div>
</template>

<style scoped>
.card {
  background: #1a1a2e; /* O azul escuro do fundo */
  border-radius: 30px;
  padding: 25px 15px;
  text-align: center;
  position: relative;
  transition: 0.3s;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.card:hover {
  transform: translateY(-8px);
  border-color: #ff4d00;
}

/* Requisito 2: Opacidade para itens fora de estoque */
.indisponivel {
  opacity: 0.6;
}

/* Lógica da Moldura Branca */
.image-section {
  display: flex;
  justify-content: center;
  margin-bottom: 15px;
}

.white-circle {
  width: 110px;
  height: 110px;
  background: #ffffff; /* Fundo branco puro como no Canva */
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
  overflow: hidden; /* Corta a imagem se ela for maior que o círculo */
  border: 3px solid #fff;
}

.food-img {
  width: 100%; /* Tamanho da imagem dentro do círculo */
  height: auto;
  object-fit: contain;
  filter: drop-shadow(0 5px 10px rgba(0,0,0,0.2));
}

/* Tipografia */
.category {
  color: #ff4d00;
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.name {
  color: #fff;
  margin: 8px 0;
  font-size: 1.1rem;
}

.price {
  color: #fff;
  font-weight: bold;
  font-size: 1.3rem;
  margin: 0;
}

/* Botão de Remover */
.btn-remove {
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: #ff4444;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.2rem;
  line-height: 1;
}

.off-badge {
  background: #ff4444;
  color: white;
  font-size: 0.6rem;
  padding: 2px 8px;
  border-radius: 5px;
  display: inline-block;
  margin-top: 10px;
  font-weight: bold;
}
</style>