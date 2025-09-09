<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Todo List</title>
    <!-- Fun Google Font -->
    <link href="https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap" rel="stylesheet">
    <style>
        /* Fun and colorful styling for a student-made to-do list */
        body {
            font-family: 'Indie Flower', cursive;
            background-color: #fffef7;
            margin: 0;
            padding: 20px;
            background-image: 
                radial-gradient(circle at 20% 80%, rgba(173, 216, 230, 0.2) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(255, 192, 203, 0.2) 0%, transparent 50%);
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            background: #fff;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
            border: 3px dashed #ffa07a;
        }

        h1 {
            text-align: center;
            color: #ff7f50;
            margin-bottom: 30px;
            font-size: 2.8em;
            font-weight: bold;
            letter-spacing: 1px;
        }

        /* Input section */
        .input-section {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        #taskInput {
            flex: 1;
            padding: 12px;
            font-size: 18px;
            border: 2px solid #ffa07a;
            border-radius: 8px;
            outline: none;
            background: #fffaf0;
            min-width: 200px;
            color: #333;
        }

        #taskInput:focus {
            border-color: #ff6347;
            box-shadow: 0 0 6px rgba(255, 99, 71, 0.4);
        }

        #addBtn {
            padding: 12px 20px;
            background-color: #ff6347;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
        }

        #addBtn:hover {
            background-color: #e5533c;
            transform: translateY(-1px) scale(1.03);
        }

        /* Todo list */
        #todoList {
            list-style: none;
            padding: 0;
        }

        .todo-item {
            background: #fff8dc;
            margin: 10px 0;
            padding: 15px;
            border-radius: 10px;
            border-left: 5px solid #ffa07a;
            display: flex;
            align-items: center;
            gap: 10px;
            transition: all 0.3s ease;
        }

        .todo-item:hover {
            background: #fff0d9;
            transform: translateX(5px);
        }

        .todo-checkbox {
            width: 18px;
            height: 18px;
            cursor: pointer;
        }

        .todo-text {
            flex: 1;
            font-size: 18px;
            color: #444;
            transition: all 0.3s ease;
        }

        .todo-text.completed {
            text-decoration: line-through;
            color: #aaa;
            opacity: 0.7;
        }

        /* Delete button with emoji for fun */
        .delete-btn {
            background: none;
            border: none;
            font-size: 22px;
            cursor: pointer;
            padding: 5px;
            border-radius: 5px;
            transition: background-color 0.3s;
        }

        .delete-btn:hover {
            background-color: #ffe4e1;
            transform: scale(1.2);
        }

        /* Empty state */
        .empty-state {
            text-align: center;
            color: #aaa;
            font-style: italic;
            padding: 40px;
            background: #fafafa;
            border-radius: 10px;
            border: 2px dashed #ccc;
            margin-top: 20px;
        }

        /* Responsive */
        @media (max-width: 600px) {
            .container {
                margin: 10px;
                padding: 20px;
            }

            .input-section {
                flex-direction: column;
            }

            #taskInput {
                min-width: auto;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🛠️ My Todo List</h1>
        
        <!-- Input section for adding new tasks -->
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="What do you need to do?" maxlength="100">
            <button id="addBtn">Add Task</button>
        </div>
        
        <!-- List where all todos will be displayed -->
        <ul id="todoList"></ul>
        
        <!-- Empty state message -->
        <div id="emptyState" class="empty-state">
            No tasks yet! Add one above to get started.
        </div>
    </div>

    <script>
        // Get references to DOM elements
        const taskInput = document.getElementById('taskInput');
        const addBtn = document.getElementById('addBtn');
        const todoList = document.getElementById('todoList');
        const emptyState = document.getElementById('emptyState');
        
        // Array to store todos
        let todos = [];
        let nextId = 1;

        // Event listeners
        addBtn.addEventListener('click', addTask);
        taskInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addTask();
            }
        });

        // Add a new task
        function addTask() {
            const taskText = taskInput.value.trim();
            if (taskText === '') {
                alert('Please enter a task!');
                return;
            }

            const newTodo = {
                id: nextId++,
                text: taskText,
                completed: false
            };

            todos.push(newTodo);
            taskInput.value = '';
            renderTodos();
            taskInput.focus();
        }

        // Toggle task completion
        function toggleTask(id) {
            const todo = todos.find(t => t.id === id);
            if (todo) {
                todo.completed = !todo.completed;
                renderTodos();
            }
        }

        // Delete a task
        function deleteTask(id) {
            todos = todos.filter(t => t.id !== id);
            renderTodos();
        }

        // Render tasks
        function renderTodos() {
            todoList.innerHTML = '';

            if (todos.length === 0) {
                emptyState.style.display = 'block';
                return;
            } else {
                emptyState.style.display = 'none';
            }

            todos.forEach(todo => {
                const li = document.createElement('li');
                li.className = 'todo-item';

                li.innerHTML = `
                    <input type="checkbox" class="todo-checkbox" ${todo.completed ? 'checked' : ''} 
                           onchange="toggleTask(${todo.id})">
                    <span class="todo-text ${todo.completed ? 'completed' : ''}">${escapeHtml(todo.text)}</span>
                    <button class="delete-btn" onclick="deleteTask(${todo.id})" title="Delete task">🗑️</button>
                `;

                todoList.appendChild(li);
            });
        }

        // Escape HTML for safety
        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        // Initialize
        renderTodos();
        taskInput.focus();
    </script>
</body>
</html>
