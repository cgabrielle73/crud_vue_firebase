<template>
  <div id="view-employee">
    <ul class="collection with-header">
      <li class="collection-header">
        <h4>{{ nome }}</h4>
      </li>
      <li class="collection-item">ID do Empregado#: {{ employee_id }}</li>
      <li class="collection-item">Departamento: {{ dept }}</li>
      <li class="collection-item">Cargo: {{ cargo }}</li>
    </ul>
    <router-link to="/" class="btn grey"> Voltar </router-link>
    <button @click="deletarEmpregado" class="btn red">Deletar</button>

    <div class="fixed-action-btn">
      <router-link
        :to="{ name: 'edit-employee', params: { employee_id: employee_id } }"
        class="btn-floating btn-large red"
      >
        <i class="fa fa-pencil"> </i>
      </router-link>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "view-employee",
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
    deletarEmpregado() {
      if (confirm("Você tem certeza?")) {
        db.collection("empregados")
          .where("employee_id", "==", this.$route.params.employee_id)
          .get()
          .then((querySnapshot) => {
            querySnapshot.forEach((doc) => {
              doc.ref.delete();
              this.$router.push("/");
            });
          });
      }
    },
  },
};
</script>