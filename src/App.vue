<script setup>
import { ref, computed } from 'vue'

const studentName = ref('')
const supportType = ref('')
const notes = ref('')
const priority = ref('Medium')
const searchTerm = ref('')

const logs = ref([
  {
    id: 1,
    student: 'John Martinez',
    type: 'Academic',
    priority: 'Medium',
    notes: 'Helped with reading comprehension and assignment directions.',
    date: '5/8/26',
  },
  {
    id: 2,
    student: 'Maria Lopez',
    type: 'Behavioral',
    priority: 'High',
    notes: 'Completed a calm-down check-in after transition.',
    date: '5/8/26',
  },
  {
    id: 3,
    student: 'Alex Rivera',
    type: 'Transition',
    priority: 'Low',
    notes: 'Supported hallway transition to next class.',
    date: '5/8/26',
  },
])

const totalLogs = computed(() => logs.value.length)

const studentsAssisted = computed(() => {
  return new Set(logs.value.map((log) => log.student)).size
})

const highPriorityLogs = computed(() => {
  return logs.value.filter((log) => log.priority === 'High').length
})

const filteredLogs = computed(() => {
  return logs.value.filter((log) =>
    log.student.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
    log.type.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
    log.notes.toLowerCase().includes(searchTerm.value.toLowerCase())
  )
})

function addLog() {
  if (!studentName.value || !supportType.value || !notes.value) {
    alert('Please complete all fields before submitting the log.')
    return
  }

  logs.value.unshift({
    id: Date.now(),
    student: studentName.value,
    type: supportType.value,
    priority: priority.value,
    notes: notes.value,
    date: new Date().toLocaleDateString(),
  })

  studentName.value = ''
  supportType.value = ''
  notes.value = ''
  priority.value = 'Medium'

  alert('Support log submitted successfully.')
}

function deleteLog(id) {
  logs.value = logs.value.filter((log) => log.id !== id)
}

function clearLogs() {
  if (confirm('Are you sure you want to clear all logs?')) {
    logs.value = []
  }
}
</script>

<template>
  <div class="app">
    <aside class="sidebar">
      <div class="brand">
        <div class="logo">SP</div>
        <div>
          <h2>SpEd Support</h2>
          <p>Student Support Log</p>
        </div>
      </div>

      <nav>
        <a href="#dashboard">📊 Dashboard</a>
        <a href="#log">📝 Log Interaction</a>
        <a href="#history">📚 History</a>
        <a href="#reports">📈 Reports</a>
      </nav>

      <div class="sidebar-footer">
        <p>Final Project MVP</p>
        <span>Vue Frontend</span>
      </div>
    </aside>

    <main class="main-content">
      <header id="dashboard">
        <p class="eyebrow">Special Education Documentation System</p>
        <h1>Support Interaction Dashboard</h1>
        <p class="subtitle">
          Track student support interactions, review recent logs, and monitor daily documentation progress.
        </p>
      </header>

      <section class="hero-card">
        <div>
          <h2>Welcome Back, Support Staff 👋</h2>
          <p>
            Use this dashboard to quickly document student support, behavior check-ins,
            academic assistance, and transition support.
          </p>
        </div>
        <a href="#log" class="hero-button">Create New Log</a>
      </section>

      <section class="stats">
        <div class="card">
          <span class="card-icon">🗂️</span>
          <h3>Total Logs</h3>
          <p>{{ totalLogs }}</p>
        </div>

        <div class="card">
          <span class="card-icon">👥</span>
          <h3>Students Assisted</h3>
          <p>{{ studentsAssisted }}</p>
        </div>

        <div class="card">
          <span class="card-icon">⚠️</span>
          <h3>High Priority</h3>
          <p>{{ highPriorityLogs }}</p>
        </div>
      </section>

      <section class="content-grid">
        <div class="panel" id="log">
          <div class="section-heading">
            <h2>Log New Interaction</h2>
            <p>Document student support quickly and clearly.</p>
          </div>

          <form @submit.prevent="addLog">
            <label>
              Student Name
              <input v-model="studentName" type="text" placeholder="Example: Sofia Garcia" />
            </label>

            <label>
              Support Type
              <select v-model="supportType">
                <option value="">Select Support Type</option>
                <option>Academic</option>
                <option>Behavioral</option>
                <option>Transition</option>
                <option>Check-In</option>
                <option>Other</option>
              </select>
            </label>

            <label>
              Priority
              <select v-model="priority">
                <option>Low</option>
                <option>Medium</option>
                <option>High</option>
              </select>
            </label>

            <label>
              Interaction Notes
              <textarea v-model="notes" placeholder="Write a short note about the support provided."></textarea>
            </label>

            <button type="submit">Submit Support Log</button>
          </form>
        </div>

        <div class="panel" id="reports">
          <div class="section-heading">
            <h2>Daily Report Summary</h2>
            <p>Quick overview of current documentation.</p>
          </div>

          <div class="report-box">
            <strong>Total Support Interactions</strong>
            <span>{{ totalLogs }} logs recorded</span>
          </div>

          <div class="report-box">
            <strong>Unique Students Assisted</strong>
            <span>{{ studentsAssisted }} students documented</span>
          </div>

          <div class="report-box">
            <strong>High Priority Interactions</strong>
            <span>{{ highPriorityLogs }} logs marked high priority</span>
          </div>

          <button type="button" class="danger-btn" @click="clearLogs">
            Clear All Logs
          </button>
        </div>
      </section>

      <section class="panel history-panel" id="history">
        <div class="history-top">
          <div class="section-heading">
            <h2>Recent Interactions</h2>
            <p>Search and review submitted support logs.</p>
          </div>

          <input
            v-model="searchTerm"
            class="search"
            type="text"
            placeholder="Search by student, type, or notes..."
          />
        </div>

        <table v-if="filteredLogs.length > 0">
          <thead>
            <tr>
              <th>Student</th>
              <th>Type</th>
              <th>Priority</th>
              <th>Notes</th>
              <th>Date</th>
              <th>Action</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="log in filteredLogs" :key="log.id">
              <td class="student-name">{{ log.student }}</td>
              <td><span class="tag blue">{{ log.type }}</span></td>
              <td>
                <span
                  class="tag"
                  :class="{
                    green: log.priority === 'Low',
                    yellow: log.priority === 'Medium',
                    red: log.priority === 'High'
                  }"
                >
                  {{ log.priority }}
                </span>
              </td>
              <td>{{ log.notes }}</td>
              <td>{{ log.date }}</td>
              <td>
                <button class="delete-btn" @click="deleteLog(log.id)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>

        <p v-else class="empty-message">
          No support logs match your search.
        </p>
      </section>
    </main>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Inter', sans-serif;
}

