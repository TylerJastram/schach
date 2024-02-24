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
      <v-btn @click="setLevel(0); humanColor = undefined">Back</v-btn>
      <br>
      <div v-if="humanColor === undefined">
        <v-btn @click="humanColor = 'white'" class="mr-5" size="x-large">White</v-btn>
        <v-btn @click="humanColor = 'black'" color="black" size="x-large">Black</v-btn>
      </div>
      <v-container v-if="humanColor" class="d-flex  flex-column align-center">
        <v-responsive aspect-ratio="1">

          <v-col class="justify-center">
            <v-row v-for="i in 8">
              <span v-for="j in ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']" class="ma-0 mp-0">

                <Feld :pieces="configuration.pieces" :coordinate="j + (9 - i)" @clicked="toggleSelection"
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
// here we are importing the chess engine or somtheing
import { moves, move, aiMove, Game } from 'js-chess-engine'
import Feld from './Feld.vue';
import { ref, defineEmits } from 'vue'
const level = ref(0)
const emit = defineEmits(['titleUpdate'])
const configuration = ref({ pieces: {} })
const game = new Game()
configuration.value = game.exportJson()

const selection = ref([])
const activeSquare = ref(undefined)
const humanColor = ref(undefined)
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


  if (selection.value.includes(coordinate)) {
    if (coordinate === activeSquare.value) {
      selection.value = []
      activeSquare.value = undefined
    }
    else {
      configuration.value.turn = humanColor.value
      const newBoard = move(configuration.value, activeSquare.value, coordinate)
      configuration.value = newBoard
      selection.value = []
      activeSquare.value = undefined
      pcMove()
    }
  } else {
    selection.value = []
    const currentPiece = configuration.value.pieces[coordinate]
    if (currentPiece === undefined) return
    const pieceColor = (currentPiece === currentPiece.toUpperCase()) ? "white" : "black"
    if (humanColor.value !== pieceColor) return
    activeSquare.value = coordinate
    selection.value.push(coordinate)
    const possibleMoves = getPossibleMoves(coordinate)
    Array.prototype.push.apply(selection.value, possibleMoves);
  }
}
function getPossibleMoves(coordinate) {
  //find piece and find out whos turn it is
  const piece = configuration.value.pieces[coordinate]

  if (piece === undefined)
    return []
  const color = piece === piece.toUpperCase() ? "white" : "black"
  configuration.value.turn = color
  const m = moves(configuration)

  return m[coordinate] ? m[coordinate] : []
}
function pcMove() {
  const aiColor = humanColor.value === "white" ? "black" : "white"
  configuration.value.turn = aiColor
  const m = aiMove(configuration, level.value)

  const keys = Object.keys(m)
  for (let i = 0; i < keys.length; i++) {

    const from = keys[i]

    const to = m[from]
    const newBoard = move(configuration, from, to)
    configuration.value = newBoard
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