<template>
  <div>
    <h2>Payroll Analysis</h2>

    <table>
      <thead>
        <tr>
          <th>Employee ID</th>
          <th>Name</th>
          <th>Hours Worked</th>
          <th>Leave (Hours)</th>
          <th>Gross Salary</th>
          <th>Hourly Rate</th>
          <th>Deductions</th>
          <th>Tax Rate (%)</th>
          <th>Tax Amount</th>
          <th>Net Salary (after deductions and tax)</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="emp in payrollData" :key="emp.employeeId">
          <td>{{ emp.employeeId }}</td>
          <td>{{ emp.name || getEmployeeName(emp.employeeId) }}</td>
          <td>{{ emp.hoursWorked }}(h)</td>
          <td>{{ emp.leaveDeductions}}(h)</td>
          <td>R{{ emp.finalSalary }}</td>
          <td>R{{ calculateHourlyRate(emp).toFixed(2) }}</td>
          <td>R{{ (calculateDeductions(emp) + calculateTax(emp)).toFixed(2) }}</td>
          <td>{{ getTaxRate(emp).toFixed(0) }}%</td>
          <td>R{{ calculateTax(emp).toFixed(2) }}</td>
          <td>R{{ calculateNetSalary(emp).toFixed(2) }}</td>
          <td>
             <button @click="removePayroll(emp.employeeId)" class="rmvBtn">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

     <!-- Add employee payrol form -->
    <div class="addPayrollForm">
      <h3>Add New Payroll Entry</h3>
      <form @submit.prevent="addPayroll">
        <p>Employee ID</p>
        <input v-model="newPayroll.employeeId" placeholder="Employee ID" required />
        <p>Employee Name</p>
        <input v-model="newPayroll.name" placeholder="Employee Name" required />
        <p>Hours Worked</p>
        <input v-model.number="newPayroll.hoursWorked" placeholder="Hours Worked" required />
        <p>Leave (Hours)</p>
        <input v-model.number="newPayroll.leaveDeductions" placeholder="Leave (Hours)" required />
        <p>Gross Salary</p>
        <input v-model.number="newPayroll.finalSalary" placeholder="Gross Salary" required /><br>
        <button class="add-payroll" type="submit">Add Payroll</button>
      </form>
    </div>
</template>

<script>
import payrollDataInfo from '../assets/payroll_data.json';
import employeeInfo from '../assets/employee_info.json';

export default {
  name: "SalaryPage",
   data() {
    return {
      payrollData: [], // will be loaded from localStorage or json
      employees: employeeInfo.employeeInformation,
      newPayroll: {
        employeeId: '',
        name:"",
        hoursWorked: 0,
        leaveDeductions: 0,
        finalSalary: 0
      }
    }
  },
   created() {
    // Load payroll from localStorage if available, otherwise from json
    const savedPayroll = localStorage.getItem("payrollData");
    if (savedPayroll) {
      this.payrollData = JSON.parse(savedPayroll);
    } else {
      this.payrollData = payrollDataInfo.payrollData;
      localStorage.setItem("payrollData", JSON.stringify(this.payrollData));
    }
  },
  computed: {
    totalPayroll() {
      return this.payrollData.reduce((sum, emp) => sum + emp.finalSalary, 0);
    },
    totalTaxes() {
      return this.payrollData.reduce((sum, emp) => sum + this.calculateTax(emp), 0).toFixed(2);
    },
    totalDeductions() {
    return this.payrollData.reduce(
      (sum, emp) => sum + this.calculateDeductions(emp) + this.calculateTax(emp),
      0
      ).toFixed(2);   
    }
  },
  methods: {
    getEmployeeName(id) {
    const empInfo = this.employees.find(e => e.employeeId === id);
    return empInfo ? empInfo.name : "Unknown";
    },
    calculateHourlyRate(emp) {
      return emp.hoursWorked ? emp.finalSalary / emp.hoursWorked : 0;
    },
    calculateDeductions(emp) {
      return emp.leaveDeductions * this.calculateHourlyRate(emp);
    },
    getTaxRate(emp) {
      // Annual taxable income (monthly taxable × 12)
      const annualTaxableIncome = (emp.finalSalary - this.calculateDeductions(emp)) * 12;

      if (annualTaxableIncome <= 237100) return 0;
      if (annualTaxableIncome <= 370500) return 18;
      if (annualTaxableIncome <= 512800) return 26;
      if (annualTaxableIncome <= 673000) return 31;
      if (annualTaxableIncome <= 857900) return 36;
      if (annualTaxableIncome <= 1817000) return 39;
      return 45;
    },

    calculateTax(emp) {
      const monthlyTaxableIncome = emp.finalSalary - this.calculateDeductions(emp);
      const annualTaxableIncome = monthlyTaxableIncome * 12;
      const rate = this.getTaxRate(emp);
      const annualTax = (annualTaxableIncome * rate) / 100;
      return annualTax / 12;
    },

    calculateNetSalary(emp) {
      const monthlyTaxableIncome = emp.finalSalary - this.calculateDeductions(emp);
      return monthlyTaxableIncome - this.calculateTax(emp);
      },
    addPayroll() {
      const exists = this.payrollData.some(
        emp => emp.employeeId === this.newPayroll.employeeId
      );
      if (exists) {
        alert("Employee ID already exists in payroll.");
        return;
      }

      this.payrollData.push({ ...this.newPayroll });

      // Save to localStorage
      localStorage.setItem("payrollData", JSON.stringify(this.payrollData));

      // Reset form
      this.newPayroll = {
        employeeId: '',
        hoursWorked: 0,
        leaveDeductions: 0,
        finalSalary: 0
      };
    },

    // Remove payroll entry
    removePayroll(id) {
      if (confirm("Are you sure you want to remove this payroll entry?")) {
        this.payrollData = this.payrollData.filter(emp => emp.employeeId !== id);

        // Update localStorage
        localStorage.setItem("payrollData", JSON.stringify(this.payrollData));
      }
    }
  }
};
</script>

<style scoped>
  
table {
  width: 100%;
  margin-bottom: 20px;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  background: #f0f0f0;
}

td {
  text-align: center;
}

tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

.addPayrollForm{
  margin:3vw;
}

.add-payroll{
  margin-top: 3vw;
}

.rmvBtn{
    background-color: red;
    color:white;
}
</style>