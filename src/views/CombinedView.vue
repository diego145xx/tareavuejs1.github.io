<template>
    <div class="container mt-5">
        <h1>Lista de Tareas</h1>

        <!-- Sección para agregar tareas manualmente -->
        <div class="mb-4">
            <h3>Agregar Tarea</h3>
            <div class="input-group">
                <input v-model="nuevaTarea" type="text" class="form-control" placeholder="Ingrese una nueva tarea" />
                <button @click="agregarTarea" class="btn btn-primary">Agregar</button>
            </div>
        </div>

        <!-- Sección para extraer tareas de la API -->
        <div class="mb-4">
            <h3>Tareas desde la API</h3>
            <button @click="obtenerTareas" class="btn btn-info">Obtener Tareas</button>
            <ul class="list-group mt-3">
                <li v-for="tarea in tareasAPI" :key="tarea.id" class="list-group-item">
                    {{ tarea.nombre }}
                </li>
            </ul>
        </div>

        <!-- Sección combinada: Mostrar todas las tareas -->
        <h4>Tareas Combinadas</h4>
        <ul class="list-group">
            <!-- Mostrar tareas de la API y las manuales -->
            <li v-for="tarea in tareasCombinadas" :key="tarea.id" class="list-group-item">
                {{ tarea.nombre }}
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            nuevaTarea: "",        // Tarea ingresada manualmente
            tareasAPI: [],         // Tareas obtenidas de la API
            tareasManuales: []     // Tareas agregadas manualmente
        };
    },
    computed: {
        // Computar las tareas combinadas, tanto de la API como las manuales
        tareasCombinadas() {
            return [...this.tareasAPI, ...this.tareasManuales];
        }
    },
    methods: {
        // Función para agregar tarea manualmente
        agregarTarea() {
            if (this.nuevaTarea.trim() === "") return; // Evitar agregar tareas vacías

            // Agregar tarea a la lista manual
            this.tareasManuales.push({
                id: Date.now(),  // Usar timestamp como ID único
                nombre: this.nuevaTarea,
                completada: false
            });

            this.nuevaTarea = "";  // Limpiar el campo de entrada
        },

        // Función para obtener tareas desde la API
        async obtenerTareas() {
            try {
                const response = await axios.get('https://dummyjson.com/todos');
                this.tareasAPI = response.data.todos;
            } catch (error) {
                console.error("Error al obtener tareas desde la API:", error);
            }
        }
    }
};
</script>

<style scoped>
/* Aquí pueden agregar estilos personalizados para el componente */
.container {
    max-width: 800px;
    margin: 0 auto;
}

.input-group {
    max-width: 500px;
    margin: 0 auto;
}

.list-group-item {
    padding: 10px 15px;
}

button {
    margin-top: 10px;
}

h3, h4 {
    margin-bottom: 15px;
}
</style>