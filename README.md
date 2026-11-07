# Catálogo de Produtos com Carrinho Reativo (Vue.js 3)

Projeto prático desenvolvido para a disciplina de **Programação Web I** do curso de Engenharia de Software / Ciência da Computação - UNIFAN.

## Objetivo
Demonstrar a aplicação de conceitos fundamentais do Vue.js 3, como **componentização**, **reatividade**, **props**, **emissão de eventos**, **data binding (`v-model`)** e **propriedades computadas (`computed`)**, através da criação de um catálogo interativo de produtos com contador de carrinho em tempo real.

## Tecnologias Utilizadas
- **Vue.js 3** (Composition API)
- **Vite** (Build tool)
- **HTML5 & CSS3** (Flexbox, CSS Grid & Design Responsivo)
- **JavaScript (ES6+)**

## Funcionalidades
- **Exibição de Cards:** Componente reutilizável `ProductCard.vue` renderizado via `v-for`.
- **Contador Reativo:** Ao clicar em "Adicionar ao Carrinho", o contador do cabeçalho é atualizado instantaneamente sem reload da página.
- **Busca em Tempo Real:** Filtro de produtos via `v-model` e `computed` property.
- **Interface Responsiva:** Layout adaptável para smartphones, tablets e desktops.

## Como Executar o Projeto

- **Clone o repositório:**
   ```bash 
    git clone https://github.com/EoPedroL/PW1_vue.js.git