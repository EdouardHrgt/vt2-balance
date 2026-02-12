<script setup>
defineProps({
  weapons: Array,
})
</script>

<template>
  <section class="carousel" v-if="weapons">
    <button class="carousel-btn carousel-btn-left"><p><</p></button>
    <div class="weapons">
      <article class="weapon" v-for="weapon in weapons" :key="weapon.name">
        <div class="weapon-header">
          <h2 class="weapon-h2">{{ weapon.name }}</h2>
          <!-- <img :src="weapon.icon" :alt="weapon.name" /> -->
        </div>
        <ul class="weapon-list">
          <li v-for="change in weapon.changes" :key="change">{{ change }}</li>
        </ul>
      </article>
    </div>
    <button class="carousel-btn carousel-btn-right"><p>></p></button>
  </section>
</template>

<style scoped>
.carousel {
  position: relative;
  margin-inline: auto;
  margin-block: 2rem;
  width: 100%;
  max-width: 1980px;
}

.weapons {
  display: flex;
  align-items: stretch;
  gap: 2rem;
  background-color: var(--black-100);
  width: 100%;
  overflow-x: auto;
  padding: 2rem;
  scroll-snap-type: x mandatory;
  scroll-behavior: smooth;
  /* scrollbar-width: none; */
  -ms-overflow-style: none;
}

.weapons::-webkit-scrollbar {
  /* display: none; */
}

.weapon {
  width: 80%;
  display: flex;
  flex-direction: column;
  flex-shrink: 0;
  overflow: hidden;
  scroll-snap-align: center;
  padding-inline: 1.5rem;
  padding-block: 1rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.weapon-list {
  flex-grow: 1;
}

.weapon-list li {
  list-style-type: disc;
  margin-left: 1.5rem;
  line-height: 30px;
  overflow-wrap: break-word;
  font-family: 'Lexend Deca', sans-serif;
  font-size: 18px;
}

.weapon-list li:nth-child(odd) {
  color: var(--grey-400);
}

.weapon-list li:nth-child(even) {
  color: var(--yellow-400);
}

.weapon-header h2 {
  margin-bottom: 1rem;
  font-family: 'Quintessential', serif;
  letter-spacing: 1px;
  text-decoration: underline;
  font-size: 28px;
  text-transform: uppercase;
}

.carousel-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: var(--white-100);
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 10;
  transition: background-color 0.3s;
}

.carousel-btn p {
  color: var(--white-900);
  font-size: 30px;
  text-transform: uppercase;
}

.carousel-btn:hover {
  background-color: rgba(0, 0, 0, 0.8);
}

.carousel-btn-left {
  left: -25px;
}

.carousel-btn-right {
  right: -25px;
}
</style>