html {
  scroll-behavior: smooth;
}

body {
  background: linear-gradient(135deg, #eef4ff, #f8fbff);
  color: #0f172a;
}

.app {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  width: 280px;
  background: linear-gradient(180deg, #132f4c, #1e3a5f);
  color: white;
  padding: 28px 22px;
  position: sticky;
  top: 0;
  height: 100vh;
  box-shadow: 10px 0 30px rgba(15, 23, 42, 0.12);
}

.brand {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 38px;
}

.logo {
  width: 48px;
  height: 48px;
  background: linear-gradient(135deg, #60a5fa, #34d399);
  border-radius: 16px;
  display: grid;
  place-items: center;
  font-weight: 800;
  color: white;
}

.brand h2 {
  font-size: 21px;
}

.brand p {
  font-size: 12px;
  color: #cbd5e1;
  margin-top: 3px;
}

.sidebar nav {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.sidebar a {
  color: #e2e8f0;
  text-decoration: none;
  font-size: 16px;
  padding: 13px 14px;
  border-radius: 14px;
  transition: all 0.2s ease;
}

.sidebar a:hover {
  background: rgba(255, 255, 255, 0.14);
  color: white;
  transform: translateX(4px);
}

.sidebar-footer {
  position: absolute;
  bottom: 28px;
  left: 22px;
  right: 22px;
  background: rgba(255, 255, 255, 0.11);
  padding: 16px;
  border-radius: 16px;
}

.sidebar-footer p {
  font-weight: 700;
}

.sidebar-footer span {
  display: block;
  margin-top: 4px;
  color: #cbd5e1;
  font-size: 13px;
}

.main-content {
  flex: 1;
  padding: 42px;
}

.eyebrow {
  color: #2563eb;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 13px;
}

header h1 {
  font-size: 42px;
  color: #0f172a;
  margin-top: 8px;
  letter-spacing: -0.04em;
}

.subtitle {
  margin-top: 12px;
  color: #64748b;
  font-size: 17px;
  max-width: 760px;
  line-height: 1.6;
}

.hero-card {
  margin-top: 28px;
  background: linear-gradient(135deg, #2563eb, #3b82f6);
  color: white;
  border-radius: 26px;
  padding: 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 18px 35px rgba(37, 99, 235, 0.25);
}

.hero-card h2 {
  font-size: 26px;
  margin-bottom: 8px;
}

.hero-card p {
  max-width: 720px;
  color: #dbeafe;
  line-height: 1.6;
}

.hero-button {
  background: white;
  color: #2563eb;
  text-decoration: none;
  padding: 14px 18px;
  border-radius: 14px;
  font-weight: 800;
  white-space: nowrap;
  transition: all 0.2s ease;
}

.hero-button:hover {
  transform: scale(1.04);
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 22px;
  margin-top: 28px;
}

.card,
.panel {
  background: rgba(255, 255, 255, 0.92);
  border: 1px solid #e2e8f0;
  border-radius: 24px;
  box-shadow: 0 12px 28px rgba(15, 23, 42, 0.08);
}

.card {
  padding: 24px;
  transition: all 0.2s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 18px 35px rgba(15, 23, 42, 0.13);
}

.card-icon {
  font-size: 28px;
}

.card h3 {
  color: #64748b;
  margin-top: 14px;
  font-size: 15px;
}

.card p {
  font-size: 38px;
  color: #0f172a;
  font-weight: 800;
  margin-top: 8px;
}

.content-grid {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 24px;
  margin-top: 30px;
}

.panel {
  padding: 26px;
}

.section-heading h2 {
  color: #0f172a;
  font-size: 23px;
}

.section-heading p {
  color: #64748b;
  margin-top: 6px;
  margin-bottom: 20px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

label {
  display: flex;
  flex-direction: column;
  gap: 8px;
  color: #334155;
  font-weight: 700;
  font-size: 14px;
}

input,
select,
textarea {
  padding: 14px;
  border: 1px solid #cbd5e1;
  border-radius: 14px;
  font-size: 15px;
  outline: none;
  background: #f8fafc;
  transition: all 0.2s ease;
}

input:focus,
select:focus,
textarea:focus {
  border-color: #2563eb;
  background: white;
  box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.12);
}

textarea {
  min-height: 120px;
  resize: vertical;
}

button {
  background: linear-gradient(135deg, #2563eb, #3b82f6);
  color: white;
  border: none;
  padding: 14px;
  border-radius: 14px;
  cursor: pointer;
  font-size: 15px;
  font-weight: 800;
  transition: all 0.2s ease;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(37, 99, 235, 0.24);
}

.danger-btn {
  width: 100%;
  margin-top: 14px;
  background: linear-gradient(135deg, #dc2626, #ef4444);
}

.report-box {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  padding: 17px;
  border-radius: 18px;
  margin-bottom: 13px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.report-box strong {
  color: #0f172a;
}

.report-box span {
  color: #64748b;
}

.history-panel {
  margin-top: 30px;
}

.history-top {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  align-items: start;
}

.search {
  width: 330px;
}

table {
  width: 100%;
  border-collapse: collapse;
  overflow: hidden;
}

th,
td {
  padding: 15px;
  text-align: left;
  border-bottom: 1px solid #e2e8f0;
  vertical-align: top;
}

th {
  color: #334155;
  background: #f8fafc;
  font-size: 14px;
}

tr:hover {
  background: #f8fafc;
}

.student-name {
  font-weight: 800;
  color: #0f172a;
}

.tag {
  display: inline-block;
  padding: 7px 11px;
  border-radius: 999px;
  font-size: 12px;
  font-weight: 800;
}

.blue {
  background: #dbeafe;
  color: #1e40af;
}

.green {
  background: #dcfce7;
  color: #166534;
}

.yellow {
  background: #fef3c7;
  color: #92400e;
}

.red {
  background: #fee2e2;
  color: #991b1b;
}

.delete-btn {
  background: #f1f5f9;
  color: #dc2626;
  box-shadow: none;
  padding: 9px 12px;
}

.delete-btn:hover {
  background: #fee2e2;
  box-shadow: none;
}

.empty-message {
  color: #64748b;
  font-style: italic;
  padding: 20px 0;
}

@media (max-width: 950px) {
  .app {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    height: auto;
    position: relative;
  }

  .sidebar-footer {
    position: static;
    margin-top: 22px;
  }

  .main-content {
    padding: 24px;
  }

  .hero-card,
  .history-top {
    flex-direction: column;
  }

  .stats,
  .content-grid {
    grid-template-columns: 1fr;
  }

  .search {
    width: 100%;
  }

  table {
    font-size: 14px;
  }
}
</style>