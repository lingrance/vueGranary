<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-table :employees = "employees"
                    @edit:employee = "editEmployee"
                    @delete:employee="deleteEmployee">

    </employee-table>
    <employee-form @add:employee="addEmployee"></employee-form>
  </div>
 <!-- <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>-->
</template>

<script>
/*import HelloWorld from './components/HelloWorld.vue'*/
import EmployeeTable from '@/components/EmployeeTable'
import EmployeeForm from  '@/components/EmployeeForm'
export default {
  name: 'app',
  components: {
   /* HelloWorld,*/
    EmployeeTable,
    EmployeeForm,
  },
  data(){
    return {
      /*employees: [
        {
          id: 1,
          name: 'Richard Hendricks',
          email: 'richard@piedpiper.com',
        },
        {
          id: 2,
          name: 'Bertram Gilfoyle',
          email: 'gilfoyle@piedpiper.com',
        },
        {
          id: 3,
          name: 'Dinesh Chugtai',
          email: 'dinesh@piedpiper.com',
        },
      ],*/
      employees:[]
    }
  },

  mounted(){
    this.getEmployees()
  },
  methods:{

    async getEmployees(){
      try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          const data = await response.json()
          this.employees = data
      }catch (e) {
        console.error(e)
      }
    },
    async addEmployee(employee){
      try {
        const  response = await fetch('https://jsonplaceholder.typicode.com/users',{
            method: 'POST',
            body:JSON.stringify(employee),
            headers:{
              'Content-type':'application/json; charset=UTF-8'
          }
        });
        const data = await  response.json();
        this.employees = [...this.employees,data]

      }catch (e) {
        console.error(e)
      }
    },
    async editEmployee(id,updateEmployee){
      const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`,{
        method: 'PUT',
        body:JSON.stringify(updateEmployee),
        headers:{'Content-type': 'application/json; charset=UTF-8'}
      });

      const data  = await response.json();
      this.employees = this.employees.map(employee =>
              (employee.id === id ? data: employee)
      )

    },
    async deleteEmployee(id){
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`,{
          method: 'DELETE'
        });
        this.employees = this.employees.filter(employee => employee.id !== id);

      }catch (e) {
        console.error(e)
      }
    }

  /*  addEmployee(employee){
      const lastId =
              this.employees.length > 0
              ? this.employees[this.employees.length  -1].id
              : 0;
      const id =lastId +1;
      const newEmployee = {...employee,id}

      this.employees = [...this.employees,newEmployee]
    },
    editEmployee(id,updatedEmployee){
        this.employees = this.employees.map(employee =>
          employee.id == id ? updatedEmployee : employee
        )
    },
    deleteEmployee(id){
      this.employees = this.employees.filter(
              employee => employee.id !== id
      )
    }
*/
  },


}
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }
  .small-container {
    max-width: 680px;
  }
/*#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}*/
</style>
