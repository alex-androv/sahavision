<template>
    <div class="folder-tree">
      <div
        v-for="folder in folders"
        :key="folder.id"
        class="folder-item"
      >
        <div 
          class="folder-header"
          :class="{ 'selected': selectedFolderId === folder.id }"
          @click="selectFolder(folder.id)"
        >
          <span 
            v-if="folder.children.length"
            class="folder-toggle"
            @click.stop="toggleFolder(folder.id)"
          >
            {{ isExpanded(folder.id) ? '▼' : '►' }}
          </span>
          <span class="folder-name">{{ folder.name }}</span>
        </div>
        
        <div 
          v-if="folder.children.length && isExpanded(folder.id)"
          class="folder-children"
        >
          <FolderTree
            :folders="folder.children"
            :selected-folder-id="selectedFolderId"
            @select="$emit('select', $event)"
          />
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts" setup>
  import { ref } from 'vue';
  
  const props = defineProps<{
    folders: FolderNode[];
    selectedFolderId?: number;
  }>();
  
  const emit = defineEmits<{
    (e: 'select', id: number): void;
  }>();
  
  const expandedFolders = ref<Set<number>>(new Set());
  
  const isExpanded = (id: number): boolean => {
    return expandedFolders.value.has(id);
  };
  
  const toggleFolder = (id: number) => {
    if (expandedFolders.value.has(id)) {
      expandedFolders.value.delete(id);
    } else {
      expandedFolders.value.add(id);
    }
  };
  
  const selectFolder = (id: number) => {
    emit('select', id);
  };
  </script>
  
  <style scoped>
  .folder-tree {
    padding-left: 20px;
  }
  
  .folder-item {
    margin: 5px 0;
  }
  
  .folder-header {
    display: flex;
    align-items: center;
    cursor: pointer;
    padding: 4px;
  }
  
  .folder-header:hover {
    background-color: #f0f0f0;
  }
  
  .folder-header.selected {
    background-color: #e0e0e0;
  }
  
  .folder-toggle {
    margin-right: 5px;
    font-size: 12px;
    width: 20px;
    display: inline-block;
    cursor: pointer;
  }
  
  .folder-name {
    user-select: none;
  }
  
  .folder-children {
    margin-left: 20px;
  }
  </style>