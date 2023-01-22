<script setup lang="ts">
import { ref } from "vue";
import type { ArrScore } from "./types/arrScore";
let numberRandom = ref(Math.floor(Math.random() * 50));
const number = ref(0);
const attempt = ref(0);
let id: number = 1;
let arrScores = ref<ArrScore[]>([]);

function add(e: number): void {
  number.value += e;
  attempt.value += 1;
}
function menos(e: number): void {
  if (number.value <= 0) {
    number.value = 0;
    attempt.value += 1;
  } else {
    number.value -= e;
    attempt.value += 1;
  }
}
function retry(): void {
  numberRandom.value = Math.floor(Math.random() * 50);
  number.value = 0;
  attempt.value = 0;
  id += 1;
  console.log(numberRandom.value);
}
function push(bifrul: number, attempt: number): void {
  arrScores.value.push({ id: bifrul, attempt });
}

const text = ref("Démarrer");
let progressBar = ref(0);
let intervId: number;

function progress(): void {
  if (text.value === "Redémarrer") {
    text.value = "Stop";
    progressBar.value = 0;
  } else if (text.value === "Démarrer") {
    text.value = "Stop";
    intervId = setInterval(addProgress, 100);
  } else if (text.value === "Stop") {
    text.value = "Démarrer";
    clearInterval(intervId);
    intervId = 0;
  }
}

function addProgress(): void {
  if (progressBar.value === 100) {
    text.value = "Redémarrer";
  } else {
    progressBar.value += 1;
  }
}
</script>

<template>
  <section id="ex-1">
    <!-- 1) - Générer une valeur entre 0 et 50 (Math.floor(Math.random()) * 50)
    - Créer un système permettant de trouver le nombre générer à partir de ces 4 boutons
    - Afficher une réponse à chaque clique (Plus grand / Plus petit)
    - Une fois le nombre trouver, afficher "Gagné !" et proposer de recommencer
    - Afficher un tableau de scores qui reprend le nombre de tentatives de chaque partie
  -->
    <div>
      <p>Valeur actuelle: {{ number }}</p>
      <button @click="add(5)">+ 5</button>
      <button @click="menos(5)">- 5</button>
      <button @click="add(1)">+ 1</button>
      <button @click="menos(1)">- 1</button>
      <p v-if="numberRandom > number">Nombre plus grand</p>
      <p v-else-if="numberRandom < number">Nombre plus petit</p>
      <p v-else>
        C'est Gagné! pour recommmencer cliquer sur le bouton ci-dessous
      </p>
      <button type="button" @click="push(id, attempt), retry()">
        Recommencer
      </button>
      <div>
        <ul v-for="arrScore in arrScores" :key="arrScore.id">
          <li>essaie : {{ arrScore.id }}, score : {{ arrScore.attempt }}</li>
        </ul>
      </div>
    </div>
  </section>

  <section id="ex-2">
    <h2>Barre de progression</h2>
    <!-- 2) Créer une barre de progression (les styles sont déjà définis dans la class .progress)
                - Permettre de mettre en pause la barre et changer le texte (Démarrer - Stop)
                - Bonus : Permettre de redémarrer la barre de progression une fois arrivé à 100%
            -->
    <div>
      <div class="progress-bar">
        <div class="percentage">
          <strong>{{ progressBar }}</strong
          >%
        </div>
        <div class="progress" :style="'width:' + progressBar + '%'"></div>
      </div>
      <button @click="progress()">{{ text }}</button>
    </div>
  </section>

  <section id="ex-3">
    <!-- 3) Au clique du bouton, activer une animation qui va changer la class de la div selon les couleurs entrées dans les 2 inputs et ce, toutes les 2 secondes -->
    <div class="colored-div"></div>
    <button>Activer l'effet</button>
    <div>
      <p>Première couleur</p>
      <input type="text" />
    </div>
    <div>
      <p>Deuxième couleur</p>
      <input type="text" />
    </div>
  </section>

  <section id="ex-4">
    <!-- 4)
        - Afficher une liste de tâches à faire  (s'il n'y a pas de tâche, afficher "Pas de tâche")
        - Permettre l'ajout d'une nouvelle tâche via l'input à condution que cette dernière comporte au moins 2 caractères (class .disabled)
        - Afficher, pour chaque nouvelle tâche, la date à laquelle elle a été ajoutée
            let date = new Date();
            date.getDate() + "/" + date.getMonth() +"/"+ date.getFullYear());
        - Permettre la suppression à partir d'un "<button>X</button>"
        de créer une listes de tâches, d'en ajouter et d'en supprimer -->
    <div class="to-do">
      <p>Pas de tâche :D</p>
      <ul>
        <li>
          Une tâche ! - <span class="date">08/10/2019</span> <button>X</button>
        </li>
      </ul>
    </div>
    <form>
      <input type="text" />
      <button class="disabled">Ajouter la tâche</button>
    </form>
  </section>
</template>

<style>
section {
  margin-bottom: 50px;
}

.colored-div {
  width: 100px;
  height: 100px;
  background: black;
}

.date {
  font-size: 12px;
}

.progress-bar {
  background: grey;
  width: 200px;
  height: 20px;
  position: relative;
}

.progress {
  width: 1%;
  height: 20px;
  background: blue;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 9;
}

.disabled {
  pointer-events: none;
  opacity: 0.5;
}

.percentage {
  position: relative;
  z-index: 99;
  display: block;
  text-align: center;
  color: white;
}
</style>
