    <script setup>
        import { defineProps } from 'vue'; 
        const { toggleMenu, isMenuOpen } = defineProps(['toggleMenu', 'isMenuOpen']);
    </script>
        
        <template>
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
                <button type="submit" class="btn btn-success text-white">Cadastrar Cliente</button>
                </form>
            </div>
            </div>
        </template>