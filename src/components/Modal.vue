<template>
    <!-- Modal Backdrop -->
    <div class="fixed inset-0 z-50 flex items-center justify-center bg-gray-800 bg-opacity-50">
      <!-- Modal Box -->
      <div class="bg-white w-full max-w-lg p-6 rounded-lg shadow-lg relative">
        <!-- Close button -->
        <button @click="$emit('close')" class="absolute top-4 right-4 text-gray-500 hover:text-gray-700">
          &times;
        </button>
  
        <!-- Slot for modal content -->
        <div>
          <slot></slot>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  // No need for logic here, it's handled by the parent
  </script>
  
  <style scoped>
  /* You can add specific styles for the modal component if needed */
  </style>
  