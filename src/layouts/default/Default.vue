<template>
  <v-app>
    <default-bar :title="title" />
    <default-left />
    <ChessUi @titleUpdate="setTitle" />
  </v-app>
</template>

<script setup>
import DefaultLeft from './DefaultLeft.vue';
import DefaultBar from './AppBar.vue'
import DefaultView from './View.vue'
import { ref } from 'vue'
import ChessUi from '../../components/ChessUi.vue'
const title = ref('loading')
function  setTitle (t)
{
title.value=t
console.log("new title")
}
</script>
