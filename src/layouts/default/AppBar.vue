<template>
  <v-app-bar>  
    <v-app-bar-title>
      {{ title }}
    </v-app-bar-title>
  </v-app-bar>
</template>

<script setup>
  //here we are costructing a variable
  import { defineProps } from "vue";
  const props = defineProps({
  title: String,
});
</script>

