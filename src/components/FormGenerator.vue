<template>
    <v-form @submit.prevent="submitForm">
      <v-row v-for="(field, index) in schema.fields" :key="index">
        <v-col cols="12">
          <v-text-field
            v-if="field.type === 'text' || field.type === 'email' || field.type === 'password'"
            v-model="localFormData[field.model]"
            :label="field.label"
            :type="field.type"
            :rules="getRules(field)"
          />
          <v-select
            v-if="field.type === 'select'"
            v-model="localFormData[field.model]"
            :label="field.label"
            :items="field.options"
            :rules="getRules(field)"
          />
          <v-checkbox
            v-if="field.type === 'checkbox'"
            v-model="localFormData[field.model]"
            :label="field.label"
            :rules="getRules(field)"
          />
        </v-col>
      </v-row>
      <v-btn type="submit">Отправить</v-btn>
    </v-form>
  </template>
  
  <script setup>
  import { defineProps, defineEmits, ref, watch } from 'vue';
  
  const props = defineProps({
    schema: { type: Object, required: true },
    modelValue: { type: Object, required: true },
  });
  
  const emit = defineEmits(['update:modelValue']);
  
  const localFormData = ref({ ...props.modelValue });
  
  watch(
    () => props.modelValue,
    (newVal) => {
      localFormData.value = { ...newVal };
    }
  );
  
  const getRules = (field) => {
    const rules = [];
    if (field.required) rules.push((v) => !!v || `${field.label} обязателен`);
    if (field.minLength) rules.push((v) => (v && v.length >= field.minLength) || `Минимум ${field.minLength} символов`);
    if (field.type === 'email') rules.push((v) => /.+@.+\..+/.test(v) || 'Неверный email');
    return rules;
  };
  
  const submitForm = () => {
    emit('update:modelValue', { ...localFormData.value });
    console.log('Form submitted:', localFormData.value);
  };
  </script>