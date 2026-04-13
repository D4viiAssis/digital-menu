<script setup>
import { ref } from 'vue'
const emit = defineEmits(['enviar-item'])

const nome = ref('')
const preco = ref(0)
const categoria = ref('Lanche')
const disponivel = ref(true)
const imagemSelecionada = ref(null) // Começa nulo

const handleFile = (e) => {
  const file = e.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (event) => {
      // Converte a imagem em uma string Base64 reativa
      imagemSelecionada.value = event.target.result
    }
    reader.readAsDataURL(file)
  }
}

const cadastrar = () => {
  if (!nome.value || !imagemSelecionada.value) {
    alert('Dê um nome e selecione uma foto!')
    return
  }
  
  emit('enviar-item', {
    nome: nome.value.toUpperCase(),
    preco: preco.value,
    categoria: categoria.value,
    disponivel: disponivel.value,
    imagem: imagemSelecionada.value // Envia a string Base64
  })

  // Reset total
  nome.value = ''
  preco.value = 0
  categoria.value = 'Lanche'
  disponivel.value = true
  imagemSelecionada.value = null
}
</script>

<template>
  <aside class="form-container">
    <h2>REGISTER ITEM</h2>
    
    <div class="input-group">
      <label>ITEM NAME</label>
      <input v-model="nome" type="text" placeholder="Ex: BAM-BURGUER">
    </div>

    <div class="input-group file-upload-group">
      <label>PHOTO (FROM PC)</label>
      <input type="file" @change="handleFile" accept="image/*" id="file-upload" hidden>
      
      <label for="file-upload" class="custom-file-upload">
        <img v-if="imagemSelecionada" :src="imagemSelecionada" class="preview-img">
        <span v-else class="upload-icon">+</span>
      </label>
    </div>

    <div class="row">
      <div class="input-group flex-1">
        <label>PRICE</label>
        <input v-model.number="preco" type="number">
      </div>
      <div class="input-group flex-1">
        <label>CAT</label>
        <select v-model="categoria">
          <option value="Lanche">LANCHE</option>
          <option value="Bebida">BEBIDA</option>
          <option value="Sobremesa">SOBREMESA</option>
        </select>
      </div>
    </div>

    <div class="input-group checkbox-group">
      <label>AVAILABLE</label>
      <input v-model="disponivel" type="checkbox" class="toggle">
    </div>

    <button @click="cadastrar" class="submit-btn">SUBMIT</button>
  </aside>
</template>

<style scoped>
.form-container {
  background: var(--card-bg);
  padding: 35px;
  border-radius: 30px;
  border: 1px solid var(--orange);
  width: 380px;
  height: fit-content;
}

h2 {
  color: var(--orange);
  font-size: 1.8rem;
  font-weight: 900;
  margin-bottom: 25px;
}

.input-group {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
}

label {
  font-size: 0.7rem;
  font-weight: bold;
  color: #888;
  margin-bottom: 8px;
  text-transform: uppercase;
}

input, select {
  background: #0b0b13;
  border: 1px solid #333;
  color: white;
  padding: 12px;
  border-radius: 10px;
  font-family: inherit;
}

/* Estilo do círculo pontilhado laranja */
.custom-file-upload {
  width: 130px;
  height: 130px;
  margin: 0 auto;
  border: 2px dashed var(--orange);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  overflow: hidden;
  position: relative;
  transition: 0.3s;
}

.custom-file-upload:hover {
  background: rgba(255, 77, 0, 0.1);
}

.upload-icon {
  font-size: 3rem;
  color: var(--orange);
  font-weight: 900;
}

.preview-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
}

.row {
  display: flex;
  gap: 15px;
}

.flex-1 {
  flex: 1;
}

.checkbox-group {
  flex-direction: row;
  align-items: center;
  gap: 10px;
}

.submit-btn {
  background: linear-gradient(90deg, var(--orange), #ff8c00);
  border: none;
  color: white;
  padding: 18px;
  width: 100%;
  border-radius: 12px;
  font-weight: 900;
  cursor: pointer;
  margin-top: 10px;
  transition: 0.3s;
  box-shadow: 0 5px 15px rgba(255, 77, 0, 0.2);
}

.submit-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(255, 77, 0, 0.4);
}
</style>