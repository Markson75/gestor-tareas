<template>
    <div>
        <h1>Lista de Tareas</h1>
        fetchTasks()
        <div>
            <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.todo }}</h5>
            <span>{{ task.completed ? 'Completada' : 'Pendiente' }}</span>
            <button @click="toggleTaskCompletion(task)">
                {{ task.completed ? 'Desmarcar' : 'Completar' }}
            </button>
            <button @click="deleteTask(task)">Eliminar</button>
        </div>
        <!-- Esta seccion es una combinación de las dos vistas anteriores -->
    </div>
</template>

<script>
import axios from "axios";
   // Esta sección debe permitir agregar tareas nuevas a la vez que extraer las tareas anteriores de la API

export default {
    name: 'CombinedView',
    data() {
        return {
            newTask: "", // Campo de entrada para la nueva tarea
            tasks: [],   // Lista de tareas locales
        };
    },
    methods: {
        addTask() {
            if (this.newTask.trim() === "") return;

            const newTask = {
                todo: this.newTask,
                completed: false,
                id: Date.now(), 
            };

            // Añadir la nueva tarea al inicio de la lista
            this.tasks.unshift(newTask);
            this.newTask = ""; // Limpiar el campo de entrada después de agregar
        },
        fetchTasks() {
            axios.get("https://dummyjson.com/todos")
            .then(response => {
                this.tasks = response.data.todos
            })
            .catch(error => {
                console.log(error)
            })
        },
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },
    }


}
</script>

<style scoped>
/* Aquí pueden agregar estilos personalizados para el componente. */
</style>