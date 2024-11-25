<template>
    <Teleport to="body">
      <div v-if="modelValue" class="modal-overlay" @click="close">
        <div class="modal-content" @click.stop>
          <header class="modal-header">
            <h2>{{ title }}</h2>
          </header>
          
          <div class="modal-body">
            <slot></slot>
          </div>
          
          <footer class="modal-footer">
            <button @click="$emit('ok')" :disabled="!canConfirm">Ок</button>
            <button @click="close">Закрыть</button>
          </footer>
        </div>
      </div>
    </Teleport>
  </template>
  
  <script lang="ts" setup>
  import { defineProps, defineEmits } from 'vue';
  
  const props = defineProps<{
    modelValue: boolean;
    title: string;
    canConfirm?: boolean;
  }>();
  
  const emit = defineEmits<{
    (e: 'update:modelValue', value: boolean): void;
    (e: 'ok'): void;
  }>();
  
  const close = () => {
    emit('update:modelValue', false);
  };
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 4px;
    min-width: 300px;
  }
  
  .modal-header {
    margin-bottom: 20px;
  }
  
  .modal-footer {
    margin-top: 20px;
    display: flex;
    justify-content: flex-end;
    gap: 10px;
  }
  
  button {
    padding: 8px 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
    background: white;
    cursor: pointer;
  }
  
  button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  </style>
  