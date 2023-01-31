<template>
  <body>
    <section>
      <header>
        <h1>Formulário de Inscrição</h1>
      </header>

      <main>
        <div class="title">
          <h2>Faça Sua Inscrição:</h2>
        </div>

        <!--Criando Formulario-->
        <form action="" method="post" @submit.prevent>
          <div id="inputs">
            <div class="lineinput">
              <a>Nome: </a>
              <label for="nome"></label>
              <input
                type="text"
                id="nome"
                name="nome"
                v-model="nome"
                placeholder="nome"
                required
              />
            </div>

            <div class="lineinput">
              <a>SobreNome: </a>
              <label for="sobrenome"></label>
              <input
                type="text"
                id="sobrenome"
                name="sobrenome"
                v-model="sobrenome"
                placeholder="sobrenome"
              />
            </div>

            <div class="lineinput">
              <a>Empresa: </a>
              <label for="empresa"></label>
              <input
                type="text"
                id="empresa"
                v-model="empresa"
                placeholder="empresa"
                required
              />
            </div>

            <div>
              <a>Ativo: </a>
              <br />
              <input
                name="escolha"
                type="radio"
                v-model="choice"
                value="Sim"
                id="escolha"
                required
              />
              <label for="Sim"> Sim</label>
              <br />

              <input
                name="escolha"
                type="radio"
                v-model="choice"
                id="escolha"
                value="Não"
              />
              <label for="nome"> Não</label>
            </div>

            <div class="lineinput">
              <a>Telefone: </a>
              <label for="telefone"></label>
              <input
                type="text"
                id="telefone"
                name="telefone"
                v-model="telefone"
                placeholder="telefone"
                required
              />
            </div>
          </div>

          <!--Criando os botões para envio-->
          <div id="send">
            <button @click="enviarFormulario" type="submit" id="btn1">
              Cadastrar
            </button>

            <button @click="cancelar" id="btn2">
              Cancelar
            </button>
          </div>

          <!--Criando Filtro-->
          <div class="lineinput">
            <a> Filtro </a>
            <input type="text" class="m3-r" placeholder=" " v-model="search" />
            <button @click="limparFiltro" type="submit" id="botão">
              Limpar
            </button>
          </div>
        </form>

        <!--Cria a tabela com os valores-->
        <table border="1">
          <tr>
            <th>Id</th>
            <th>Nome</th>
            <th>Sobrenome</th>
            <th>Empresa</th>
            <th>Ativo</th>
            <th>Telefone</th>
            <th>Ações</th>
          </tr>

          <tr v-for="arr in itemsFiltered" :key="arr">
            <td>{{ arr.id }}</td>
            <td>{{ arr.nome }}</td>
            <td>{{ arr.sobrenome }}</td>
            <td>{{ arr.empresa }}</td>
            <td>{{ arr.choice }}</td>
            <td>{{ arr.telefone }}</td>
            <td>
              <span @click="remove(arr.id)" class="remove">Excluir</span>

              <span @click="preparaEdit(arr)" class="edit"> Editar </span>
            </td>
          </tr>
        </table>
      </main>
    </section>
  </body>
</template>

<script>
export default {
  data() {
    return {
      id: 1,
      nome: "",
      sobrenome: "",
      empresa: "",
      choice: "",
      telefone: "",
      dados: [],
      search: "",
      editId: null, //quando estiver diferente de nulo é uma edição, e quando tiver igual a nulo é um inserção
    };
  },

  computed: {
    itemsFiltered() {
      let valores = [];

      valores = this.dados.filter((item) => {
        return (
          item.nome.toLowerCase().indexOf(this.search.toLowerCase()) > -1 ||
          item.sobrenome.toLowerCase().indexOf(this.search.toLowerCase()) > -1 ||
          item.telefone.toLowerCase().indexOf(this.search.toLowerCase()) > -1 ||
          item.empresa.toLowerCase().indexOf(this.search.toLowerCase()) > -1
        );
      });

      return valores;
    },
  },

  methods: {
    enviarFormulario() {
      if (
        this.nome == "" ||
        this.empresa == "" ||
        this.choice == "" ||
        this.telefone == ""
      ) {
        alert(
          "Você deixou alguns campos obrigatorios em branco. Verifique e tente Novamente! "
        );
        return false;
      }

      //se o "this.editID" for igual a nulo vai add um novo usuario dentro do array
      if (this.editId == null) {
        this.dados.push({
          id: this.id++,
          nome: this.nome,
          sobrenome: this.sobrenome,
          empresa: this.empresa,
          choice: this.choice,
          telefone: this.telefone,
        });
      } else {
        this.atualizar(this.editId); //se for diferente de nulo ele chama o método atualizar
      }
      this.cancelar();
    },

    atualizar(id) {
      //acessando os itens do array pelo
      for (let i = 0; i < this.dados.length; i++) {
        if (this.dados[i].id == id) {
          this.dados[i].nome = this.nome;
          this.dados[i].sobrenome = this.sobrenome;
          this.dados[i].empresa = this.empresa;
          this.dados[i].choice = this.choice;
          this.dados[i].telefone = this.telefone;
        }
      }
    },

    remove(id) {
      if (confirm("Deseja realmente deletar o usuario do ID" + id)) {
        //criamos um for, cm uma variavel 'i', o for vai verificar até que 'i ' seja menor que os itens do array
        for (let i = 0; i < this.dados.length; i++) {
          if (this.dados[i].id == id) {
            this.dados.splice(i, 1);
          }
        }
      }
    },

    preparaEdit(informacoes) {
      this.editId = informacoes.id;

      //pegando todos os dados e jogando de volta para dentro do input usando o v-model dos inputs
      this.nome = informacoes.nome;
      this.sobrenome = informacoes.sobrenome;
      this.empresa = informacoes.empresa;
      this.choice = informacoes.choice;
      this.telefone = informacoes.telefone;

      //quando apertar pra editar ele muda o texto do botão para atualizar
      document.getElementById("btn1").innerText = "Atualizar";
    },

    limparFiltro() {
      this.search = "";
    },

    cancelar(){
      this.nome = "";
      this.sobrenome = "";
      this.empresa = "";
      this.choice = "";
      this.telefone = "";

      document.getElementById('btn1').innerText = 'Cadastrar';
      this.editId = null
      
    }
  },
};
</script>

<style>
#app {
  display: block !important;
}
* {
  margin: 0%;
}

body {
  background-color: #fafbfc;
  display: grid !important;
  place-items: center !important;
}

header {
  background-color: #80ced6;
  padding: 10px;
  color: #fff;
  text-align: center;
}

main {
  padding: 30px 50px;
}

.inputs {
  background-color: #fff;
  border: #ccc;
  padding: 20px;
  margin-top: 15px;
  border-radius: 5px;
}

.lineinput {
  display: grid;
  margin-bottom: 10px;
}

input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 3px;
  outline: 0;
}

.content {
  margin-top: 15px;
}

table {
  border-collapse: collapse;
  border: 1px solid #ccc;
  width: 100%;
}

table th {
  background-color: rgb(214, 214, 214);
}

table th,
td {
  padding: 1px 5px;
  text-align: left;
}

table td img {
  width: 22px;
}

.remove {
  padding: 2px 2px;
  background-color: red;
  color: #fff;
  border-radius: 30px;
}

.remove:hover {
  cursor: pointer;
}

.edit {
  padding: 2px 2px;
  background-color: rgb(0, 255, 34);
  color: #fff;
  border-radius: 30px;
}

.edit:hover {
  cursor: pointer;
}
</style>