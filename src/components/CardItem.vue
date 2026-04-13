<script setup>
defineProps({
  produto: Object
})

defineEmits(['remover'])
</script>

<template>
  <div class="card" :class="{ off: !produto.disponivel }">
    <button class="remove-btn" @click="$emit('remover', produto.id)">&times;</button>
    
    <div class="img-section">
      <div class="photo-circle">
        <img :src="produto.imagem" class="user-photo" alt="Lanche">
      </div>
    </div>

    <div class="info">
      <span class="category-tag">{{ produto.categoria }}</span>
      <h3 class="food-name">{{ produto.nome }}</h3>
      <p class="food-price">R$ {{ produto.preco.toFixed(2) }}</p>
    </div>
  </div>
</template>

<style scoped>
.card {
  background: var(--card-bg);
  border-radius: 30px;
  padding: 30px 15px;
  text-align: center;
  position: relative;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.card:hover {
  transform: translateY(-12px);
  border-color: var(--orange);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
}

.off {
  opacity: 0.3;
  filter: grayscale(1);
}

.img-section {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.photo-circle {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  /* AQUI ESTÁ A MÁGICA: Borda real + Brilho (Glow) */
  border: 4px solid var(--orange);
  box-shadow: 0 0 15px rgba(255, 77, 0, 0.3);
  
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
}

.user-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.category-tag {
  color: var(--orange);
  font-size: 0.6rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.food-name {
  margin: 8px 0;
  font-size: 1.1rem;
  color: #fff;
  font-weight: 700;
}

.food-price {
  font-size: 1.25rem;
  font-weight: 900;
  color: #ccc;
  margin: 0;
}

.remove-btn {
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(255, 255, 255, 0.05);
  border: none;
  color: #ff4444;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.2s;
}

.remove-btn:hover {
  background: #ff4444;
  color: white;
}
</style>