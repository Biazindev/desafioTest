import { reactive } from "vue";
<script>
export default {
  data() {
    return {
      isMenuOpen: false,
      telaAtual: 'cadastroClientes',
      novoCliente: {
        nome: '',
        documento: '',
        telefone: '',
        email: '',
        ativo: '',
        associado: ' ',
      },
      novoProduto: {
        nome: '',
        preco: '',
        ativo: 'Sim'
      },
      clienteSelecionado: '',
      produtosSelecionados: [],
      clientes: [],
      produtos: [''],
      checkbox1Enabled: false,
      checkbox2Enabled: true
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
      }, 
    calcularProdutosSelecionados(cliente) {
      if (cliente.produtos && cliente.produtos.length > 0) {
        const nomesProdutos = cliente.produtos.map(produto => produto.nome);
        return nomesProdutos.join(', ');
      } else {
        return 'Nenhum produto associado';
      }
    },
    cadastrarCliente() {
      const cliente = { ...this.novoCliente, id: this.clientes.length + 1, produtos: this.produtos };
      this.clientes.push(cliente);
      this.limparFormularioCliente();
    },
    cadastrarProduto() {
      this.produtos.push({ ...this.novoProduto, id: this.produtos.length + 1 });
      this.limparFormularioProduto();
    },
    associarProdutosCliente() {
  console.log('Lista de clientes:', this.clientes);
  // Verifica se foi selecionado algum cliente
  if (!this.clienteSelecionado) {
    console.error('Cliente não selecionado.');
    return;
  }

  // Encontra o cliente selecionado na lista de clientes
  const clienteSelecionado = this.clientes.find(c => c.id === this.clienteSelecionado);
  if (!clienteSelecionado) {
    console.error('Cliente não encontrado.');
    return;
  }

  // Filtra os produtos selecionados e os associa ao cliente selecionado
  clienteSelecionado.produtos = this.produtos.filter(produto => this.produtosSelecionados.includes(produto.id));
  this.produtosSelecionados = [];

  console.log('Produtos associados ao cliente:', clienteSelecionado.produtos);
},

    editarCliente(cliente) {
      if (!cliente || typeof cliente !== 'object' || !cliente.id) {
        console.error('Cliente não encontrado ou inválido.');
        return;
      }
      cliente.editandoTelefone = true;
      cliente.editandoEmail = true;
      cliente.novoTelefone = cliente.telefone;
      cliente.novoEmail = cliente.email;
      cliente.confirmacaoEdicao = true;
      cliente.confirmarEdicao = () => {
        setTimeout(() => {
          console.log('Salvando as alterações do cliente:', cliente);
          cliente.editandoTelefone = false;
          cliente.editandoEmail = false;
          cliente.confirmacaoEdicao = false;
        }, 2000);
      };
      cliente.cancelarEdicao = () => {
        cliente.telefone = cliente.novoTelefone;
        cliente.email = cliente.novoEmail;
        cliente.editandoTelefone = false;
        cliente.editandoEmail = false;
        cliente.confirmacaoEdicao = false;
      };
    },
    excluirCliente(clienteId) {
      const index = this.clientes.findIndex(cliente => cliente.id === clienteId);
      if (index !== -1) {
        this.clientes.splice(index, 1);
        console.log('Cliente excluído com sucesso.');
      } else {
        console.error('Cliente não encontrado para exclusão.');
      }
    },
    toggleAtivoCliente(cliente) {
      cliente.ativo = !cliente.ativo;
    },
    confirmarEdicaoCliente(cliente) {
      cliente.telefone = cliente.novoTelefone;
      cliente.email = cliente.novoEmail;
      cliente.editandoTelefone = false;
      cliente.editandoEmail = false;
      cliente.confirmacaoEdicao = false;
    },
    cancelarEdicaoCliente(cliente) {
      cliente.novoTelefone = cliente.telefone;
      cliente.novoEmail = cliente.email;
      cliente.editandoTelefone = false;
      cliente.editandoEmail = false;
      cliente.confirmacaoEdicao = false;
    },
    ativarInativarProduto(produto) {
      console.log('Ativar/Inativar produto:', produto);
    },
    ativarInativarCliente(cliente) {
      console.log('Ativar/Inativar cliente:', cliente);
    },
    excluirProduto(produto) {
      const index = this.produtos.findIndex(p => p.id === produto.id);
      if (index !== -1) {
        this.produtos.splice(index, 1);
        console.log('Produto excluído com sucesso.');
      } else {
        console.error('Produto não encontrado para exclusão.');
      }
    },

    limparFormularioCliente() {
      this.novoCliente.nome = '';
      this.novoCliente.documento = '';
      this.novoCliente.telefone = '';
      this.novoCliente.email = '';
      this.novoCliente.ativo = '';
      this.novoCliente.associado = '';
    },
    limparFormularioProduto() {
      this.novoProduto.nome = '';
      this.novoProduto.preco = '';
      this.novoProduto.ativo = 'Sim';
    }
  },
  mounted() {
    const documentoClienteInput = document.getElementById('documentoCliente');
    if (documentoClienteInput) {
      documentoClienteInput.addEventListener('input', (event) => {
        const input = event.target;
        const value = input.value.replace(/\D/g, '');
        input.value = formatarCPF(value);
      });
    }
    const telefoneClienteInput = document.getElementById('telefoneCliente');
    if (telefoneClienteInput) {
      telefoneClienteInput.addEventListener('input', (event) => {
        const input = event.target;
        const value = input.value.replace(/\D/g, '');
        input.value = formatarTelefone(value);
      });
    }
    new bootstrap.Collapse(document.querySelector('.navbar-collapse'));
  }
};
function formatarCPF(documento) {
  documento = documento.replace(/\D/g, '');
  documento = documento.replace(/(\d{3})(\d)/, '$1.$2');
  documento = documento.replace(/(\d{3})(\d)/, '$1.$2');
  documento = documento.replace(/(\d{3})(\d{1,2})$/, '$1-$2');
  return documento;
}

