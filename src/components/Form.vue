<script setup>
import { ref } from 'vue'
const emit = defineEmits(['enviar-item'])

const nome = ref('')
const preco = ref(0)
const categoria = ref('')
const disponivel = ref(true)
const imagemSelecionada = ref(null)

const handleFile = (e) => {
  const file = e.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (event) => {
      imagemSelecionada.value = event.target.result
    }
    reader.readAsDataURL(file)
  }
}

const cadastrar = () => {
  if (!nome.value || !imagemSelecionada.value) {
    alert('Preencha o nome e selecione uma foto!')
    return
  }
  
  emit('enviar-item', {
    nome: nome.value.toUpperCase(),
    preco: preco.value,
    categoria: categoria.value,
    disponivel: disponivel.value,
    imagem: imagemSelecionada.value
  })

  nome.value = ''
  preco.value = 0
  imagemSelecionada.value = null
}
</script>

<template>
  <aside class="form-container">
    <h2 class="form-title">REGISTER ITEM</h2>
    
    <div class="input-group">
      <label>ITEM NAME</label>
      <input v-model="nome" type="text" placeholder="Ex: BAM-BURGUER" class="dark-input">
    </div>

    <div class="input-group">
      <label>PHOTO (FROM PC)</label>
      <input type="file" @change="handleFile" accept="image/*" id="file-upload" hidden>
      
      <label for="file-upload" class="upload-area">
        <div v-if="imagemSelecionada" class="preview-container">
          <img :src="imagemSelecionada" class="preview-img">
        </div>
          <span class="plus-icon" v-else>+</span>
      </label>
    </div>

    <div class="row">
      <div class="input-group flex-1">
        <label>PRICE (R$)</label>
        <input v-model.number="preco" type="number" class="dark-input">
      </div>
      <div class="input-group flex-1">
        <label>CATEGORY</label>
        <select v-model="categoria" class="dark-input">
          <option value="" disabled>SELECT</option>
          <option value="Lunch">LUNCH</option>
          <option value="beverage">BEVERAGE</option>
          <option value="dessert">DESSERT</option>
          <option value="combo">COMBO</option>
        </select>
      </div>
    </div>

    <div class="checkbox-container">
      <label>AVAILABLE</label>
      <input v-model="disponivel" type="checkbox" class="styled-checkbox">
    </div>

    <button @click="cadastrar" class="btn-submit">SUBMIT</button>
  </aside>
</template>

<style scoped>
.form-container {
  background: rgba(22, 22, 37, 0.8);
  backdrop-filter: blur(15px);
  padding: 30px;
  border-radius: 40px;
  border: 1px solid rgba(255, 77, 0, 0.3);
  width: 450px;
  box-shadow: 0 20px 20px rgba(0, 0, 0, 0.4);
  position: sticky;
  top: 40px;
}

.form-title {
  color: var(--orange);
  font-size: 2.2rem;
  font-weight: 900;
  margin-bottom: 30px;
  letter-spacing: -1px;
}

.input-group {
  margin-bottom: 25px;
  display: flex;
  flex-direction: column;
}

label {
  font-size: 0.65rem;
  font-weight: 800;
  color: #666;
  margin-bottom: 10px;
  letter-spacing: 1px;
}

.dark-input {
  background: #08080c;
  border: 1.5px solid #1a1a24;
  color: white;
  padding: 15px;
  border-radius: 12px;
  font-size: 0.9rem;
  transition: all 0.3s ease;
}

.dark-input:focus {
  border-color: var(--orange);
  background: #0c0c14;
  outline: none;
}

.upload-area {
  position: relative;
  width: 120px;
  height: 120px;
  margin: 0 auto;
  border: 2px dashed rgba(255, 77, 0, 0.5);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: 0.3s;
  background: rgba(0, 0, 0, 0.2);
}

.upload-area:hover {
  background: rgba(255, 77, 0, 0.1);
  border-color: var(--orange);
}

.plus-icon {
  position: absolute;
  top: 25px;
  font-size: 4em;
  color: var(--orange);
  font-weight: 300;
}

.preview-container {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid var(--orange);
}

.preview-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.row {
  display: flex;
  gap: 20px;
}

.flex-1 { flex: 1; }

.checkbox-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  margin-bottom: 30px;
}

.styled-checkbox {
  width: 20px;
  height: 20px;
  accent-color: var(--orange);
  cursor: pointer;
}

.btn-submit {
  background: linear-gradient(90deg, #ff4d00 0%, #ff8c00 100%);
  border: none;
  color: white;
  padding: 20px;
  width: 100%;
  border-radius: 15px;
  font-weight: 900;
  font-size: 1rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(  0.175, 0.885, 0.32, 1.275);
}

.btn-submit:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 30px rgba(255, 77, 0, 0.4);
}
</style>