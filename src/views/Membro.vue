<template>
  <body>
    <div class="card-body">
      <div
        class="col-lg-12"
        v-if="autorizacao === '[ROLE_ADMIN]' || autorizacao === '[ROLE_USER]'"
      >
        <div class="col-lg-6" style="text-align: left;">
          <button
            type="button"
            class="btn btn-primary  ico-user mb-2"
            v-on:click="inicio"
            style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive"
          >
            Início
          </button>
        </div>
        <div class="col-lg-6" style="text-align: right;">
          <button
            type="button"
            class="btn btn-primary mb-2"
            v-on:click="sair"
            style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive"
          >
            Sair
          </button>
        </div>
      </div>

      <p
        class="h2"
        style="font-size: 400%; font-family:Comic Sans MS, Comic Sans, cursive"
        face="Arial"
      >
        Membros
      </p>
      <div class="col-lg-12" style="text-align: right;">
        <button
          type="submit"
          class="btn btn-primary  ico-user mb-2"
          v-on:click="open"
          style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive"
        >
          Adicionar
        </button>
      </div>
      <div>
        <form>
          <div class="form-group col-md-4">
            <label
              for="conteudoTxt"
              style="font-size: 200%; font-family:Comic Sans MS, Comic Sans, cursive"
              >Filtro de pesquisa</label
            >
            <select
              class="form-control"
              v-model="tipoDeBusca"
              style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive; height: 50px"
            >
              <option>Patente</option>
              <option>Nome</option>
              <option>Apelido</option>
              <option>Todos Membros</option>
            </select>
          </div>
          <div class="form-group col-md-4">
            <label
              for="conteudoTxt"
              style="font-size: 200%; font-family:Comic Sans MS, Comic Sans, cursive"
              >Informações para busca</label
            >
            <input
              type="text"
              id="conteudoTxt"
              class="form-control"
              required
              v-model="conteudoTxt"
              placeholder="Ex: Nome, Apelido ou Patente"
              style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive; height: 50px;"
            />
          </div>

          <div
            class="form-group col-md-1"
            style="font-size: 110%;margin-top: 25px"
          >
            <label for="conteudoTxt"></label>
            <button
              type="button"
              style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive"
              class="btn btn-primary  mb-2"
              v-on:click="busca(conteudoTxt)"
            >
              Buscar
            </button>
          </div>
        </form>
      </div>
      <table id="membro" class="table table-striped" v-if="showMembros">
        <thead>
          <br />
          <br />
          <tr
            style="font-size: 150%; font-family:Comic Sans MS, Comic Sans, cursive"
          >
            <th id="cabecalho">ID</th>
            <th id="cabecalho">Nome</th>
            <th id="cabecalho">Apelido</th>
            <th id="cabecalho">Patente</th>
            <th id="cabecalho" scope="col">Ações</th>
          </tr>
        </thead>
        <tbody
          style="font-size: 120%; font-family:Comic Sans MS, Comic Sans, cursive"
        >
          <tr v-for="membro in membros" :key="membro.id">
            <td>{{ membro.id }}</td>
            <td>{{ membro.nome }}</td>
            <td>{{ membro.apelido }}</td>
            <td>{{ membro.patente }}</td>

            <button
              id="glyphicon2"
              class=" glyphicon glyphicon-trash mr-1"
              type="submit"
              style="color:red"
              v-on:click="excluir(membro.id)"
            ></button>

            <button
              id="glyphicon2"
              type="button"
              class="glyphicon glyphicon-pencil mr-1"
              v-on:click="open(membro)"
              style="color:blue"
            >
              <span class="glyphicon glyphicon" aria-hidden="true"></span>
            </button>
          </tr>
        </tbody>
      </table>

      <vue-modal-2
        name="modal-1"
        @on-close="close"
        noHeader
        noFooter
        class="modal-dialog modal-lg"
      >
        <header>
          <br />
        </header>
        <form
          @submit.prevent="cadastrar"
          style="font-size: 120%; font-family:Comic Sans MS, Comic Sans, cursive"
        >
          <div class="form-group row" v-if="showId">
            <label for="id">id</label>
            <input
              type="number"
              id="id"
              class="form-control"
              required
              autofocus
              v-model="id"
              readonly
            />
          </div>
          <div class="form-group col-md-12">
            <label for="nome">Nome</label>
            <input
              type="text"
              id="nome"
              class="form-control"
              required
              autofocus
              v-model="nome"
            />
          </div>

          <div class="form-group col-md-12">
            <label for="apelido">Apelido</label>
            <input
              type="text"
              id="apelido"
              class="form-control"
              required
              v-model="apelido"
            />
          </div>
          <div class="form-group col-md-12">
            <label for="patente">Patente</label>
            <input
              type="text"
              id="patente"
              class="form-control"
              required
              v-model="patente"
            />
          </div>
          <div class="btn-group">
            <button
              style="margin: 0 15px;"
              type="button"
              class="btn btn-danger"
              v-on:click="close"
            >
              Cancelar
            </button>

            <button
              style="margin: 0 15px;"
              class="btn btn btn-primary"
              type="submit"
            >
              Salvar
            </button>
          </div>
        </form>
        <footer>
          <br />
        </footer>
      </vue-modal-2>
    </div>
  </body>
