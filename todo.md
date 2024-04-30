---
layout: default
title: Task Manager
permalink: /todo
---
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Super Cool Task Manager</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 800px;
            margin: 50px auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
        }

        input[type="text"],
        input[type="date"],
        input[type="time"] {
            width: calc(100% - 100px);
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        button {
            width: 100px;
            padding: 10px;
            background-color: #4caf50;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            padding: 10px;
            background-color: #f9f9f9;
            border-radius: 5px;
            margin-bottom: 10px;
            position: relative;
        }

        li:last-child {
            margin-bottom: 0;
        }

        .btn-container {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
        }

        .btn-container button {
            margin-left: 5px;
        }

        .completed {
            text-decoration: line-through;
            color: #888;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>Super Cool Task Manager</h1>
        <input type="text" id="taskInput" placeholder="Enter task...">
        <input type="date" id="taskDate"> <!-- New input for date -->
        <input type="time" id="taskTime"> <!-- New input for time -->
        <button onclick="addTask()">Add Task</button>
        <ul id="taskList"></ul>
    </div>
    <script>
        function addTask() {
            var description = document.getElementById("taskInput").value;
            var date = document.getElementById("taskDate").value; // Retrieve selected date
            var time = document.getElementById("taskTime").value; // Retrieve selected time
            fetch('http://127.0.0.1:8086/api/todo/add', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ description: description, date: date, time: time }) // Pass date and time
            })
            .then(response => response.json())
            .then(data => {
                alert(data.message);
                fetchTaskList();
            });
        }
        function fetchTaskList() {
            fetch('http://127.0.0.1:8086/api/todo/list')
            .then(response => response.json())
            .then(taskList => {
                var taskListHtml = '';
                taskList.forEach(task => {
                    taskListHtml += `<li${task.completed ? ' class="completed"' : ''}>${task.description}
                        <span style="margin-left: 10px;">Date: ${task.date}</span> <!-- Display date -->
                        <span style="margin-left: 10px;">Time: ${task.time}</span> <!-- Display time -->
                        <div class="btn-container">
                            <button onclick="completeTask('${task.description}')">Complete</button>
                            <button onclick="deleteTask('${task.description}')">Delete</button>
                        </div>
                    </li>`;
                });
                document.getElementById("taskList").innerHTML = taskListHtml;
            });
        }
        function completeTask(description) {
            fetch('http://127.0.0.1:8086/api/todo/complete/' + encodeURIComponent(description), {
                method: 'PUT'
            })
            .then(response => response.json())
            .then(data => {
                alert(data.message);
                fetchTaskList();
            });
        }
        function deleteTask(description) {
            fetch('http://127.0.0.1:8086/api/todo/delete/' + encodeURIComponent(description), {
                method: 'DELETE'
            })
            .then(response => response.json())
            .then(data => {
                alert(data.message);
                fetchTaskList();
            });
        }
        fetchTaskList(); 
    </script>
</body>

</html>
