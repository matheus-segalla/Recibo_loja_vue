<template>
  <div>
    <!-- Cabeçalho com Nome, Data e Endereço -->
    <header class="cabecalho">
      <table>
        <div>
          <img align="left" src="./assets/BENEVISLOGO.png">
          <h2>Marcos Vicente da Silva MATERIAIS PARA CONSTRUÇÃO ME</h2>
          RUA JOÃO ALVES DE GODOI, 44 - JD ABERTINA PINHALZINHO, SP
          CNPJ: 00.384.560/0001-80
        </div>
      </table>
      <div>
        <label for="clienteNome">Nome:</label>
        <input v-model="clienteNome" type="text" name="nome">
        <label for="clienteCpf">CPF/CNPJ:</label>
        <input v-model="clienteCpf" type="text" name="nome">
        <label for="dataPedido">Data do Pedido:</label>
        <input v-model="dataPedido" type="date" />
      </div>

      <div>Selected: {{ selected }}</div>

<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
      <div>Endereço:</div>
      <div>
        <label for="endereco.rua">Rua:</label>
        <input v-model="endereco.rua" type="text" />

        <label for="endereco.bairro">Bairro:</label>
        <input v-model="endereco.bairro" type="text" />

      </div>
      <div>
        <label for="enderecoBairro">cidade:</label>
        <input v-model="endereco.cidade" type="text" />

        <label for="dataPedido">Número:</label>
        <input v-model="endereco.numero" type="text" />

        <label for="clienteFone">FONE:</label>
        <input v-model="fone" type="text" />

      </div>
      <div>
        <label for="enderecoBairro">Forma de pagamento:</label>
        <input type="text" />

        <label for="ValorProdutos">Valor Produtos:</label>
        <input type="text" />


      </div>
      <label for="Desc">Desconto/acrés.</label>
      <input type="text" />
      <div>
        <label for="ValorProdutosTotal">Valor TOTAL:</label>
        <input type="text" />
      </div>


    </header>
    <!-- Preço Total -->
    <div class="tabbar mt-8">
      <span class="tabbar-item">Preço Total do Pedido: R$ {{ calcularPrecoTotalPedido() }}</span>
    </div>

    <!-- Formulário de Pedido -->

    <div class="flex">

      <label class="text-sm font-semibold" for="quantidade">Quantidade:</label>
      <input v-model="quantidade" type="number" class="input" required />

      <label class="text-sm font-semibold" for="produtoNome">Nome do Produto:</label>
      <input v-model="produtoNome" type="text" class="input" required />

      <label class="text-sm font-semibold" for="precoUnitario">Preço Unitário:</label>
      <input v-model="precoUnitario" type="number" class="input" required />

    </div>
    <thead>
      <tr>
        <th></th>
        <th>Name</th>
        <th>Job</th>
        <th>Favorite Color</th>
      </tr>
    </thead>
    <!-- Botão para Adicionar Produto à Lista -->
    <button type="button" @click="adicionarItem" class="btn bg-blue-500 text-white mt-2">Adicionar Item</button>
    <!-- Botão de Envio do Pedido -->
    <button type="button" @click="limparItensPedido" class="btn bg-gray-500 text-white mt-2">Limpar Itens do
      Pedido</button>

    <!-- Lista de Itens do Pedido -->
    <ul>
      <li v-for="(item, index) in itensPedido" :key="index">
        Quantidade: {{ item.quantidade }}, Nome do Produto: {{ item.nome }},
        Preço Unitário: {{ item.precoUnitario }}, Preço Total: {{ calcularPrecoTotal(item) }}
      </li>
    </ul>



    <!-- Exibição dos Detalhes do Pedido -->
    <div v-if="pedidoCriado" class="mt-8">
      <h2 class="text-2xl font-semibold">Pedido Criado!</h2>
      <p class="text-gray-700">Detalhes do Pedido:</p>
      <p>Data do Pedido: {{ dataPedido }}</p>
      <p>Cliente: {{ clienteNome }}</p>
      <p>Endereço:</p>
      <p>Bairro: {{ endereco.bairro }}</p>
      <p>Cidade: {{ endereco.cidade }}</p>
      <p>Rua: {{ endereco.rua }}</p>
      <p>Número: {{ endereco.numero }}</p>
      <p>Itens do Pedido:</p>
      <ul>
        <li v-for="(item, index) in itensPedido" :key="index">
          Quantidade: {{ item.quantidade }}, Nome do Produto: {{ item.nome }},
          Preço Unitário: {{ item.precoUnitario }}, Preço Total: {{ calcularPrecoTotal(item) }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PedidoForm',
  data() {
    return {
      dataPedido: '',
      clienteNome: '',
      clienteCpf: '',
      clienteFone: '',
      endereco: {
        bairro: '',
        cidade: '',
        rua: '',
        numero: '',
      },
      produtoNome: '',
      quantidade: 0,
      precoUnitario: 0,
      itensPedido: [],
      pedidoCriado: false,
    };
  },

  methods: {
    calcularPrecoTotalPedido() {
      // Calcula o preço total do pedido somando o preço total de cada item
      return this.itensPedido.reduce((total, item) => total + parseFloat(this.calcularPrecoTotal(item)), 0).toFixed(2);
    },
    limparFormulario() {
      // Limpa todos os campos do formulário
      this.clienteNome = '';
      this.clienteCpf = '';
      this.clienteFone = '';
      this.dataPedido = '';
      this.endereco.rua = '';
      this.endereco.bairro = '';
      this.endereco.cidade = '';
      this.endereco.numero = '';
      this.produtoNome = '';
      this.quantidade = 0;
      this.precoUnitario = 0;
      this.pedidoCriado = false;
    },

    limparItensPedido() {
      // Limpa a lista de itens do pedido
      this.itensPedido = [];
    },


    adicionarItem() {
      const novoItem = {
        quantidade: this.quantidade,
        nome: this.produtoNome,
        precoUnitario: this.precoUnitario,
      };

      this.itensPedido.push(novoItem);

      // Limpar os campos após adicionar o item
      this.produtoNome = '';
      this.quantidade = 0;
      this.precoUnitario = 0;
    },

    calcularPrecoTotal(item) {
      return (item.quantidade * item.precoUnitario).toFixed(2);
    },
  },
};
</script>

<style scoped>
/* Adicione estilos CSS aqui para personalizar a aparência do seu componente */
header.cabecalho {
  background-color: #e7e7e7;
  padding: 10px;
  text-align: center;
  width: 21cm;
}

header h1 {
  margin: 0;
}

.cabecalho-info {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}

.cabecalho-linha {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
}

form {
  margin-top: 20px;
}

form label {
  display: block;
  margin-bottom: 5px;
}

form input {
  width: 100%;
  padding: 8px;
  margin-bottom: 15px;
}

button {
  background-color: #4caf50;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* Estilo para a linha divisória */
hr {
  border: 1px solid #ccc;
  margin: 20px 0;
}
</style>
