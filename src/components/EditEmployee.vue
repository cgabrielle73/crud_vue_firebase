<template>
  <div id="edit-employee">
    <h3>Editar Empregados</h3>
    <div class="row">
      <form @submit.prevent="atualizarEmpregado" class="col s12">
        <div class="row">
          <div class="input-field col s12">
            <input disabled type="text" v-model="employee_id" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="nome" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="dept" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="cargo" required />
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
  name: "edit-employee",
  data() {
    return {
      employee_id: null,
      nome: null,
      dept: null,
      cargo: null,
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("empregados")
      .where("employee_id", "==", to.params.employee_id)
      .get()
      .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          next((vm) => {
            vm.employee_id = doc.data().employee_id;
            vm.nome = doc.data().nome;
            vm.dept = doc.data().dept;
            vm.cargo = doc.data().cargo;
          });
        });
      });
  },
  watch: {
    $route: "fetchData",
  },
  methods: {
    fetchData() {
      db.collection("empregados")
        .where("employee_id", "==", this.$route.params.employee_id)
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            this.employee_id = doc.data().employee_id;
            this.nome = doc.data().nome;
            this.dept = doc.data().dept;
            this.cargo = doc.data().cargo;
          });
        });
    },
    // linha para att os dados do empregado
    atualizarEmpregado() {
      db.collection("empregados")
        .where("employee_id", "==", this.$route.params.employee_id)
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            doc.ref
              .update({
                employee_id: this.employee_id,
                nome: this.nome,
                dept: this.dept,
                cargo: this.cargo,
              })
              .then(() => {
                this.$router.push({
                  name: "view-employee",
                  params: { employee_id: this.employee_id },
                });
              });
          });
        });
    },
  },
};
</script>