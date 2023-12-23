<template>
  <v-container class="fill-height">



    <div v-if="level === 0">



      <v-btn color="green-accent-4" class="Level-Button ma-16" rounded="xl" size="250" stacked @click="setLevel(1)">
        <span class="Level-Button-Icon">♙</span>
        <h1>Level 1</h1>
      </v-btn>



      <v-btn color="yellow-darken-3" class="Level-Button ma-16" rounded="xl" size="250" stacked @click="setLevel(2)">
        <span class="Level-Button-Icon">♘</span>
        <h1>Level 2</h1>
      </v-btn>

      <v-btn color="red-accent-4" class="Level-Button ma-16" rounded="xl" size="250" stacked @click="setLevel(3)">
        <span class="Level-Button-Icon">♕</span>
        <h1>Level 3</h1>
      </v-btn>
    </div>
    <v-container v-if="level !== 0" class="d-flex flex-column align-center fluid ">


      <h1>level {{ level }}</h1>
      <v-btn @click="setLevel(0)">Back</v-btn>

      <br>

      <v-container class="d-flex  flex-column align-center">
        <v-responsive aspect-ratio="1">

          <v-col class="justify-center">
            <v-row v-for="i in 8">
              <span v-for="j in ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']" class="ma-0 mp-0">

                <Feld :pieces="pieces" :coordinate="j + (9 - i)" @clicked="toggleSelection"
                  :selected="selection.includes(j + (9 - i))"></Feld>

              </span>
            </v-row>
          </v-col>
        </v-responsive>

      </v-container>
    </v-container>
  </v-container>
</template>

<script setup>
import Feld from './Feld.vue';
import { ref, defineEmits } from 'vue'
const level = ref(0)
const emit = defineEmits(['titleUpdate'])
const pieces = ref({

  //White pieces:
  "A1": "R",
  "B1": "N",
  "C1": "B",
  "D1": "Q",
  "E1": "K",
  "F1": "B",
  "G1": "N",
  "H1": "R",
  "A2": "P",
  "B2": "P",
  "C2": "P",
  "D2": "P",
  "E2": "P",
  "F2": "P",
  "G2": "P",
  "H2": "P",

  //black pieces:
  "A8": "r",
  "B8": "n",
  "C8": "b",
  "D8": "q",
  "E8": "k",
  "F8": "b",
  "G8": "n",
  "H8": "r",
  "A7": "p",
  "B7": "p",
  "C7": "p",
  "D7": "p",
  "E7": "p",
  "F7": "p",
  "G7": "p",
  "H7": "p",

})
const selection = ref([])
setLevel(0)
function setLevel(newLevel) {
  if (newLevel == 0) {
    emit('titleUpdate', "Home")
  } else {
    emit('titleUpdate', "Level " + newLevel)
  }
  level.value = newLevel
}
function toggleSelection(coordinate) {
  console.log("Schach" + coordinate)
  if (selection.value.includes(coordinate)) {
    selection.value = selection.value.filter(e => e !== coordinate);
  } else {
    selection.value.push(coordinate)
  }
}


</script>

<style scoped>
.Level-Button h1 {
  font-size: 60px;
  color: black;

}

.Level-Button .Level-Button-Icon {
  font-size: 100px;
  color: black;
}
</style>