</template>
<script>
import Vue from "vue";
import axios from "axios";
import { mapState } from "vuex";
import Modal from "@burhanahmeed/vue-modal-2";
import moment from "moment";

Vue.prototype.moment = moment;
Vue.component("modal", {
  template: "#modal-template",
});
Vue.config.productionTip = false;
Vue.use(Modal);
export default {
  data() {
    return {
      showMembros: false,
      showMembro: false,
      showId: false,
      id: null,
      nome: "",
      apelido: "",
      patente: "",
      membro: "",
      membros: [],
      tipoDeBusca: null,
    };
  },
  computed: {
    ...mapState(["usuario", "autorizacao"]),
  },
  methods: {
    busca(conteudoTxt) {
      this.showMembros = false;
      this.membros = [];
      if (this.tipoDeBusca == "Todos Membros") {
        this.buscarTodos();
        conteudoTxt = "";
      }
      if (this.tipoDeBusca == "Nome") {
        if (conteudoTxt == null) {
          alert("Digite um nome");
        } else if (conteudoTxt.length > 50) {
          alert("Nome muito grande");
        } else {
          this.buscarNome(conteudoTxt);
        }
      }
      if (this.tipoDeBusca == "Apelido") {
        if (conteudoTxt == null) {
          alert("Digite um Apelido");
        } else if (conteudoTxt.length > 30) {
          alert("Apelido muito grande");
        } else this.buscarApelido(conteudoTxt);
      }
      if (this.tipoDeBusca == "Patente") {
        if (conteudoTxt == null) {
          alert("Digite uma patente");
        } else this.buscarPatente(conteudoTxt);
      }
      if (this.tipoDeBusca == null) {
        alert("Escolha um filtro para pesquisa");
      }
    },

    buscarTodos() {
      axios
        .get("/membros/all", { headers: { Accept: "application/json" } })
        .then((res) => {
          this.showMembros = true;
          console.log(res);
          this.membros = res.data;
          this.total = this.membros.count();
        })
        .catch((error) => console.log(error));
    },

    buscarApelido(apelido) {
      axios
        .get("/membros/apelido/" + apelido, {
          headers: { Accept: "application/json" },
        })
        .then((res) => {
          this.showMembros = true;
          console.log(res);
          this.membro = res.data;
          this.membros.push(this.membro);
        })
        .catch((error) => console.log(error));
    },

    buscarNome(nome) {
      axios
        .get("/membros/nome/" + nome, {
          headers: { Accept: "application/json" },
        })
        .then((res) => {
          this.showMembros = true;
          console.log(res);
          this.membros = res.data;
        })
        .catch((error) => console.log(error));
    },
    buscarPatente(patente) {
      axios
        .get("/membros/patente/" + patente, {
          headers: { Accept: "application/json" },
        })
        .then((res) => {
          this.showMembros = true;
          console.log(res);
          this.membros = res.data;
        })
        .catch((error) => console.log(error));
    },

    excluir(id) {
      var resposta = confirm("Deseja remover esse registro?");
      if (resposta == true) {
        axios
          .delete("/membros/delete/" + id)
          .then((response) => {
            this.membros = [];
            this.showMembros = false;
            alert("Deletado com sucesso !");

            console.log(response);
          })
          .catch((error) => console.log(error));
      }
    },

    open(membro) {
      this.$vm2.open("modal-1");
      if (membro.id != null) {
        this.id = membro.id;
        this.nome = membro.nome;
        this.apelido = membro.apelido;
        this.patente = membro.patente;
      }
    },

    inicio() {
      this.$router.push("/home");
    },

    sair() {
      this.$store.commit("logout");
      this.$router.push("/");
    },
    close() {
      this.$vm2.close("modal-1");
      this.id = null;
      this.nome = "";
      this.apelido = "";
      this.patente = "";
    },

    cadastrar() {
      if (this.nome.length <= 3) {
        alert("Nome muito pequeno");
      } else if (this.nome.length > 50) {
        alert("Nome muito grande");
      } else if (this.apelido.length > 30) {
        alert("Apelido muito grande");
      } else if (this.idMembro == null) {
        const membroDTO = {
          nome: this.nome,
          apelido: this.apelido,
          patente: this.patente,
        };
        axios
          .post("/membros/create", membroDTO)

          .then((res) => {
            console.log(res);
            this.close();
            alert("Cadastro efetuado com sucesso!!!");
            this.membros = [];
            this.showMembros = false;
          })
          .catch((error) => console.log(error));
      } else {
        this.atualizar();
      }
    },

    atualizar() {
      const membroDTO = {
        nome: this.nome,
        apelido: this.apelido,
        patente: this.patente,
      };

      axios
        .put("/membros/put/" + this.id, membroDTO)

        .then((res) => {
          console.log(res);
          this.close();

          alert("Cadastro atualizado com sucesso!!!");
          this.membros = [];
          this.showMembros = false;
        })
        .catch((error) => console.log(error));
    },
    isNumber(n) {
      return !isNaN(parseInt(n)) & isFinite(n);
    },
  },
};
</script>
<style>
#cabecalho {
  text-align: center;
}

#glyphicon2 {
  font-size: 20px;
}

body {
  background-image: linear-gradient(to top, #cfd9df 0%, #e2ebf0 100%);
}
</style>
