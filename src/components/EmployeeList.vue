<template>
    <div class="employee-list">
        
         <!-- Table to display the employees information -->
        <table class="infoTable">
            <caption>Employee Information 

            <!-- filter and search for employees -->    
            <span>
                <input class="searchInput" type="text" v-model="searchQuery" placeholder="Search employees..." />
            </span>
            </caption>

            <thead>
                <tr>
                    <th>Employee ID</th>
                    <th>Name</th>
                    <th>Position</th>
                    <th>Department</th>
                    <th>Salary</th>
                    <th>Employment History</th>
                    <th>Contact</th>
                    <th>action</th>
                </tr>
            </thead>
            <tbody>  <!---employee information diplayed or filtered-->
                <tr v-for="employee in filteredEmployees" :key="employee.employeeId">
                    <td>{{ employee.employeeId }}</td>
                    <td>{{ employee.name }}</td>
                    <td>{{ employee.position }}</td>
                    <td>{{ employee.department }}</td>
                    <td>R{{ employee.salary }}</td>
                    <td>{{ employee.employmentHistory }}</td>
                    <td>{{ employee.contact }}</td>
                    <td><button class="rmvBtn" @click="removeEmployee(employee.employeeId)">Remove</button></td>
                </tr>
            </tbody>
        </table>

    <!-- Form to add new employees -->
    <div class="addEmployeeForm">
      <h3>Add New Employee</h3>

      <!-- error message for invalid form input -->
      <p v-if="errorMessage" class="errorMsg">Invalid input:{{ errorMessage }}</p>

      <form @submit.prevent="addEmployee">
        <input v-model="newEmployee.employeeId" placeholder="Employee ID" required />
        <input v-model="newEmployee.name" placeholder="Name" required />
        <input v-model="newEmployee.position" placeholder="Position" required />
        <input v-model="newEmployee.department" placeholder="Department" required />
        <input v-model="newEmployee.salary" placeholder="Salary" required />
        <input v-model="newEmployee.employmentHistory" placeholder="Employment History" />
        <input v-model="newEmployee.contact" placeholder="Contact" required />
        <button type="submit">Add Employee</button>
      </form>
    </div>
  </div>
</template>

<script>
import employeeInfo from '../assets/employee_info.json';

export default {
  name: "EmployeeList",
  data() {
    return {
      employees: [], // will be loaded from localStorage or JSON
      searchQuery: '',
      filteredEmployees: [],
      newEmployee: {
        employeeId: '',
        name: '',
        position: '',
        department: '',
        salary: '',
        employmentHistory: '',
        contact: ''
      },
      errorMessage: ''
    };
  },
  created() {
    // Load employees from localStorage if available, otherwise from JSON
    const savedEmployees = localStorage.getItem("employees");
    if (savedEmployees) {
      this.employees = JSON.parse(savedEmployees);
    } else {
      this.employees = employeeInfo.employeeInformation;
      localStorage.setItem("employees", JSON.stringify(this.employees));
    }
  },
  computed: {
    // filtere employee list
    filteredEmployees() {
      if (!this.searchQuery) return this.employees;
      const query = this.searchQuery.toLowerCase();
      return this.employees.filter(employee =>
        employee.name.toLowerCase().includes(query) ||
        employee.position.toLowerCase().includes(query) ||
        employee.department.toLowerCase().includes(query)
      );
    }
  },
  methods: {
    addEmployee() {
      this.errorMessage = '';

      // Validation checks for form inputs
      if (!/^\d+$/.test(this.newEmployee.employeeId)) {
        this.errorMessage = "Employee ID must be numeric.";
        return;
      }
      if (!/^[A-Za-z\s]+$/.test(this.newEmployee.name)) {
        this.errorMessage = "Name must contain only letters and spaces.";
        return;
      }
      if (!/^[A-Za-z\s]+$/.test(this.newEmployee.position)) {
        this.errorMessage = "Position must contain only letters and spaces.";
        return;
      }
      if (!/^[A-Za-z\s]+$/.test(this.newEmployee.department)) {
        this.errorMessage = "Department must contain only letters and spaces.";
        return;
      }
      if (!/^\d+(\.\d{1,2})?$/.test(this.newEmployee.salary)) {
        this.errorMessage = "Salary must be a valid number.";
        return;
      }
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      const phonePattern = /^\+?\d{7,15}$/;
      if (
        !emailPattern.test(this.newEmployee.contact) &&
        !phonePattern.test(this.newEmployee.contact)
      ) {
        this.errorMessage = "Contact must be a valid email or phone number.";
        return;
      }
      const exists = this.employees.some(
        emp => emp.employeeId === this.newEmployee.employeeId
      );
      if (exists) {
        this.errorMessage = "Employee ID already exists.";
        return;
      }

      // Add employee 
      this.employees.push({ ...this.newEmployee });

      // Save to localStorage
      localStorage.setItem("employees", JSON.stringify(this.employees));

      // Reset the form
      this.newEmployee = {
        employeeId: '',
        name: '',
        position: '',
        department: '',
        salary: '',
        employmentHistory: '',
        contact: ''
      };
    },

    removeEmployee(id) {
      if (confirm("Are you sure you want to remove this employee?")) {
        this.employees = this.employees.filter(emp => emp.employeeId !== id);

        // Update localStorage
        localStorage.setItem("employees", JSON.stringify(this.employees));
      }
    }
  }
};
</script>

<style scoped>
.employee-list {
    margin: 0vw;
    padding-top: 4vw;
}

.infoTable {
    width: 100%;
    border-collapse: collapse;
    margin-top: 2vw;
    border: 0.1vw solid #333;
}

.infoTable caption {
    font-size: 3vw;
    margin-bottom: 1vw;
    font-weight: bold;
}

.infoTable th, .infoTable td {
    border: 0.1vw solid #333;
    padding: 0.5vw;
    text-align: left;
    font-size: 1.3vw;
}

span .searchInput {
    position: absolute;
    padding: 0.5vw;
    font-size: 1.3vw;
    width: 20vw;
    right: 2.7vw;
    margin-top: 1vw;
}

.addEmployeeForm {
  margin-top: 2vw;
}

.addEmployeeForm input {
  margin: 0.5vw;
  padding: 0.5vw;
  font-size: 1.2vw;
}

.addEmployeeForm button {
  padding: 0.5vw 1vw;
  font-size: 1.2vw;
  cursor: pointer;
}

.rmvBtn{
    background-color: red;
    color:white;
}

.errorMsg{
    color: red;
}
</style>
