<template>
  <div id="dashboard">
    <ul class="collection with-header">
      <li class="collection-header"><h4>Empregados</h4></li>
      <li
        v-for="employee in employees"
        :key="employee.id"
        class="collection-item"
      >
        <div class="chip">{{ employee.dept }}</div>
        {{ employee.employee_id }}: {{ employee.nome }}
        <router-link
          class="secondary-content"
          v-bind:to="{
            name: 'view-employee',
            params: { employee_id: employee.employee_id },
          }"
          ><i class="fa fa-eye"></i
        ></router-link>
      </li>
    </ul>

    <div class="fixed-action-btn">
      <router-link to="/new" class="btn-floating btn-large red">
        <i class="fa fa-plus"> </i>
      </router-link>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "dashboard",
  data() {
    return {
      employees: [],
    };
  },
  created() {
    db.collection("empregados")
      .orderBy("dept")
      .get()
      .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          const data = {
            id: doc.id,
            employee_id: doc.data().employee_id,
            nome: doc.data().nome,
            dept: doc.data().dept,
            cargo: doc.data().cargo,
          };
          this.employees.push(data);
        });
      });
  },
};
</script>