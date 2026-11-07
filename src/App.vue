<template>
  <div class="app-container">
    <!-- Cabeçalho com botão interativo do Carrinho -->
    <header class="header">
      <div class="header-content">
        <h1>TechStore <span>Vue</span></h1>
        
        <div class="header-actions">
          <button class="cart-badge-btn" @click="abrirModal">
            🛒 Itens no carrinho: <strong>{{ totalItensCarrinho }}</strong>
          </button>
        </div>
      </div>
    </header>

    <main class="main-content">
      <!-- Campo de Busca -->
      <section class="search-section">
        <input 
          v-model="termoBusca" 
          type="text" 
          placeholder="Pesquisar produto pelo nome..." 
          class="search-input"
        />
      </section>

      <!-- Lista de Produtos -->
      <section class="products-grid" v-if="produtosFiltrados.length > 0">
        <ProductCard
          v-for="produto in produtosFiltrados"
          :key="produto.id"
          :nome="produto.nome"
          :preco="produto.preco"
          :imagem="produto.imagem"
          @adicionar-ao-carrinho="adicionarAoCarrinho(produto)"
        />
      </section>

      <div v-else class="empty-state">
        <p>Nenhum produto encontrado para "{{ termoBusca }}".</p>
      </div>
    </main>

    <!-- Modal do Carrinho de Compras -->
    <div v-if="exibirModal" class="modal-overlay" @click.self="fecharModal">
      <div class="modal-card">
        <div class="modal-header">
          <h2>Seu Carrinho</h2>
          <button class="btn-close" @click="fecharModal">&times;</button>
        </div>

        <div class="modal-body">
          <div v-if="itensCarrinho.length === 0" class="cart-empty-msg">
            <p>O seu carrinho está vazio no momento.</p>
          </div>

          <ul v-else class="cart-items-list">
            <li v-for="item in itensCarrinho" :key="item.id" class="cart-item">
              <img :src="item.imagem" :alt="item.nome" class="cart-item-img" />
              <div class="cart-item-details">
                <h4>{{ item.nome }}</h4>
                <p>Qtd: {{ item.quantidade }} x R$ {{ item.preco.toFixed(2).replace('.', ',') }}</p>
              </div>
              <div class="cart-item-total">
                R$ {{ (item.preco * item.quantidade).toFixed(2).replace('.', ',') }}
              </div>
            </li>
          </ul>
        </div>

        <div class="modal-footer" v-if="itensCarrinho.length > 0">
          <div class="cart-total-price">
            Total: <strong>R$ {{ valorTotalCarrinho.toFixed(2).replace('.', ',') }}</strong>
          </div>
          <div class="modal-actions">
            <button class="btn-clear" @click="limparCarrinho">Limpar Carrinho</button>
            <button class="btn-checkout" @click="fecharModal">Fechar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import ProductCard from './components/ProductCard.vue'

const termoBusca = ref('')
const exibirModal = ref(false)

// Lista de produtos do catálogo
const produtos = ref([
  { id: 1, nome: 'Teclado Mecânico RGB', preco: 289.90, imagem: 'https://images.unsplash.com/photo-1619683322755-4545503f1afa?q=80&w=774&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' },
  { id: 2, nome: 'Mouse Gamer 16000 DPI', preco: 159.90, imagem: 'https://images.unsplash.com/photo-1629429408209-1f912961dbd8?q=80&w=870&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' },
  { id: 3, nome: 'Monitor UltraWide 29', preco: 1250.00, imagem: 'https://images.unsplash.com/photo-1666771410140-0573b232426e?q=80&w=774&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' },
  { id: 4, nome: 'Headset Surround 7.1', preco: 349.00, imagem: 'https://images.unsplash.com/photo-1710265029735-434f63c672c4?q=80&w=870&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' },
  { id: 5, nome: 'Webcam Full HD 1080p', preco: 199.90, imagem: 'https://images.unsplash.com/photo-1623949556303-b0d17d198863?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' },
  { id: 6, nome: 'Mousepad Extra Grande', preco: 79.90, imagem: 'https://media.istockphoto.com/id/1330776370/pt/foto/big-black-fabric-gaming-mouse-pad-in-roll-on-white-background.webp?a=1&b=1&s=612x612&w=0&k=20&c=rfVm2gXltmgGn6j6BdGbaomjFBloYuNi3GlCLbHLDq0=' },
  { id: 7, nome: 'Cadeira Gamer Ergonômica', preco: 899.00, imagem: 'https://plus.unsplash.com/premium_photo-1731966821478-fef27450b053?q=80&w=870&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' },
  { id: 8, nome: 'Microfone Condensador USB', preco: 249.90, imagem: 'https://images.unsplash.com/photo-1660631228116-b3643559f611?q=80&w=387&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D' }
])

