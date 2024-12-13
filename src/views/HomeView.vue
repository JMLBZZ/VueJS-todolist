<template>
  <div class="home">
    <h1 class="my-5 fw-bold fs-1">Gestionnaire des tâches</h1>
    <p class="my-3 fs-3">Planifiez vos tâches !</p>
    <h3 class="my-3 fw-bold fs-3">Créer une tâche</h3>

    <FormView/>
  </div>
</template>

<script>
// @ is an alias to /src
import FormView from '@/components/FormView.vue'

export default {
  name: 'HomeView',
  components: {
    FormView
  }
}
</script>