function formatarTelefone(telefone) {
  telefone = telefone.replace(/\D/g, '');
  if (telefone.length === 11) { // Celular: (##) #####-####
    telefone = telefone.replace(/(\d{2})(\d{5})(\d{4})/, '($1) $2-$3');
  } else if (telefone.length === 10) { // Fixo: (##) ####-####
    telefone = telefone.replace(/(\d{2})(\d{4})(\d{4})/, '($1) $2-$3');
  }
  return telefone;
}

</script>
<template>
    <div class="bg-primary col-12" id="app">
    <nav class="navbar navbar-expand-lg col-12">
      <div class="container">
        <button class="navbar-toggler col-12" @click="toggleMenu" type="button" aria-controls="navbarNav"
      aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
        <div class="navbar-collapse" :class="{ show: isMenuOpen }" id="navbarNav">
            <ul class="navbar-nav">
            <li class="nav-item rounded" @click="telaAtual = 'cadastroClientes'">
              <a class="nav-link text-white btn-sm btn btn-primary mx-2" style="font-weight: 400;">Cadastro de Clientes</a>
            </li>
            <li class="nav-item rounded" @click="telaAtual = 'cadastroProdutos'">
              <a class="nav-link text-white btn-sm btn btn-primary mx-2" style="font-weight: 400;">Cadastro de Produtos</a>
            </li>
            <li class="nav-item rounded" @click="telaAtual = 'associacaoProdutosClientes'">
              <a class="nav-link text-white btn-sm btn btn-primary mx-2" style="font-weight: 400;">Associar Produtos a Clientes</a>
            </li>
            <li class="nav-item" @click="telaAtual = 'listagemProdutos'">
              <a class="nav-link text-white btn-sm btn btn-primary mx-2" style="font-weight: 400;">Listagem de Produtos</a>
            </li>
            <li class="nav-item rounded" @click="telaAtual = 'listagemClientes'">
              <a class="nav-link text-white btn-sm btn btn-primary mx-2" style="font-weight: 400;">Listagem de Clientes</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <div class="container mt-3">
            <div v-if="telaAtual === 'cadastroClientes'">
        <h2>Cadastro de Clientes</h2>
        <form @submit.prevent="cadastrarCliente">
          <div class="mb-3">
            <label for="nomeCliente" class="form-label">Nome:</label>
            <input type="text" class="form-control" id="nomeCliente" v-model="novoCliente.nome" required>
          </div>
          <div class="mb-3">
            <label for="documentoCliente" class="form-label">Documento:</label>
            <input type="text" class="form-control" id="documentoCliente" v-model="novoCliente.documento" required>
          </div>
          <div class="mb-3">
            <label for="telefoneCliente" class="form-label">Telefone:</label>
            <input type="tel" class="form-control" id="telefoneCliente" v-model="novoCliente.telefone" required>
          </div>
          <div class="mb-3">
            <label for="emailCliente" class="form-label">E-mail:</label>
            <input type="email" class="form-control" id="emailCliente" v-model="novoCliente.email" required>
          </div>
          <div class="mb-3">
            <div class="form-group">
              <label for="ativoCliente" class="form-label">Ativo:</label>
              <select class="form-select" id="ativoCliente" v-model="novoCliente.ativo" required>
                <option class="text-dark" value="Sim">Sim</option>
                <option class="text-dark" value="Não">Não</option>
              </select>
            </div>
          </div>
          <button type="submit" class="col-12 btn btn-success text-white mb-5">Cadastrar Cliente</button>
        </form>
      </div>
      <div v-else-if="telaAtual === 'cadastroProdutos'" class="container mt-3">
        <h2>Cadastro de Produtos</h2>
        <form @submit.prevent="cadastrarProduto">
          <div class="mb-3">
            <label for="nomeProduto" class="form-label">Nome:</label>
            <input type="text" class="form-control" id="nomeProduto" v-model="novoProduto.nome" required>
          </div>
          <div class="mb-3">
            <label for="precoProduto" class="form-label">Preço:</label>
            <input type="number" class="form-control" id="precoProduto" v-model="novoProduto.preco" required>
          </div>
          <div class="mb-3">
            <div class="form-group">
              <label for="ativoProduto" class="form-label">Ativo:</label>
              <select class="form-select" id="ativoProduto" v-model="novoProduto.ativo" required>
                <option class="text-dark" value="Sim">Sim</option>
                <option class="text-dark" value="Não">Não</option>
              </select>
            </div>
          </div>
          <button type="submit" class="col-12 btn btn-success text-white mb-5">Cadastrar Produto</button>
        </form>
      </div>
      <div v-else-if="telaAtual === 'associacaoProdutosClientes'" class="container mt-3">
        <h2>Associar Produtos a Clientes</h2>
        <form @submit.prevent="associarProdutosCliente">
          <div class="mb-3">
            <label for="clienteSelecionado" class="form-label">Cliente:</label>
            <select class="form-select" id="clienteSelecionado" v-model="clienteSelecionado" required>
              <option class="text-dark" v-for="cliente in clientes" :key="cliente.id" :value="cliente.id">{{ cliente.nome }}</option>
            </select>
          </div>
          <div class="mb-3">
            <label for="produtosSelecionados" class="form-label">Produtos:</label>
            <select class="form-select" id="produtosSelecionados" v-model="produtosSelecionados" multiple required>
              <option class="text-dark" v-for="produto in produtos" :key="produto.id" :value="produto.id">{{ produto.nome }}</option>
            </select>
          </div>
          <button type="submit" class="col-12 btn btn-success text-white mb-5">Associar Produtos</button>
        </form>
      </div>
      <div v-else-if="telaAtual === 'listagemProdutos'" class="container mt-3">
        <h2>Listagem de Produtos</h2>
