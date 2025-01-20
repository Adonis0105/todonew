<template>
    <div class="todo-app">
      <h1>To-Do List</h1>
      <!-- Champ pour ajouter une tâche -->
      <div class="input-section">
        <input
          v-model="newTask"
          type="text"
          placeholder="Ajouter une nouvelle tâche..."
          @keypress.enter="addTask"
        />
        <button @click="addTask">Ajouter</button>
      </div>
      <!-- Liste des tâches -->
      <ul class="task-list">
        <li
          v-for="(task, index) in tasks"
          :key="index"
          :class="{ completed: task.completed }"
        >
          <span>{{ task.text }}</span>
          <div class="task-buttons">
            <button class="complete-btn" @click="toggleComplete(index)">
              {{ task.completed ? "Annuler" : "Terminer" }}
            </button>
            <button class="delete-btn" @click="deleteTask(index)">
              Supprimer
            </button>
          </div>
        </li>
      </ul>
      <!-- Bouton pour vider la liste -->
      <div v-if="tasks.length > 0" class="clear-section">
        <button @click="clearTasks">Vider la liste</button>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, watch } from "vue";
  
  export default {
    name: "TodoList",
    setup() {
      const newTask = ref(""); // Nouvelle tâche à ajouter
      const tasks = ref([]); // Liste des tâches
  
      const loadTasks = () => {
        const savedTasks = JSON.parse(localStorage.getItem("tasks") || "[]");
        tasks.value = savedTasks;
      };
  
      const addTask = () => {
        const trimmedTask = newTask.value.trim();
        if (trimmedTask) {
          tasks.value.push({ text: trimmedTask, completed: false });
          newTask.value = "";
        } else {
          alert("Veuillez saisir une tâche valide !");
        }
      };
  
      const toggleComplete = (index) => {
        tasks.value[index].completed = !tasks.value[index].completed;
      };
  
      const deleteTask = (index) => {
        tasks.value.splice(index, 1);
      };
  
      const clearTasks = () => {
        tasks.value = [];
      };
  
      watch(
        tasks,
        (newTasks) => {
          localStorage.setItem("tasks", JSON.stringify(newTasks));
        },
        { deep: true },
      );
  
      loadTasks();
  
      return {
        newTask,
        tasks,
        addTask,
        toggleComplete,
        deleteTask,
        clearTasks,
      };
    },
  };
  </script>
  
  <style scoped>
  .todo-app {
    font-family: Arial, sans-serif;
    margin: 20px auto;
    max-width: 500px;
    padding: 20px;
    background: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  
  h1 {
    margin-bottom: 20px;
    color: #4a90e2;
  }
  
  .input-section {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  .input-section input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-right: 10px;
    outline: none;
    transition: border-color 0.3s;
  }
  
  .input-section input:focus {
    border-color: #4a90e2;
  }
  
  .input-section button {
    padding: 10px 20px;
    background-color: #4a90e2;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .input-section button:hover {
    background-color: #357ab8;
  }
  
  .task-list {
    list-style: none;
    padding: 0;
  }
  
  .task-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 4px;
    background: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
  }
  
  .task-list li:hover {
    transform: scale(1.02);
  }
  
  .task-list li.completed span {
    text-decoration: line-through;
    color: #999;
  }
  
  .task-buttons {
    display: flex;
    gap: 10px;
  }
  
  .task-list .complete-btn {
    padding: 5px 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .task-list .complete-btn:hover {
    background-color: #3e8c41;
  }
  
  .task-list .delete-btn {
    padding: 5px 10px;
    background-color: #f44336;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .task-list .delete-btn:hover {
    background-color: #d32f2f;
  }
  
  .clear-section {
    margin-top: 20px;
  }
  
  .clear-section button {
    padding: 10px 20px;
    background-color: #ff9800;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .clear-section button:hover {
    background-color: #e68900;
  }
  </style>
  