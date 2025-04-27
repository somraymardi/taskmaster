# taskmaster
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Master</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100">
    <div class="max-w-md mx-auto mt-12 p-6 rounded-lg shadow-md bg-white">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-bold">Task Master</h2>
            <span class="text-gray-600">Manage your tasks efficiently</span>
        </div>
        <ul class="todo-list" id="todo-list"></ul>
        <div class="add-todo mt-6 flex justify-between items-center">
            <input type="text" id="todo-input" placeholder="Add new task" class="w-full py-2 pl-10 text-sm text-gray-800 border border-gray-400 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-600 bg-gray-50">
            <button type="button" id="add-todo-btn" class="ml-4 px-4 py-2 bg-green-500 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-green-700 hover:shadow-lg focus:bg-green-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-green-800 active:shadow-lg transition duration-150 ease-in-out">Add Task</button>
        </div>
        <div class="mt-6 text-center text-gray-600">
            Built on <a href="https://cerebrascoder.com" target="_blank" class="text-gray-900 underline">Cerebras Coder</a>
        </div>
    </div>

    <script>
        // Get the todo list and add todo button elements
        const todoList = document.getElementById('todo-list');
        const addTodoBtn = document.getElementById('add-todo-btn');
        const todoInput = document.getElementById('todo-input');

        // Load todos from local storage
        let todos = JSON.parse(localStorage.getItem('todos')) || [];

        // Function to render the todo list
        function renderTodoList() {
            todoList.innerHTML = '';
            todos.forEach((todo, index) => {
                const todoItem = document.createElement('li');
                todoItem.innerHTML = `
                    <div class="flex justify-between items-center py-4 border-b border-gray-300">
                        <div class="flex items-center">
                            <input type="checkbox" id="todo-${index}" class="mr-4" ${todo.completed ? 'checked' : ''}>
                            <span class="todo-text ${todo.completed ? 'text-gray-400 line-through' : 'text-gray-600'}">${todo.text}</span>
                        </div>
                        <button type="button" class="px-4 py-2 bg-red-500 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-red-700 hover:shadow-lg focus:bg-red-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-red-800 active:shadow-lg transition duration-150 ease-in-out delete-todo" data-index="${index}">Delete</button>
                    </div>
                `;
                todoList.appendChild(todoItem);
            });
        }

        // Render the initial todo list
        renderTodoList();

        // Add event listener to the add todo button
        addTodoBtn.addEventListener('click', () => {
            const todoText = todoInput.value.trim();
            if (todoText) {
                todos.push({ text: todoText, completed: false });
                localStorage.setItem('todos', JSON.stringify(todos));
                todoInput.value = '';
                renderTodoList();
            }
        });

        // Add event listener to the todo list
        todoList.addEventListener('change', (e) => {
            if (e.target.type === 'checkbox') {
                const index = parseInt(e.target.id.split('-')[1]);
                todos[index].completed = e.target.checked;
                localStorage.setItem('todos', JSON.stringify(todos));
                renderTodoList();
            }
        });

        // Add event listener to the delete todo buttons
        todoList.addEventListener('click', (e) => {
            if (e.target.classList.contains('delete-todo')) {
                const index = parseInt(e.target.dataset.index);
                todos.splice(index, 1);
                localStorage.setItem('todos', JSON.stringify(todos));
                renderTodoList();
            }
        });
    </script>
</body>
</html>
