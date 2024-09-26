<template>
  <div id="app">
    <div class="container">
      <div class="bmi-box" @mouseover="hover = true" @mouseleave="hover = false">
        <h1>Calculadora de IMC</h1>
        <form @submit.prevent="calculateBMI">
          <div>
            <label for="edad">Edad:</label>
            <input type="number" v-model="edad" id="imcinput" required>
          </div>
          <div>
            <label for="peso">Peso (kg):</label>
            <input type="number" v-model="peso" id="imcinput" required>
          </div>
          <div>
            <label for="estatura">Altura (cm):</label>
            <input type="number" v-model="estatura" id="imcinput" required>
          </div>
          <button type="submit">Calcular IMC</button>
        </form>

        <div v-if="bmi" class="result">
          <h2>Tu IMC es: {{ bmi.toFixed(2) }}</h2>
          <p>Estado de salud: {{ bmiCategory }}</p>
          <img v-if="imageUrl" :src="imageUrl" alt="Imagen de estado de salud" class="health-image">
        </div>
      </div>

      <div class="game">
        <h1>Juego Piedra, Papel o Tijera</h1>

        <div class="choices">
          <button @click="play('Piedra')">✊ Piedra</button>
          <button @click="play('Papel')">🖐 Papel</button>
          <button @click="play('Tijera')">✌ Tijera</button>
        </div>

        <div class="result" v-if="gameOver">
          <p>Elección del Jugador: {{ playerChoice }}</p>
          <p>Elección de la Computadora: {{ computerChoice }}</p>
          <p>Resultado: {{ resultMessage }}</p>
        </div>

        <button @click="resetGame" v-if="gameOver">Jugar de nuevo</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const edad = ref(null);
const peso = ref(null);
const estatura = ref(null);
const bmi = ref(null);
const bmiCategory = ref('');
const imageUrl = ref('');
const hover = ref(false);

const playerChoice = ref('');
const computerChoice = ref('');
const resultMessage = ref('');
const gameOver = ref(false);

const calculateBMI = () => {
  const estaturaInMeters = estatura.value / 100;
  bmi.value = peso.value / (estaturaInMeters * estaturaInMeters);
  categorizeBMI();
};

const categorizeBMI = () => {
  if (bmi.value < 16) {
    bmiCategory.value = 'Delgadez Extrema';
    imageUrl.value = 'https://i.pinimg.com/474x/63/62/b1/6362b1c64197fde9faf9f12ab3b015e2.jpg';
  } else if (bmi.value >= 16 && bmi.value < 18.5) {
    bmiCategory.value = 'Delgado';
    imageUrl.value = 'https://thumbs.dreamstime.com/b/hombre-flaco-9403778.jpg'; 
  } else if (bmi.value >= 18.5 && bmi.value < 24.9) {
    bmiCategory.value = 'Peso Normal';
    imageUrl.value = 'https://previews.123rf.com/images/fxquadro/fxquadro1504/fxquadro150400491/38249710-hombre-fuerte-musculoso-culturista-muestra-sus-m%C3%BAsculos-que-contienen-pesas.jpg'; 
  } else if (bmi.value >= 25 && bmi.value < 29.9) {
    bmiCategory.value = 'Sobrepeso';
    imageUrl.value = 'https://i.pinimg.com/236x/ba/64/cd/ba64cd7599e6ea09f8c979d8b9e607d2.jpg'; 
  } else {
    bmiCategory.value = 'Obesidad';
    imageUrl.value = 'https://w7.pngwing.com/pngs/771/145/png-transparent-fat-eating-man-a-fat-man-tshirt-food-fitness-thumbnail.png'; 
  }
};

const play = (playerChoiceInput) => {
  if (gameOver.value) return;

  playerChoice.value = playerChoiceInput;
  computerChoice.value = getComputerChoice();
  resultMessage.value = getResult(playerChoice.value, computerChoice.value);
  gameOver.value = true;
};

const getComputerChoice = () => {
  const choices = ['Piedra', 'Papel', 'Tijera'];
  const randomIndex = Math.floor(Math.random() * choices.length);
  return choices[randomIndex];
};

const getResult = (player, computer) => {
  if (player === computer) {
    return 'Empate';
  } else if (
    (player === 'Piedra' && computer === 'Tijera') ||
    (player === 'Papel' && computer === 'Piedra') ||
    (player === 'Tijera' && computer === 'Papel')
  ) {
    return 'Ganaste!';
  } else {
    return 'Perdiste!';
  }
};

const resetGame = () => {
  playerChoice.value = '';
  computerChoice.value = '';
  resultMessage.value = '';
  gameOver.value = false;
};
</script>

<style scoped>
#app {
  font-family: 'Arial', sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #83e799;
  color: white;
  margin: 0;
  width: 100%;
}

.container {
  display: flex;
  gap: 20px;
}

.bmi-box, .game {
  background-color: rgb(19, 19, 19);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  width: 100%;
  text-align: center;
}

.bmi-box:hover, .game:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 25px rgb(0, 0, 0);
}

.bmi-box form div {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

input {
  width: 80%;
  padding: 10px;
  border: 1px solid #2e2e2e;
  border-radius: 5px;
}

button {
  width: 80%;
  padding: 10px;
  background-color: #0c9b53;
  color: rgb(0, 0, 0);
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0c9b53;
}

.result {
  margin-top: 20px;
  min-height: 120px;
}

h2 {
  margin-top: 20px;
  font-size: 24px;
  color: #ffffff;
}

p {
  font-size: 18px;
  color: #ffffff;
}

.health-image {
  margin-top: 10px;
  width: 150px;
  height: 150px; 
  object-fit: cover; 
  border-radius: 10px;
}
</style>
