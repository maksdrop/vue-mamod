<template>
  <div id="app">
    <TheForm  
      v-if="!registration"
      :model-value="registration"
      @updateRegistration="updateRegistration"
    />
    <div v-else :class="[registration ? 'show' : '', 'registry__success']">Регистрация успешно завершена!</div>
  </div>
</template>

<script>
import TheForm from './components/TheForm.vue'

export default {
  name: 'App',
  components: {
    TheForm
  },
  data() {
    return {
      registration: false
    }
  },
  methods: {
    updateRegistration(bool) {
      this.registration = bool;
    }
  }
}
</script>

<style>
.registry__success {
  background: linear-gradient(45deg, #4CAF50, #2196F3);
  color: white;
  text-align: center;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  transform: scale(0.9);
  opacity: 0;
  transition: transform 0.3s, opacity 0.3s;
}

.registry__success.show {
  transform: scale(1);
  opacity: 1;
}
</style>
