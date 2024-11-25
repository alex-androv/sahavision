<template>
  <div class="app">
    <button @click="showModal = true">Открыть</button>
    
    <Modal
      v-model="showModal"
      title="Выберите папку"
      :can-confirm="!!selectedFolderId"
      @ok="handleOk"
    >
      <FolderTree
        :folders="mockFolders"
        :selected-folder-id="selectedFolderId"
        @select="handleSelect"
      />
    </Modal>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Modal from './components/Modal.vue';
import FolderTree from './components/FolderTree.vue';
import { mockFolders } from './mock/folders';

const showModal = ref(false);
const selectedFolderId = ref<number>();

const handleSelect = (id: number) => {
  selectedFolderId.value = id;
};

const handleOk = () => {
  if (selectedFolderId.value) {
    console.log('Selected folder ID:', selectedFolderId.value);
  }
  showModal.value = false;
  selectedFolderId.value = undefined;
};
</script>

<style>
.app {
  padding: 20px;
}
</style>  