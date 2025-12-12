<template>
   <div>
    <h2>Employee Attendance & Leave Analysis</h2>
    
    <div v-for="employee in attendanceAndLeave" :key="employee.employeeId" class="employee-section">
      <h3>{{ employee.name }} (EmployeeID: {{ employee.employeeId }})</h3>
      <p><strong>Days Worked:</strong> {{ calculateDaysWorked(employee) }}</p>

      <!-- Attendance Table -->
      <h4>Attendance</h4>
      <table>
        <thead>
          <tr>
            <th>Date</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="record in employee.attendance" :key="record.date">
            <td>{{ record.date }}</td>
            <td :class="record.status.toLowerCase()">{{ record.status }}</td>
          </tr>
        </tbody>
      </table>

      <!-- Leave Requests Table -->
      <h4>Leave Requests</h4>
      <table>
        <thead>
          <tr>
            <th>Date</th>
            <th>Reason</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="leave in employee.leaveRequests" :key="leave.date">
            <td>{{ leave.date }}</td>
            <td>{{ leave.reason }}</td>
            <td :class="leave.status.toLowerCase()">{{ leave.status }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
    import attendanceData from '../assets/attendance.json';

    export default{
        name:"AttendancePage",
        data(){
            return{
                attendanceAndLeave: attendanceData.attendanceAndLeave,
            }
        }
        ,methods:{
    // Calculate total days worked
    calculateDaysWorked(employee) {
        return employee.attendance.filter(record => record.status === 'Present').length;
        }
    }
 }

</script>

<style scoped>
.employee-section {
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #ccc;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 15px;
}

th, td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f4f4f4;
}

td.present {
  background-color: #d4edda;
}

td.absent {
  background-color: #f8d7da;
}

td.approved {
  background-color: #d1ecf1;
}

td.pending {
  background-color: #fff3cd;
}

td.rejected {
  background-color: #f8d7da;
}

</style>