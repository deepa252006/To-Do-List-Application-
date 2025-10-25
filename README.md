# To-Do-List-Application-<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List Application</title>

    <!-- ✅ Internal CSS Styling -->
    <style>
        /* Full Page Styling */
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #4c8bf5, #6fd3c1);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        /* Main App Container */
        .todo-box {
            background: #fff;
            width: 400px;
            padding: 25px;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
            border-radius: 12px;
        }

        .todo-box h2 {
            text-align: center;
            font-size: 24px;
            margin-bottom: 12px;
            color: #333;
        }

        /* Input + Add Task Button */
        .input-section {
            display: flex;
            gap: 10px;
        }
        #taskInput {
            flex: 1;
            padding: 10px;
            border: 2px solid #4c8bf5;
            border-radius: 6px;
            outline: none;
        }
        #addBtn {
            padding: 10px 15px;
            background: #4c8bf5;
            color: #fff;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }
        #addBtn:hover {
            background: #306acc;
        }

        /* Task List Styles */
        ul {
            list-style: none;
            padding: 0;
            margin-top: 20px;
        }

        ul li {
            background: #f0f0f0;
            padding: 12px;
            margin-bottom: 10px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Completed Task Style */
        ul li.completed span {
            text-decoration: line-through;
            color: #888;
        }

        /* Task Text */
        ul li span {
            flex: 1;
            cursor: pointer;
        }

        /* Buttons */
        .deleteBtn {
            background: #e63946;
            padding: 6px 10px;
            color: #fff;
            border: none;
            border-radius: 6px;
            margin-left: 8px;
            cursor: pointer;
        }
        .deleteBtn:hover {
            background: darkred;
        }
    </style>
</head>

<body>

    <div class="todo-box">
        <h2>✅ To-Do List Application</h2>

        <!-- User Input Section -->
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Enter a task...">
            <button id="addBtn">Add</button>
        </div>

        <!-- Display Task List -->
        <ul id="taskList"></ul>
    </div>

    <!-- ✅ JavaScript Section -->
    <script>

        const taskInput = document.getElementById("taskInput");
        const addBtn = document.getElementById("addBtn");
        const taskList = document.getElementById("taskList");

        // ✅ Load Saved Tasks from LocalStorage
        window.addEventListener("load", loadTasks);

        addBtn.addEventListener("click", addTask);

        function addTask() {
            const taskText = taskInput.value.trim();
            if (!taskText) {
                alert("Please enter a task!");
                return;
            }

            createTaskElement(taskText);
            saveTask(taskText);

            taskInput.value = ""; // Clear input
        }

        // ✅ Create and Display Task UI
        function createTaskElement(taskText, isCompleted = false) {

            const li = document.createElement("li");
            if (isCompleted) li.classList.add("completed");

            li.innerHTML = `
                <span>${taskText}</span>
                <button class="deleteBtn">Delete</button>
            `;

            // ✅ Toggle task completed
            li.querySelector("span").addEventListener("click", () => {
                li.classList.toggle("completed");
                updateLocalStorage();
            });

            // ✅ Delete Task
            li.querySelector(".deleteBtn").addEventListener("click", () => {
                li.remove();
                updateLocalStorage();
            });

            taskList.appendChild(li);
        }

        // ✅ Save tasks to LocalStorage
        function saveTask(taskText) {
            const tasks = getTasksFromStorage();
            tasks.push({ text: taskText, completed: false });
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

        // ✅ Load tasks when page refreshed
        function loadTasks() {
            const tasks = getTasksFromStorage();
            tasks.forEach(task => {
                createTaskElement(task.text, task.completed);
            });
        }

        // ✅ Convert LocalStorage text to JS Array
        function getTasksFromStorage() {
            return JSON.parse(localStorage.getItem("tasks")) || [];
        }

        // ✅ Update LocalStorage after edit/delete
        function updateLocalStorage() {
            const tasks = [];
            document.querySelectorAll("#taskList li").forEach(li => {
                tasks.push({
                    text: li.querySelector("span").textContent,
                    completed: li.classList.contains("completed")
                });
            });
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

    </script>

</body>
</html>
