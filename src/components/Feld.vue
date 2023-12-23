<template>
  <v-sheet fill-height fluid class="d-flex ma-0 pa-0" :color="themeColor" width="50" height="50" block>
    <v-row justify="center">
      <v-col>
        <div :class="{ highlight: selected }" class="pa-0" @click="fieldClicked">
          <v-img :src="pieceCharacter" ></v-img>
        </div>
      </v-col>
    </v-row>



  </v-sheet>
</template>

<script setup>
import { computed, ref } from '@vue/reactivity';
import { watch } from 'vue';
import { onMounted } from 'vue';


const props = defineProps({
  pieces: Object,
  coordinate: String,
  selected: Boolean
})
const emit = defineEmits(['clicked'])

const themeColor = ref("red")
const pieceCharacter = ref("a")

watch(props.pieces, () => updateField())
onMounted(() => {
  updateField()
  const row = Number(props.coordinate.substring(1, 2))
  const col = (props.coordinate.charCodeAt(0)) - 64

  if ((row + col) % 2 === 1) themeColor.value = "#E9EDCC"; else themeColor.value = "#779954"

})
function fieldClicked() {
  console.log(props.coordinate)
  emit("clicked", props.coordinate)
}
function updateField() {
  if (Object.keys(props.pieces).indexOf(props.coordinate) > -1) {
    pieceCharacter.value = getPiece(props.pieces[props.coordinate])
  } else { pieceCharacter.value = getPiece("blank") }
}
function getPiece(piece) {

  //white pieces:
  if (piece === 'R') return "./whitepieces/R.png"
  if (piece === 'N') return "./whitepieces/N.png"
  if (piece === 'B') return "./whitepieces/B.png"
  if (piece === 'Q') return "./whitepieces/Q.png"
  if (piece === 'K') return "./whitepieces/K.png"
  if (piece === 'P') return "./whitepieces/P.png"

  //black pieces:
  if (piece === 'p') return "./blackpieces/p.png"
  if (piece === 'k') return "./blackpieces/k.png"
  if (piece === 'q') return "./blackpieces/q.png"
  if (piece === 'b') return "./blackpieces/b.png"
  if (piece === 'n') return "./blackpieces/n.png"
  if (piece === 'r') return "./blackpieces/r.png"


  return "./blank.png"
}
</script>

<style scoped>
.piece {
  font-size: 40px;
  text-align: center;

}

.coordinate {
  position: absolute;
}

.highlight {
  background-color: rgba(255, 0, 0, 0.26);
}
</style>