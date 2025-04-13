<template>
  <v-app>
    <v-container>
      <h1>Автоформа</h1>
      <FormGenerator :schema="formSchema" v-model="formData" />
      <pre>{{ formData }}</pre> <!-- Для отладки -->
    </v-container>
  </v-app>
</template>

<script setup>
import { ref } from 'vue';
import FormGenerator from './components/FormGenerator.vue';
import formSchema from './formSchema.js';

const formData = ref({
  name: '',
  email: '',
  password: '',
  role: '',
  terms: false,
});
</script>