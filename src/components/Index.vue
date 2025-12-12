<template>
  <div>
    <h2>Welcome!!!</h2>

    <div class="container">
      <!-- graph showing attendance data -->
      <div class="graph">
        <h3>Employee Attendance</h3>
        <BarChart :data="attendanceChartData" :options="attendanceOptions" />
      </div>

      <!-- graoh showing payroll data -->
      <div class="graph">
        <h3>Payroll Distribution</h3>
        <PieChart :data="payrollChartData" :options="payrollOptions" />
      </div>

      <!-- Graph showing hours workde-->
      <div class="graph">
        <h3>Hours Worked</h3>
        <LineChart :data="hoursChartData" :options="hoursOptions" />
      </div>

      <!-- graph showing average salary by department -->
      <div class="graph">
        <h3>Average Salary by Department</h3>
        <BarChart :data="departmentSalaryChartData" :options="departmentOptions" />
      </div>
    </div>
  </div>
</template>

<script>
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  ArcElement,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale
} from 'chart.js';

import { defineComponent } from 'vue';
import { Bar, Pie, Line } from 'vue-chartjs';
import attendance from '../assets/attendance.json';
import payrollDataInfo from '../assets/payroll_data.json';

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  ArcElement,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale
);

export default defineComponent({
  name: 'Index',
  components: {
    BarChart: Bar,
    PieChart: Pie,
    LineChart: Line
  },
  data() {
    const validPayroll = (payrollDataInfo.payrollData || []).filter(
      emp => emp && emp.employeeId && typeof emp.finalSalary === 'number'
    );

    // Group by department for 4th chart
    const departmentMap = {};
    validPayroll.forEach(emp => {
      if (!departmentMap[emp.department]) {
        departmentMap[emp.department] = [];
      }
      departmentMap[emp.department].push(emp.finalSalary);
    });

    const departments = Object.keys(departmentMap);
    const avgSalaries = departments.map(
      dept =>
        departmentMap[dept].reduce((a, b) => a + b, 0) /
        departmentMap[dept].length
    );

    return {
      // options
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      },

      // Attendance data chart
      attendanceChartData: {
        labels: ['Present', 'Absent'],
        datasets: [
          {
            label: 'Attendance Count',
            data: [attendance.present || 0, attendance.absent || 0],
            backgroundColor: ['#4CAF50', '#F44336']
          }
        ]
      },
      attendanceOptions: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          title: {
            display: true,
            text: 'Overall Attendance Status'
          }
        },
        scales: {
          y: {
            beginAtZero: true,
            ticks: {
              precision: 0
            }
          }
        }
      },
      // Payroll data chart
      payrollChartData: {
        labels: validPayroll.map(emp => emp.employeeId),
        datasets: [
          {
            label: 'Gross Salary',
            data: validPayroll.map(emp => emp.finalSalary),
            backgroundColor: validPayroll.map(
              (_, i) =>
                ['#2196F3', '#FF9800', '#9C27B0', '#4CAF50', '#F44336'][i % 5]
            )
          }
        ]
      },
      payrollOptions: {
        plugins: {
          title: {
            display: true,
            text: 'Salary Distribution Across Employees'
          },
          tooltip: {
            callbacks: {
              label: ctx => `Employee ${ctx.label}: R${ctx.raw}`
            }
          }
        }
      },

      // Hours worked chart
      hoursChartData: {
        labels: validPayroll.map(emp => emp.employeeId),
        datasets: [
          {
            label: 'Hours Worked',
            data: validPayroll.map(emp => emp.hoursWorked || 0),
            borderColor: '#FF9800',
            backgroundColor: '#FFCC80',
            fill: false
          }
        ]
      },
      hoursOptions: {
        plugins: {
          title: {
            display: true,
            text: 'Hours Worked Per Employee'
          },
          tooltip: {
            callbacks: {
              label: ctx => `${ctx.label}: ${ctx.raw} hours`
            }
          }
        }
      },

      // Department Salary Chart
      departmentSalaryChartData: {
        labels: departments,
        datasets: [
          {
            label: 'Average Salary',
            data: avgSalaries,
            backgroundColor: '#673AB7'
          }
        ]
      },
      departmentOptions: {
        plugins: {
          title: {
            display: true,
            text: 'Average Salary by Department'
          },
          tooltip: {
            callbacks: {
              label: ctx => `${ctx.label}: r${ctx.raw.toFixed(2)}`
            }
          }
        }
      }
    };
  }
});
</script>

<style scoped>
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1vw;

}

.graph {
  border: 1px solid #333;
  border-radius: 1vw;
  margin: 0.6vw;
  height: 46vh;
  padding: 1vw;
}
</style>