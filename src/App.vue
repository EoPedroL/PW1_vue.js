<template>
  <div class="app-container">
    <header class="header">
      <div class="header-content">
        <h1>TechStore <span>Vue</span></h1>
        <div class="cart-badge">
          🛒 Itens no carrinho: <strong>{{ contadorCarrinho }}</strong>
        </div>
      </div>
    </header>

    <main class="main-content">
      <section class="search-section">
        <input 
          v-model="termoBusca" 
          type="text" 
          placeholder="Pesquisar produto pelo nome..." 
          class="search-input"
        />
      </section>

      <section class="products-grid" v-if="produtosFiltrados.length > 0">
        <ProductCard
          v-for="produto in produtosFiltrados"
          :key="produto.id"
          :nome="produto.nome"
          :preco="produto.preco"
          :imagem="produto.imagem"
          @adicionar-ao-carrinho="incrementarCarrinho"
        />
      </section>

      <div v-else class="empty-state">
        <p>Nenhum produto encontrado para "{{ termoBusca }}".</p>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import ProductCard from './components/ProductCard.vue'

const contadorCarrinho = ref(0)
const termoBusca = ref('')

const produtos = ref([
  { id: 1, nome: 'Teclado Mecânico RGB', preco: 289.90, imagem: 'https://images.unsplash.com/photo-1587829741301-dc798b83add3?auto=format&fit=crop&w=400&q=80' },
  { id: 2, nome: 'Mouse Gamer 16000 DPI', preco: 159.90, imagem: 'https://images.unsplash.com/photo-1527864550417-7fd91fc51a46?auto=format&fit=crop&w=400&q=80' },
  { id: 3, nome: 'Monitor UltraWide 29"', preco: 1250.00, imagem: 'https://images.unsplash.com/photo-1527443224154-c4a3942d3acf?auto=format&fit=crop&w=400&q=80' },
  { id: 4, nome: 'Headset Surround 7.1', preco: 349.00, imagem: 'https://images.unsplash.com/photo-1546435770-a3e426bf472b?auto=format&fit=crop&w=400&q=80' },
  { id: 5, nome: 'Webcam Full HD 1080p', preco: 199.90, imagem: 'https://images.unsplash.com/photo-1587826080692-f439cd0b70da?auto=format&fit=crop&w=400&q=80' },
  { id: 6, nome: 'Mousepad Extra Grande', preco: 79.90, imagem: 'https://images.unsplash.com/photo-1616440347437-b1c73416efc2?auto=format&fit=crop&w=400&q=80' }
])

function incrementarCarrinho() {
  contadorCarrinho.value++
}

const produtosFiltrados = computed(() => {
  return produtos.value.filter(p => 
    p.nome.toLowerCase().includes(termoBusca.value.toLowerCase().trim())
  )
})
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #f8fafc;
  color: #334155;
}

.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  background-color: #1e293b;
  color: #ffffff;
  padding: 1.5rem 2rem;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.header-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.header h1 span {
  color: #4f46e5;
}

.cart-badge {
  background-color: #334155;
  padding: 0.6rem 1.2rem;
  border-radius: 999px;
  font-size: 0.95rem;
  border: 1px solid #475569;
}

.main-content {
  max-width: 1200px;
  width: 100%;
  margin: 2rem auto;
  padding: 0 1.5rem;
}

.search-section {
  margin-bottom: 2rem;
}

.search-input {
  width: 100%;
  padding: 0.85rem 1.25rem;
  font-size: 1rem;
  border: 2px solid #cbd5e1;
  border-radius: 10px;
  outline: none;
  transition: border-color 0.2s ease;
}

.search-input:focus {
  border-color: #4f46e5;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1.5rem;
}

.empty-state {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 1.1rem;
}
</style>