<template>
    <div>
        <h1>Lista de Tareas</h1>
        <button @click="fetchTasks">Cargar Tareas</button>
        <div v-if="tasks.length > 0">
            <div v-for="task in tasks" :key="task.id">
                <div>
                    <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.todo }}</h5>
                    <span>{{ task.completed ? 'Completada' : 'Pendiente' }}</span>
                    <button @click="toggleTaskCompletion(task)">
                        {{ task.completed ? 'Desmarcar' : 'Completar' }}
                    </button>
                    <button @click="deleteTask(task)">Eliminar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "TaskList",
    data() {
        return {
            tasks: [], // Almacenamiento local de las tareas traídas de la API
        };
    },
    methods: {
        // Llamada para obtener las tareas desde la API externa
        async fetchTasks() {
            try {
                const response = await axios.get('https://dummyjson.com/todos'); // API para obtener tareas
                this.tasks = response.data.todos; // Asignamos las tareas a la variable local 'tasks'
            } catch (error) {
                console.error('Error al obtener las tareas:', error);
            }
        },

        // Cambiar el estado de una tarea (completada/no completada)
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },

        // Eliminar la tarea seleccionada
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
    },
};
</script>

<style scoped>
/* Aquí pueden experimentar con estilos de tu preferencia *//* Estilo general para el contenedor principal */
.container {
    max-width: 800px;
    margin: 0 auto;
}

/* Estilo para el botón de "Cargar Tareas" */
button {
    margin-top: 10px;
}

/* Estilo para cada ítem de tarea */
.task-item {
    margin-bottom: 20px;
    padding: 15px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

/* Estilo para el título de la tarea */
.task-item h5 {
    margin: 0;
    font-size: 18px;
    color: #333;
}

/* Estilo para el estado de la tarea (completada o pendiente) */
.task-item span {
    margin-left: 10px;
    font-style: italic;
    color: #888;
}

/* Alineación de los botones dentro de cada tarea */
.ml-2 {
    margin-left: 10px;
}

/* Estilos adicionales para los botones */
button {
    font-weight: bold;
    padding: 5px 10px;
    border-radius: 5px;
}

/* Botón de "Completar/Desmarcar" */
.btn-success {
    background-color: #28a745;
    border: none;
}

/* Botón de "Eliminar" */
.btn-danger {
    background-color: #dc3545;
    border: none;
}

/* Estilos cuando no hay tareas disponibles */
p {
    font-size: 16px;
    color: #888;
    text-align: center;
}</style>