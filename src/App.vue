<script setup>
import { reactive } from 'vue';
import Cabecalho from '/Usuario/Desktop/testDataStone/desafioTest/src/components/Cabecalho.vue';
import CadastroClientes from '/Usuario/Desktop/testDataStone/desafioTest/src/components/CadastroCliente.vue';
import CadastroProduto from '/Usuario/Desktop/testDataStone/desafioTest/src/components/CadastroProduto.vue';
import Associa from '/Usuario/Desktop/testDataStone/desafioTest/src/components/Associa.vue';
import ListagemProduto from '/Usuario/Desktop/testDataStone/desafioTest/src/components/ListagemProdutos.vue';
import ListagemCliente from '/Usuario/Desktop/testDataStone/desafioTest/src/components/ListagemClientes.vue';

const state = reactive({
  isMenuOpen: false
});

const toggleMenu = () => {
  state.isMenuOpen = !state.isMenuOpen;
};

const calcularProdutosSelecionados = (cliente) => {
  if (cliente.produtos && cliente.produtos.length > 0) {
    const nomesProdutos = cliente.produtos.map(produto => produto.nome);
    return nomesProdutos.join(', ');
  } else {
    return 'Nenhum produto associado';
  }
};
</script>

<template>
  <div>
    <Cabecalho :toggle-menu="toggleMenu" />
    <CadastroClientes />
    <CadastroProduto />
    <Associa />
    <ListagemProduto />
    <ListagemCliente />
  </div>
</template>

<style scoped>

</style>
