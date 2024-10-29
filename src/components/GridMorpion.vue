<template lang="">
  <h1>Le jeu du morpion</h1>
  <div class="Jeu">

    <div  v-for="(cell, index) in tableauCell" :key="index">
        <button @click="retourneId(index)" :key="index">
          <p>{{ cell }}</p>
        </button>
    </div>
  </div>
  <div class="StatutJeu">
    <div v-if="error" class="error">Case déjà coché</div>
    <div v-if="joueur1">Joueur 1 Gagne</div>
    <div v-if="joueur2">Joueur 2 Gagne</div>
    <div v-if="isFinish">Partie terminée</div>
  </div>

</template>
<script setup>

import { ref } from 'vue';

let maxTour = 10
let tour = 1
const joueur1 = ref(false)
const joueur2 = ref(false)
const isFinish = ref(false)
const message = ref('')
const error = ref(false)
const tableauCell = ref(Array(9).fill(''));
const winPattern = ref([ [0,1,2],  [3,4,5],  [6,7,8],  [0,3,6],  [1,4,7],  [2,5,8],  [0,4,8],  [2,4,6],])
const partie = ref(true)

const retourneId = (index) => {
console.log('l`index : ', index);
if(!partie.value) {
  return
} else {
  if (tour < maxTour) {
  error.value = false
  if (tableauCell.value[index] === '') { 
    console.log('tour numéro :' , tour);
    tableauCell.value[index] = tour % 2 === 0 ? 'X' : 'O';
    const winner = checkWinner();
    if (winner) {
      isFinish.value = !isFinish.value
      partie.value = !partie.value
    } else {
      tour++;
    }
    } else {
      error.value = !error.value
      message.value = 'Cette case est déjà prise'
    }
  } else {
    console.log('Partie Terminée');
    tableauCell.value = Array(9).fill('');
    tour = 0; 
  }
}

}

const checkWinner = () => {
for (let [a, b, c] of winPattern.value) {
  if (tableauCell.value[a] && tableauCell.value[a] === tableauCell.value[b] && tableauCell.value[a] === tableauCell.value[c]) {
    if(tableauCell.value[a]) {
      joueur1.value = !joueur1.value
    } else {
      joueur2.value = !joueur2.value
    }
    return tableauCell.value[a];
  }
}
return null;
}
</script>
<style scoped>

.Jeu {
display: flex;
flex-wrap: wrap;
background-color: SteelBlue;
margin: 0 auto;
padding: 10px;
width: 350px;
}

.Jeu div {
margin: 0 auto;
}

.Jeu button {
width: 100px;
height: 100px;
font-weight: bold;
font-size: 50px;
margin: 5px;
float:left;
display: flex;
justify-content: center;
align-items: center;
}

.StatutJeu {
color: black;
background: #eee;
padding: 10px 0;
text-align: center;
margin: 0 auto;
font-size: 20px;
width: 350px;
padding: 8px;
  border: 2px solid steelblue;
}

.error {
color: red;
}
</style>