// Array do carrinho que guarda os produtos selecionados
const itensCarrinho = ref([])

// Adiciona um produto ao carrinho (ou incrementa a quantidade se já existir)
function adicionarAoCarrinho(produto) {
  const itemExistente = itensCarrinho.value.find(item => item.id === produto.id)
  if (itemExistente) {
    itemExistente.quantidade++
  } else {
    itensCarrinho.value.push({ ...produto, quantidade: 1 })
  }
}

// Esvazia o array do carrinho
function limparCarrinho() {
  itensCarrinho.value = []
}

// Controle da modal
function abrirModal() {
  exibirModal.value = true
}

function fecharModal() {
  exibirModal.value = false
}

// Propriedades Computadas (Computed Properties)
const totalItensCarrinho = computed(() => {
  return itensCarrinho.value.reduce((total, item) => total + item.quantidade, 0)
})

const valorTotalCarrinho = computed(() => {
  return itensCarrinho.value.reduce((total, item) => total + (item.preco * item.quantidade), 0)
})

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

.cart-badge-btn {
  background-color: #334155;
  color: #ffffff;
  padding: 0.6rem 1.2rem;
  border-radius: 999px;
  font-size: 0.95rem;
  border: 1px solid #475569;
  cursor: pointer;
  transition: background-color 0.2s ease, transform 0.1s ease;
}

.cart-badge-btn:hover {
  background-color: #475569;
  transform: scale(1.03);
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

/* Modal CSS */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
}

.modal-card {
  background-color: #ffffff;
  border-radius: 12px;
  max-width: 500px;
  width: 100%;
  max-height: 85vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  overflow: hidden;
}

.modal-header {
  padding: 1.25rem;
  background-color: #1e293b;
  color: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-close {
  background: none;
  border: none;
  color: #ffffff;
  font-size: 1.5rem;
  cursor: pointer;
}

.modal-body {
  padding: 1.25rem;
  overflow-y: auto;
  flex-grow: 1;
}

.cart-empty-msg {
  text-align: center;
  padding: 2rem;
  color: #64748b;
}

.cart-items-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.cart-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding-bottom: 0.75rem;
  border-bottom: 1px solid #e2e8f0;
}

.cart-item-img {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 6px;
}

.cart-item-details {
  flex-grow: 1;
}

.cart-item-details h4 {
  font-size: 0.95rem;
  color: #1e293b;
}

.cart-item-details p {
  font-size: 0.85rem;
  color: #64748b;
}

.cart-item-total {
  font-weight: 600;
  color: #10b981;
}

.modal-footer {
  padding: 1.25rem;
  background-color: #f8fafc;
  border-top: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.cart-total-price {
  font-size: 1.1rem;
  text-align: right;
}

.modal-actions {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}

.btn-clear {
  background-color: #ef4444;
  color: #ffffff;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
}

.btn-clear:hover {
  background-color: #dc2626;
}

.btn-checkout {
  background-color: #4f46e5;
  color: #ffffff;
  border: none;