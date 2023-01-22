<script setup lang="ts">
import { ref, computed } from "vue";
const password = ref("");
const valide = ref("");
const alertCara = computed(() => {
  return `Votre mode de passe possède ${password.value.length} caractère`;
});
function passwordValid() {
  if (password.value.length < 10) {
    valide.value = "mot de passe trop court";
    alert(alertCara.value);
  } else {
    valide.value = "mot de passe ok";
    alert(alertCara.value);
  }
  password.value = "";
}

const vegetables = ref([
  { id: 1, name: "salade", price: 1, quantity: 0 },
  { id: 2, name: "tomate", price: 0.1, quantity: 0 },
  { id: 3, name: "carotte", price: 0.2, quantity: 0 },
]);

const total = computed(() => {
  let totalPrice: number = 0;
  vegetables.value.forEach((element) => {
    totalPrice += element.quantity * element.price;
  });
  return totalPrice.toFixed(2);
});
</script>

<template>
  <section id="exercise">
    <!-- 
        - Ecouter la valeur de l'input
        - Afficher "Mot de passe trop court" si le mot de passe est inférieur à 10 caractères, sinon afficher "Mot de passe valide" dans #error.
        - Ecouter la propriété "keydown" et si l'utilisateur appuie sur Enter ou clique sur le bouton, il faut :
                - afficher une alerte avec la longueur du mot de passe ("Votre mot de passe possède X caractères")
                - réinitialiser l'input à la valeur de départ
    -->
    <div>
      <input
        type="password"
        v-model="password"
        @keydown.enter="passwordValid"
      />
      <p id="error">{{ valide }}</p>
      <button type="button" @click="passwordValid">
        Afficher le nombre de caractères
      </button>
    </div>
  </section>

  <section id="exercise2">
    <!-- Créer un système permettant de sélectionner une commande et de l'afficher en temps réel -->
    <ul>
      <li v-for="vegetable in vegetables" :key="vegetable.id">
        <div class="ingredient">
          <h2>{{ vegetable.name }}</h2>
          <p>Prix : {{ vegetable.price }}&euro;</p>
          <div class="action">
            <button @click="vegetable.quantity--">-</button>
            <input type="number" :value="vegetable.quantity" />
            <button @click="vegetable.quantity++">+</button>
          </div>
        </div>
      </li>
    </ul>
    <div>
      <h1>Ma Commande</h1>
      <ul>
        <li v-for="vegetable in vegetables" :key="vegetable.id">
          {{ vegetable.name }} : {{ vegetable.quantity }}
        </li>
      </ul>
      <div></div>
      <p>Total : {{ total }}&euro;</p>
    </div>
  </section>
  <div></div>
</template>

<style scoped>
section {
  margin-bottom: 50px;
}
.ingredient h2 {
  display: block;
  width: 100%;
}
.ingredient .action {
  display: flex;
}
#exercise2 input {
  text-align: center;
}
</style>