<ul>
  <li v-for="produto in produtos" :key="produto.id">
    {{ produto.nome }} R${{ produto.preco }} |
    <span v-if="produto.ativo">
      <input class="form-check-input" type="checkbox" v-model="produto.checked" :checked="true" :disabled="false">
      <label class="form-check-label">Ativo</label>
    </span>
    <span v-else>
      <input class="form-check-input" type="checkbox" v-model="produto.checked" :checked="false" :disabled="true">
      <label id="#inativo" class="form-check-label">Inativo</label>
    </span>
    <br/>
    <button @click="excluirProduto(produto)" class="col-3 btn mx-3 btn-danger mb-3">Excluir</button> <!-- Passando o produto como argumento -->
  </li>
</ul>
</div>
      <div v-else-if="telaAtual === 'listagemClientes'" class="container mt-3">
  <h2>Listagem de Clientes</h2>
  <div class="table-responsive">
    <table class="table table-striped">
      <thead>
        <tr class="col-12">
          <th>Nome</th>
          <th>Documento</th>
          <th>Telefone</th>
          <th>E-mail</th>
          <th>Produtos Associados</th> <!-- Nova coluna para exibir os produtos -->
          <th>Ativo</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="cliente in clientes" :key="cliente.id" :class="{ 'cliente-inativo': !cliente.ativo }">
          <td class="text-white">{{ cliente.nome }}</td>
          <td class="text-white">{{ cliente.documento }}</td>
          <td>
            <span v-if="cliente.editandoTelefone">
              <input class="text-dark" type="text" v-model="cliente.novoTelefone">
            </span>
            <span v-else>{{ cliente.telefone }}</span>
          </td>
          <td>
            <span v-if="cliente.editandoEmail">
              <input class="text-dark" type="email" v-model="cliente.novoEmail">
            </span>
            <span v-else>{{ cliente.email }}</span>
          </td>
          <td> <!-- Exibir os produtos associados -->
            <span v-if="cliente.produtos && cliente.produtos.length > 0">
              <ul>
                <li v-for="produto in cliente.produtos" :key="produto.id">
                  {{ produto.nome }}
                </li>
              </ul>
            </span>
            <span v-else>Nenhum produto associado</span>
          </td>
          <td>
            <span class="text-dark" v-if="cliente.ativo">
              <button @click="toggleAtivoCliente(cliente)" class="btn btn-sm btn-success">Ativo</button>
            </span>
            <span :class="{'text-dark': !cliente.ativo, 'btn-inativo': !cliente.ativo}" v-else>
              <button @click="toggleAtivoCliente(cliente)" class="btn btn-sm btn-success">Inativo</button>
            </span>
          </td>
          <td>
            <button @click="editarCliente(cliente)" class="btn btn-sm btn-primary">Editar</button>
            <button v-if="cliente.confirmacaoEdicao" @click="confirmarEdicaoCliente(cliente)" class="btn btn-sm btn-success">Confirmar</button>
            <button v-if="cliente.confirmacaoEdicao" @click="cancelarEdicaoCliente(cliente)" class="btn btn-sm btn-danger">Cancelar</button>
            <button @click="excluirCliente(cliente.id)" class="btn btn-sm btn-danger">Excluir</button>
          </td>
        </tr>
      </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
