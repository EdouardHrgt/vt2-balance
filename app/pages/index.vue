<script setup>
import { ref, onMounted } from 'vue'

const showPopup = ref(false)

onMounted(() => {
  const hasClosedPopup = localStorage.getItem('hasClosedInfoPopup')

  if (!hasClosedPopup) {
    setTimeout(() => {
      showPopup.value = true
    }, 900)
  }
})

const closePopup = () => {
  showPopup.value = false
  localStorage.setItem('hasClosedInfoPopup', 'true')
}
</script>

<template>
  <main>
    <CareersList />
    <h1>Pick a career to see the <strong class="title-str">Changelogs</strong></h1>
    <Transition name="popup">
      <div v-if="showPopup" class="txt blur-box">
        <p>
          A game is about to start and you've only got a few minutes to figure out the rebalance mod
          being used? This site is here to help.
        </p>
        <p>
          It gathers <strong>the main changes from popular rebalance mods</strong> so you can
          quickly check what really matters. The info may not always be fully up to date. For exact
          values, make sure to check the official sources.
        </p>
        <p>
          The goal is simple: <strong>give players fast access to key info</strong> without digging
          through documents or reading walls of text.
        </p>
        <p>
          I worked on this project alone, and with so much text, there may be a <br />
          <strong>few mistakes</strong>. If you spot anything off, feel free to let me know.
        </p>
        <p>
          I also had to standardize how details are shown across all mods, so specifically for
          weapons with lots of changes, it might feel a bit heavy. Sorry about that 😉
        </p>
        <p>
          It's impossible for me to track every mod change alone, so <br />
          <strong>if you want the site to stay up to date, I'll need some help</strong>.<br />If you
          are interested, contact me (<span>@Bobely</span>) on VT2 Modded Discord…You just need to
          know how to edit a JSON file.
        </p>
        <button @click="closePopup">Don't show me this again</button>
      </div>
    </Transition>
  </main>
</template>

<style scoped>
main {
  position: relative;
}

h1 {
  text-align: center;
  font-size: 32px;
  margin-block: 2rem;
  text-transform: uppercase;
  font-family: 'Rubik Marker Hatch', system-ui;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 1px;
  color: var(--grey-400);
}

.title-str {
  background: linear-gradient(to right, #fdd90c, #ff5e00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.txt {
  margin-left: 2rem;
  width: fit-content;
  padding: 2rem;
  margin-top: 2rem;
}

p {
  font-size: 20px;
  margin-bottom: 1rem;
  max-width: 700px;
  color: var(--grey-400);
  font-weight: 300;
}

strong {
  color: rgb(240, 213, 123);
}

button {
  background: transparent;
  border: none;
  display: block;
  font-size: 20px;
  background-color: var(--black-900);
  text-transform: uppercase;
  padding: 0.3rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  color: var(--grey-400);
  border: 2px solid var(--grey-400);
  transition: all 0.4s ease;
  margin-left: auto;
}

span {
  color: rgb(0, 255, 234);
}

@media (hover: hover) {
  button:hover {
    background-color: var(--grey-400);
    color: var(--red-900);
    border: 2px solid var(--red-900);
    opacity: 1;
  }
}

.popup-enter-active {
  animation: popupIn 0.5s ease-out;
}

.popup-leave-active {
  animation: popupOut 0.3s ease-in;
}

@keyframes popupIn {
  0% {
    opacity: 0;
    transform: translateY(30px) scale(0.95);
  }
  100% {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

@keyframes popupOut {
  0% {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
  100% {
    opacity: 0;
    transform: translateY(20px) scale(0.98);
  }
}

@media (max-width: 1024px) {
  .txt {
    margin-left: 0;
    margin-top: 2rem;
    margin-inline: auto;
    max-width: 90%;
  }
  p {
    max-width: 100%;
    color: var(--white-900);
    text-align: center;
  }
  strong {
    color: var(--grey-400);
  }
  button {
    margin-inline: auto;
  }
}
</style>
