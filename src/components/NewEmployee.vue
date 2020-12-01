<template>
  <div id="new-employee">
    <h3>Cadastrar Empregados</h3>
    <div class="row">
      <form @submit.prevent="salvarEmpregado" class="col s12">
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="employee_id" required />
            <label> ID Empregado#</label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="nome" required />
            <label> Nome</label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="dept" required />
            <label> Departamento </label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="cargo" required />
            <label> Cargo </label>
          </div>
        </div>
        <button type="submit" class="btn">Enviar</button>
        <router-link to="/" class="btn grey"> Cancelar </router-link>
      </form>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "new-employee",
  data() {
    return {
      employee_id: null,
      nome: null,
      dept: null,
      cargo: null,
    };
  },
  methods: {
    salvarEmpregado() {
      db.collection("empregados")
        .add({
          employee_id: this.employee_id,
          nome: this.nome,
          dept: this.dept,
          cargo: this.cargo,
        })
        .then((docRef) => this.$router.push("/"))
        .catch((error) => console.log(err));
    },
  },
};
</script>