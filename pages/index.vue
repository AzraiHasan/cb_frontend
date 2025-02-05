<!-- pages/index.vue -->
<!-- pages/index.vue -->
<template>
 <div class="min-h-screen bg-gray-50">
  <div class="max-w-4xl mx-auto py-20 px-4">
   <div class="text-center mb-12">
    <h1 class="text-4xl font-bold text-gray-900 mb-4">
     Banking as a Service Platform
    </h1>
    <p class="text-lg text-gray-600">
     Select your portal to continue
    </p>
   </div>

   <div class="grid md:grid-cols-2 gap-6 max-w-2xl mx-auto">
    <UCard class="hover:shadow-lg transition-shadow">
     <template #header>
      <h2 class="text-xl font-semibold">Bank Administration</h2>
     </template>

     <p class="text-gray-600 mb-4">
      Manage customers, monitor transactions, and access reports
     </p>

     <template #footer>
      <UButton to="/admin" color="primary" block size="lg">
       Access Admin Portal
      </UButton>
     </template>
    </UCard>

    <UCard class="hover:shadow-lg transition-shadow">
     <template #header>
      <h2 class="text-xl font-semibold">Banking Portal</h2>
     </template>

     <p class="text-gray-600 mb-4">
      View accounts, make transactions, and manage your banking needs
     </p>

     <template #footer>
      <UButton to="/banking" color="primary" block size="lg">
       Access Banking Portal
      </UButton>
     </template>
    </UCard>
   </div>
  </div>
 </div>
</template>

<script setup lang="ts">
// Future authentication checks can be added here
</script>