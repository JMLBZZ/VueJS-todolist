<template>
  <div class="form-container">
    <form v-on:submit.prevent="createTask">
      <div class="form-group">
        <input
          type="text"
          v-model="taskName"
          placeholder="Nom de la tâche *"
          class="form-control"
          ref="taskNameInput"
          required
        />
      </div>

      <div class="form-group">
        <textarea
          v-model="taskDescription"
          placeholder="Description de la tâche *"
          class="form-control"
          rows="4"
          required
        ></textarea>
      </div>

      <div class="form-group">
        <select v-model="taskPriority" class="form-control" required>
          <option disabled value="">Sélectionnez une priorité *</option>
          <option value="court terme">Court terme</option>
          <option value="moyen terme">Moyen terme</option>
          <option value="long terme">Long terme</option>
        </select>
      </div>

      <button type="submit" class="btn btn-primary" :disabled="!isFormValid">
        Créer
      </button>
    </form>

    <ModalView :taskTitle="taskNameModal" />
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import ModalView from "@/components/ModalView.vue";


export default {
  name: "FormView",
  components: {
    ModalView,
  },
  data() {
    return {
      taskName: "",
      taskDescription: "",
      taskPriority: "",
      taskNameModal:"",
    };
  },
  computed: {
    isFormValid() {
      return this.taskName && this.taskDescription && this.taskPriority;
    },
  },
  methods: {
    createTask() {
      const id = uuidv4();

      const newTask = {
        id,
        name: this.taskName,
        description: this.taskDescription,
        priority: this.taskPriority,
      };

      //enregistrement local storage :
      const existingTasks = JSON.parse(localStorage.getItem("tasks")) || [];
      existingTasks.push(newTask);
      localStorage.setItem("tasks", JSON.stringify(existingTasks));

      console.log("Task created:", newTask);
      this.taskNameModal = this.taskName;

      this.showModal();
      this.resetForm();
    },
    // voir le modal
    /*
    showModal() {
      const modal = new bootstrap.Modal(document.getElementById("addModal"));
      modal.show();
    },
    */
    showModal() {
      const modalElement = document.getElementById("addModal");
      if (modalElement) {
        const modal = new bootstrap.Modal(modalElement, {
          backdrop: false,
        });
        modal.show();
      } else {
        console.error("L'élément du modal est introuvable.");
      }
    },

    //reset du formulaire
    resetForm() {
      this.taskName = "";
      this.taskDescription = "";
      this.taskPriority = "";

      //focus
      if (this.$refs.taskNameInput) {
        this.$refs.taskNameInput.focus();
      }
    },
  },
};

</script>

<style scoped>
  .form-container {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
  }
  .form-group {
    margin-bottom: 15px;
  }
  .form-control {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 14px;
  }
  textarea.form-control {
    resize: none;
  }
  .btn {
    width: 100%;
    padding: 10px;
    background-color: blue;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
  }
  .btn:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
</style>
