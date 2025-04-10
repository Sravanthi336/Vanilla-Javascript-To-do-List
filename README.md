# Vanilla-Javascript-To-do-List
HTML code
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>To-Do List App</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h1>📝 To-Do List</h1>
    <div class="todo-input">
      <input type="text" id="taskInput" placeholder="Enter a new task..." />
      <button id="addTaskBtn">Add Task</button>
    </div>
    <ul id="taskList"></ul>
  </div>

  <script src="script.js"></script>
</body>
</html>

CSS code
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background: linear-gradient(to right, #4facfe, #00f2fe);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 50px 20px;
}

.container {
  background-color: white;
  padding: 30px 20px;
  border-radius: 10px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.todo-input {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

#taskInput {
  flex: 1;
  padding: 10px;
  font-size: 16px;
}

#addTaskBtn {
  padding: 10px 15px;
  background-color: #4facfe;
  border: none;
  color: white;
  font-weight: bold;
  cursor: pointer;
  border-radius: 5px;
}

#addTaskBtn:hover {
  background-color: #00c3ff;
}

ul#taskList {
  list-style: none;
}

li.task {
  display: flex;
  justify-content: space-between;
  background-color: #f3f3f3;
  padding: 12px;
  margin-bottom: 10px;
  border-radius: 5px;
  align-items: center;
}

li.task.completed {
  text-decoration: line-through;
  opacity: 0.6;
}

.task button {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}


Javascript code
const taskInput = document.getElementById("taskInput");
const addTaskBtn = document.getElementById("addTaskBtn");
const taskList = document.getElementById("taskList");

// Load tasks from local storage
window.onload = () => {
  const tasks = JSON.parse(localStorage.getItem("tasks")) || [];
  tasks.forEach(task => addTask(task.text, task.completed));
};

addTaskBtn.addEventListener("click", () => {
  const taskText = taskInput.value.trim();
  if (taskText) {
    addTask(taskText);
    saveTask(taskText);
    taskInput.value = "";
  }
});

function addTask(text, completed = false) {
  const li = document.createElement("li");
  li.classList.add("task");
  if (completed) li.classList.add("completed");

  li.innerHTML = `
    <span>${text}</span>
    <div>
      <button onclick="toggleComplete(this)">✔️</button>
      <button onclick="deleteTask(this)">❌</button>
    </div>
  `;

  taskList.appendChild(li);
}

function toggleComplete(button) {
  const task = button.closest("li");
  task.classList.toggle("completed");
  updateLocalStorage();
}

function deleteTask(button) {
  const task = button.closest("li");
  task.remove();
  updateLocalStorage();
}

function saveTask(text) {
  const tasks = JSON.parse(localStorage.getItem("tasks")) || [];
  tasks.push({ text, completed: false });
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function updateLocalStorage() {
  const tasks = [];
  document.querySelectorAll("li.task").forEach(task => {
    tasks.push({
      text: task.querySelector("span").innerText,
      completed: task.classList.contains("completed")
    });
  });
  localStorage.setItem("tasks", JSON.stringify(tasks));
}
