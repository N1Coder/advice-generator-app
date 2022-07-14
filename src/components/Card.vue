<script setup>
import { ref, onMounted } from "vue";

const base_URL = "https://api.adviceslip.com/advice";

let msgAdvice = ref(""),
  msgAdviceId = ref("");

// Get advice when component
const AdviceMsg = onMounted(() => {
  fetch(base_URL, { cache: "no-cache" })
    .then((res) => res.json())
    .then((data) => {
      const { id, advice } = data.slip;
      msgAdviceId.value = id;
      msgAdvice.value = advice;
      console.log(data);
    })
    .catch((err) => console.log(err));
});

// method to get a new advice
const getAdviceMsg = () => {
  fetch(base_URL, { cache: "no-cache" })
    .then((res) => res.json())
    .then((data) => {
      const { id, advice } = data.slip;
      msgAdviceId.value = id;
      msgAdvice.value = advice;
      console.log(data);
    })
    .catch((err) => console.log(err));
};
</script>

<template>
  <section class="card">
    <div v-if="msgAdvice !== ''">
      <h1 class="quotes-id">advice #{{ msgAdviceId }}</h1>
      <p class="quotes-advice">"{{ msgAdvice }}"</p>
    </div>

    <h2 class="loading-text" v-else>Loading an advice...</h2>

    <div class="pattern-divider"></div>
    <button class="btn" @click="getAdviceMsg">
      <img src="../../images/icon-dice.svg" alt="Dice icon" />
    </button>
  </section>
</template>

<style lang="scss">
// changing root font size to 10px
:root {
  font-size: 62.5%;
}

// colors map
$colors: (
  "primary": hsl(150, 100%, 66%),
  "main-bg": hsl(217, 19%, 24%),
  "main-card": hsl(217, 19%, 38%),
  "main-text": hsl(193, 38%, 86%),
  "main-icon": hsl(218, 23%, 16%),
);

// function to get color from colors map
@function color($color) {
  @return map-get($map: $colors, $key: $color);
}

// for media query
@mixin mq($size) {
  @media (min-width: $size) {
    @content;
  }
}

// reset default style
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Manrope", sans-serif;
}

body {
  background-color: darken(color("main-bg"), 10%);
}

main {
  min-height: 100vh;
  display: grid;
  place-items: center;
}

section.card {
  position: relative;
  margin: 0 2.5rem;
  background-color: darken(color("main-card"), 10%);
  padding: 4rem 1.5rem;
  text-align: center;
  border-radius: 1rem;

  h1.quotes-id {
    margin: 1rem 0 1.5rem;
    text-transform: uppercase;
    color: color("primary");
    font-size: 1.2rem;
    letter-spacing: 0.5rem;
  }

  h2.loading-text {
    margin: 2rem 0;
    color: color("primary");
  }

  p.quotes-advice {
    padding: 0 1rem;
    margin: 2rem 0 2.5rem;
    font-size: 2.8rem;
    font-weight: bold;
    font-family: sans-serif;
    color: color("main-text");
  }

  .pattern-divider {
    display: block;
    margin: 0 auto;
    margin-bottom: 3.5rem;
    background-image: url("../../images/pattern-divider-mobile.svg");
    background-repeat: no-repeat;
    background-position: center;
    width: 100%;
    padding: 1rem;
  }

  .btn {
    display: grid;
    place-items: center;
    position: absolute;
    left: 50%;
    bottom: -10%;
    transform: translateX(-50%);
    margin: auto;
    outline: none;
    border: none;
    background-color: color("primary");
    width: 6rem;
    aspect-ratio: 1 / 1;
    border-radius: 50%;
    cursor: pointer;
    transition: 200ms ease;

    &:hover,
    &:active {
      box-shadow: 0 0 25px color("primary");
    }
  }
}

@include mq(60rem) {
  section.card {
    max-width: 35%;
    padding: 4rem 3.5rem;

    p.quotes-advice {
      padding: 0 2rem;
    }

    .pattern-divider {
      background-image: url("../../images/pattern-divider-desktop.svg");
    }
  }
}
</style>