</template>
<style scoped>
* {
  color: #fff;
}
.cliente-inativo {
    text-decoration: line-through; /* Aplica uma linha no cliente quando estiver inativo */
  }
html {
    background-color: #0d6efd;
}

li {
    display: grid;
}

.form-check-input:checked {
    background-color: #0cf187;
    border-color: #fff;
    margin: 4px;
}

a.nav-link.text-white.btn-sm.btn.btn-primary.mx-2 {
    font-size: medium;
    align-content: space-around;
}
a.nav-link.text-white.btn-sm.btn.btn-primary.mx-2 {
    background-color: rgba(0, 0, 0, 0.125);
}
button.btn.btn-success.text-white {
    margin-bottom: 300px;
}

  @media screen and (max-width: 780px) {
    .container {
    width: 350px;
}
.table-responsive table td {
        display: table-cell; 
        text-align: center;
    }
a.nav-link.text-white.btn-sm.btn.btn-primary.mx-2 {
    padding: 4px;
    margin: 2px;
}
  .navbar-collapse {
    display: none;
  }
  .navbar-collapse.show {
    display: block;
  }
    .navbar-toggler {
    display: block;
  }
    .table-responsive table thead {
      display: none;
    }
    .table-responsive table thead tr {
      display: block;
    }
    .table-responsive table th {
      display: block;
      text-align: center;
    }
    .table-responsive table tbody tr {
      display: block;
      border-bottom: 1px solid #ddd; 
      margin-bottom: 10px;
    }

    .table-responsive table td {
      display: block;
      text-align: center;
    }
    .table-responsive table td button {
      display: block;
      margin: 5px auto; 
    }
  }
  input.form-check-input {
    padding: 8px;
}

label.form-check-label {
    margin-left: 30px;
}
.container.mt-3 {
    display: contents;
}
button.btn.btn-sm.btn-danger {
    margin-left: inherit;
}
</style>