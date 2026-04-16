<script setup>
defineProps({
  product: Object
})

defineEmits(['remove'])
</script>

<template>
  <div class="card" :class="{ off: !product.available }">
    <button class="remove-btn" @click="$emit('remove', product.id)" title="Remove item">
      <span class="icon">&times;</span>
    </button>
    <div class="img-section">
      <div class="photo-circle">
        <img :src="product.image" class="user-photo" alt="Product">
      </div>
    </div>
    <div class="info">
      <span class="category-tag">{{ product.category }}</span>
      <h3 class="food-name">{{ product.name }}</h3>
      <div class="price-container">
        <p class="food-price">$ {{ product.price.toFixed(2) }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  background: rgba(22, 22, 37, 0.4);
  backdrop-filter: blur(8px);
  border-radius: 25px;
  padding: 20px 15px;
  text-align: center;
  position: relative;
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  border: 1px solid rgba(255, 255, 255, 0.03);
  width: 100%;
  max-width: 220px; 
  margin: 0 auto;
}

.card:hover {
  transform: translateY(-10px) scale(1.02);
  border-color: rgba(255, 77, 0, 0.5);
  background: rgba(22, 22, 37, 0.7);
  box-shadow: 0 20px 15px rgba(0, 0, 0, 0.6), 0 0 20px rgba(255, 77, 0, 0.1);
}

.off {
  opacity: 0.4;
  filter: grayscale(0.8);
  cursor: not-allowed;
  text-decoration: line-through;
}

.img-section {
  display: flex;
  justify-content: center;
  margin-bottom: 15px;
}

.photo-circle {
  width: 110px;
  height: 110px;
  border-radius: 50%;
  border: 3px solid var(--orange);
  box-shadow: 0 0 20px rgba(255, 77, 0, 0.4);
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #000;
  transition: 0.3s;
}

.card:hover .photo-circle {
  box-shadow: 0 0 30px rgba(255, 77, 0, 0.6);
  transform: rotate(3deg);
}

.user-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.category-tag {
  color: var(--orange);
  font-size: 0.55rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  display: block;
  margin-bottom: 5px;
}

.food-name {
  margin: 5px 0;
  font-size: 1rem;
  color: #fff;
  font-weight: 800;
  letter-spacing: -0.5px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.price-container {
  margin-top: 10px;
}

.food-price {
  font-size: 1.1rem;
  font-weight: 900;
  color: #ddd;
  margin: 0;
}

.remove-btn {
  position: absolute;
  top: -10px;
  right: -10px;
  background: linear-gradient(135deg, #302c4f 0%, #4b4675 100%);
  border: 2px solid #302c4f;
  color: white;
  width: 32px;
  height: 32px;
  border-radius: 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
  transition: all 0.3s ease;
  z-index: 5;
}

.remove-btn .icon {
  font-size: 1.4rem;
  line-height: 1;
  margin-top: -2px;
}

.remove-btn:hover {
  transform: rotate(90deg) scale(1.1);
  background: rgb(10, 22, 34);
  box-shadow: 0 0 20px rgba(47, 77, 100, 0.5);
}
</style>