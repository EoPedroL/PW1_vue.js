<template>
  <div class="product-card">
    <div class="image-container">
      <img :src="imagem" :alt="nome" />
    </div>
    <div class="product-info">
      <h3 class="product-title">{{ nome }}</h3>
      <p class="product-price">R$ {{ preco.toFixed(2).replace('.', ',') }}</p>
      <button class="btn-add" @click="adicionarAoCarrinho">
        Adicionar ao Carrinho
      </button>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  nome: {
    type: String,
    required: true
  },
  preco: {
    type: Number,
    required: true
  },
  imagem: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['adicionar-ao-carrinho'])

function adicionarAoCarrinho() {
  emit('adicionar-ao-carrinho')
}
</script>

<style scoped>
.product-card {
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.12);
}

.image-container {
  width: 100%;
  height: 180px;
  background-color: #f4f4f5;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.image-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.product-info {
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.product-title {
  font-size: 1.1rem;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 0.5rem;
}

.product-price {
  font-size: 1.25rem;
  font-weight: 700;
  color: #10b981;
  margin-bottom: 1rem;
}

.btn-add {
  margin-top: auto;
  background-color: #4f46e5;
  color: #ffffff;
  border: none;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.btn-add:hover {
  background-color: #4338ca;
}

.btn-add:active {
  transform: scale(0.98);
}
</style>