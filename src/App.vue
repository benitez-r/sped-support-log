<template>
  <div class="app">
    <aside class="sidebar">
      <h2>SpEd Support Log</h2>

      <nav>
        <a href="#">Dashboard</a>
        <a href="#">Log Interaction</a>
        <a href="#">History</a>
        <a href="#">Reports</a>
      </nav>
    </aside>

    <main class="main-content">
      <header>
        <h1>Special Education Support Dashboard</h1>
        <p>Track and manage student support interactions.</p>
      </header>

      <section class="stats">
        <div class="card">
          <h3>Total Logs</h3>
          <p>128</p>
        </div>

        <div class="card">
          <h3>Students Assisted</h3>
          <p>34</p>
        </div>

        <div class="card">
          <h3>Reports Generated</h3>
          <p>12</p>
        </div>
      </section>

      <section class="content-grid">
        <div class="panel">
          <h2>Log New Interaction</h2>

          <form>
            <input type="text" placeholder="Student Name" />

            <select>
              <option>Support Type</option>
              <option>Academic</option>
              <option>Behavioral</option>
              <option>Transition</option>
            </select>

            <textarea placeholder="Interaction Notes"></textarea>

            <button type="button">Submit Log</button>
          </form>
        </div>

        <div class="panel">
          <h2>Recent Interactions</h2>

          <table>
            <thead>
              <tr>
                <th>Student</th>
                <th>Type</th>
                <th>Date</th>
              </tr>
            </thead>

            <tbody>
              <tr>
                <td>John</td>
                <td>Academic</td>
                <td>5/8/26</td>
              </tr>

              <tr>
                <td>Maria</td>
                <td>Behavioral</td>
                <td>5/8/26</td>
              </tr>

              <tr>
                <td>Alex</td>
                <td>Transition</td>
                <td>5/8/26</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  background: #f4f7fb;
}

.app {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  width: 240px;
  background: #1e3a5f;
  color: white;
  padding: 30px 20px;
}

.sidebar h2 {
  margin-bottom: 40px;
}

.sidebar nav {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.sidebar a {
  color: white;
  text-decoration: none;
  font-size: 18px;
}

.main-content {
  flex: 1;
  padding: 40px;
}

header h1 {
  font-size: 36px;
  color: #1e3a5f;
}

header p {
  margin-top: 10px;
  color: #555;
}

.stats {
  display: flex;
  gap: 20px;
  margin-top: 30px;
}

.card {
  background: white;
  padding: 25px;
  border-radius: 15px;
  flex: 1;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.card h3 {
  color: #555;
  margin-bottom: 10px;
}

.card p {
  font-size: 32px;
  color: #1e3a5f;
  font-weight: bold;
}

.content-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-top: 40px;
}

.panel {
  background: white;
  padding: 25px;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.panel h2 {
  margin-bottom: 20px;
  color: #1e3a5f;
}

form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

input,
select,
textarea {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 10px;
}

textarea {
  height: 120px;
}

button {
  background: #2563eb;
  color: white;
  border: none;
  padding: 14px;
  border-radius: 10px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background: #1d4ed8;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  color: #1e3a5f;
}
</style>