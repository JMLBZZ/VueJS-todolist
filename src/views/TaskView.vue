<template>
    <div class="task-list-container">
        <!-- Barre de recherche (filtre des tâches) -->
        <input
            type="text"
            v-model="filterBar"
            placeholder="Filtrer les tâches..."
            class="filter-bar"
        />
        <!-- Radio bouton -->
        <div class="radio-group">
            <label>
                <input 
                type="radio" 
                name="priorityRadio" 
                value="toutes" 
                v-model="selectedRadio" 
                />
                Toutes
            </label>
            <label>
                <input 
                type="radio" 
                name="priorityRadio" 
                value="court terme" 
                v-model="selectedRadio" 
                />
                Court terme
            </label>
            <label>
                <input 
                type="radio" 
                name="priorityRadio" 
                value="moyen terme" 
                v-model="selectedRadio" 
                />
                Moyen terme
            </label>
            <label>
                <input 
                type="radio" 
                name="priorityRadio" 
                value="long terme" 
                v-model="selectedRadio" 
                />
                Long terme
            </label>
        </div>

        <!-- Liste des tâches -->
        <div v-if="taskFilter.length === 0" class="task-zero">
            Aucune tâche enregistré.
        </div>
        <div v-else>
            <div v-for="task in taskFilter" :key="task.id" :class="['task-card', priorityClass(task.priority)]">
                <h2 class="task-title">{{ task.name }}</h2>
                <p class="task-description">{{ task.description }}</p>
                <p class="task-priority">Échéance : {{ task.priority }}</p>
                <button 
                    class="btn-delete" 
                    @click="deleteTask(task.id, task.name)"
                >
                    Supprimer
                </button>
            </div>
        </div>
        
        <ModalView :taskTitle="modalMessage" v-if="true" />
    </div>
</template>

<script>
import ModalView from "@/components/ModalView.vue";

    export default {
        name: "TaskView",
        components: {
            ModalView,
        },
        data() {
            return {
            tasks: [],
            selectedRadio: "toutes",
            filterBar:"",
            showModal: false,
            modalMessage: "",
            };
        },
        computed: {
            taskFilter() {
                let filtered = this.tasks;

                //Filtre par priorité :
                if (this.selectedRadio !== "toutes") {
                    filtered = filtered.filter(
                        (task) => task.priority === this.selectedRadio
                    );
                }

                //Filtre par titre :
                if (this.filterBar.trim() !== "") {
                    const query = this.filterBar.toLowerCase();
                    filtered = filtered.filter((task) =>
                        task.name.toLowerCase().startsWith(query)
                    );
                }

                return filtered;
            },
        },
        mounted() {
            this.loadTasks();
        },
        methods: {
            loadTasks() {
                const savedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
                this.tasks = savedTasks;
            },
            deleteTask(taskId, taskName) {
                //supprime la tâche
                this.tasks = this.tasks.filter((task) => task.id !== taskId);
                localStorage.setItem("tasks", JSON.stringify(this.tasks));

                //affiche le modal
                this.modalMessage = `La tâche "${taskName}" a été supprimée avec succès !`;

                const modal = new bootstrap.Modal(document.getElementById("addModal"));
                modal.show();
                


                //cache le modal après 3 secondes
                setTimeout(() => {
                    modal.hide();
                }, 3000);
            },
            priorityClass(priority) {
                switch (priority) {
                    case "court terme":
                    return "priority-short";
                    case "moyen terme":
                    return "priority-medium";
                    case "long terme":
                    return "priority-long";
                    default:
                    return "";
                }
            },
        },
    };

</script>

<style scoped>
    .task-list-container {
        max-width: 600px;
        margin: 0 auto;
        padding: 20px;
    }

    /* message aucune tâche */
    .task-zero {
        text-align: center;
        font-size: 18px;
        color: grey;
    }

    /* cadres des tâches */
    .task-card {
        border: 3px solid transparent;
        border-radius: 8px;
        padding: 15px;
        margin-bottom: 15px;
    }

    /* Couleurs des priorités */
    .priority-short {
        border-color: red;
    }
    .priority-medium {
        border-color: orange;
    }
    .priority-long {
        border-color: green;
    }

    /* taches */
    .task-title {
        font-size: 18px;
        font-weight: bold;
        margin-bottom: 10px;
    }
    .task-description {
        font-size: 14px;
        margin-bottom: 10px;
    }
    .task-priority {
        font-size: 14px;
        font-style: italic;
        margin-bottom: 10px;
    }

    /* bouton supprimer */
    .btn-delete {
        padding: 8px 12px;
        background-color: #c80000;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .btn-delete:hover {
        background-color: red;
    }

    /* Filtres radio */
    .radio-group {
        display: flex;
        justify-content: space-around;
        margin-bottom: 50px;
    }
    .radio-group label {
        font-size: 14px;
        cursor: pointer;
    }
    .radio-group input[type="radio"] {
        margin-right: 5px;
    }

    /* Barre de recherche (filtre les tâches)*/
    .filter-bar {
        display: block;
        width: 100%;
        padding: 10px;
        margin-bottom: 50px;
        font-size: 16px;
        border: 1px solid lightgrey;
        border-radius: 5px;
    }
</style>