<template>
  <div>
    <input v-model="searchQuery" @input="buscarOperadoras" placeholder="Digite o nome da operadora..." />
    <table>
      <thead>
        <tr>
          <th>Registro ANS</th>
          <th>CNPJ</th>
          <th>Razão Social</th>
          <th>Nome Fantasia</th>
          <th>Modalidade</th>
          <th>Logradouro</th>
          <th>Número</th>
          <th>Complemento</th>
          <th>Bairro</th>
          <th>Cidade</th>
          <th>UF</th>
          <th>CEP</th>
          <th>DDD</th>
          <th>Telefone</th>
          <th>Fax</th>
          <th>Endereço Eletrônico</th>
          <th>Representante</th>
          <th>Cargo Representante</th>
          <th>Região de Comercialização</th>
          <th>Data Registro ANS</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="registro in registros" :key="registro.Registro_ANS">
          <td>{{ registro.Registro_ANS || '-' }}</td>
          <td>{{ registro.CNPJ || '-' }}</td>
          <td>{{ registro.Razao_Social || '-' }}</td>
          <td>{{ registro.Nome_Fantasia || '-' }}</td>
          <td>{{ registro.Modalidade || '-' }}</td>
          <td>{{ registro.Logradouro || '-' }}</td>
          <td>{{ registro.Numero || '-' }}</td>
          <td>{{ registro.Complemento || '-' }}</td>
          <td>{{ registro.Bairro || '-' }}</td>
          <td>{{ registro.Cidade || '-' }}</td>
          <td>{{ registro.UF || '-' }}</td>
          <td>{{ registro.CEP || '-' }}</td>
          <td>{{ registro.DDD || '-' }}</td>
          <td>{{ registro.Telefone || '-' }}</td>
          <td>{{ registro.Fax || '-' }}</td>
          <td>{{ registro.Endereco_eletronico || '-' }}</td>
          <td>{{ registro.Representante || '-' }}</td>
          <td>{{ registro.Cargo_Representante || '-' }}</td>
          <td>{{ registro.Regiao_de_Comercializacao || '-' }}</td>
          <td>{{ registro.Data_Registro_ANS || '-' }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
const apiUrl = process.env.VUE_APP_API_URL;

export default {
  data() {
    return {
      searchQuery: "",
      registros: [],
    };
  },
  methods: {
    async buscarOperadoras() {
      if (this.searchQuery.length < 2) {
        this.registros = [];
        return;
      }

      try {
        const response = await axios.get(`${apiUrl}/operadoras?q=${this.searchQuery}`);
        // Garante que os campos não sejam `null` ou `undefined`
        this.registros = response.data.map(op => ({
          ...op,
          
          Nome_Fantasia: op.Nome_Fantasia?.trim() || '-',
          Razao_Social: op.Razao_Social?.trim() || '-',
          Cidade: op.Cidade?.trim() || '-',
          UF: op.UF?.trim() || '-',
        }));
      } catch (error) {
        console.error("Erro ao buscar operadoras:", error);
      }
    },
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  background-color: #f2f2f2;
}

input {
  margin-bottom: 20px;
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
}
</style>