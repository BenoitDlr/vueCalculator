<script setup>
import { ref, watch } from 'vue';
import CalcKey from './components/CalcKey.vue';

const arrayNumbers = ['7', '8', '9', '4', '5', '6', '1', '2', '3', '0']
const arrayOperators = ['*', '-', '+', '=']

const calc = ref('')
const result = ref(0)

//On surveille chaque saisie de l'utilisateur et on évalue l'opération à chaque appui sur "="
watch(calc, (newCalc, oldCalc) => {
  if (newCalc.includes('=')) {
    result.value = eval(oldCalc)
  }
})

//Fonction permettant d'ajouter chaque saisie à la chaîne de calcul
function addToInput(input) {
  //On repart du précédent résultat si l'opération a été évaluée...
  if (calc.value.includes('=')) {
    calc.value = result.value + input.toString()
  }
  // ...Sinon on ajoute la saisie à la chaîne déjà existante
  else {
    calc.value = calc.value + input.toString()
  }
}

</script>

<template>
  <main>
    <div>{{ calc }}</div>
    <input v-model="result" class="saisie" type="text" disabled />
    <div class="board">
      <div class="gridCalc">
        <CalcKey v-for="n in arrayNumbers" @click="addToInput(n)" :key="n">{{ n }}</CalcKey>
      </div>
      <div class="gridOperator">
        <CalcKey v-for="op in arrayOperators" @click="addToInput(op)" :key="op">{{ op }}</CalcKey>
      </div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.saisie {
  margin: 0 20px 20px 0;
  height: 50px;
  width: 400px;
}

.board {
  display: flex;
  flex-direction: row;
}

.gridCalc {
  width: 300px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.gridOperator {
  width: 100px;
  display: grid;
  grid-template-columns: repeat(1, 1fr);
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
