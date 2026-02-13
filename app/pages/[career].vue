<template>
  <main v-if="career">
    <section class="title-mods" :class="{ 'title-mods-scrolled': isSticky }" ref="stickySection">
      <div class="title-infos" v-if="modName">
        <h1>{{ career }} :</h1>
        <p class="title-mod-name">{{ modName }}</p>
      </div>
      <h1 v-else>
        {{ career }} :
        <span class="sub-h1">(Please select a Mod)</span>
      </h1>
      <ModPicker @modSelected="handleModSelection" :selectedModId="selectedModId" />
    </section>

    <div v-if="modName">
      <p class="err" v-if="errorMsg">... No Changelogs found for this Mod ...</p>

      <!-- WEAPONS -->
      <WeaponsCarousel :weapons="modData.weapons" :mod="modName" v-if="modData.weapons" />
      <!-- CAREERS CHANGES -->
      <div class="articles-container" v-if="errorMsg == false">
        <article
          v-for="(cls, index) in modData.careers"
          :key="cls.name || index"
          :style="{ '--article-index': index }"
          class="blur-box article-animate">
          <img :src="cls.icon" :alt="'picture of ' + cls.name" />
          <div class="content">
            <h3 v-if="cls.untouched" class="untouched">/.\ No changes made /.\</h3>
            <div class="passives" v-if="cls.passives?.length">
              <h2>Passives:</h2>
              <ul class="passives-ul">
                <li v-for="passive in cls.passives" :key="passive">{{ passive }}</li>
              </ul>
            </div>

            <div class="talents" v-if="cls.talents?.length">
              <h2>Talents:</h2>
              <ul v-for="talent in cls.talents" :key="talent" class="talents-ul">
                <li>
                  <h4>Level:{{ talent.level }} : {{ talent.name }}</h4>
                  <ul class="sub-talent-ul">
                    <li v-for="change in talent.changes">{{ change }}</li>
                  </ul>
                </li>
              </ul>
            </div>
          </div>
        </article>
        <button @click="scrollToTop" class="back-to-top">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round">
            <path d="m18 15-6-6-6 6" />
          </svg>
          <span>Scroll to top</span>
        </button>
      </div>
    </div>
  </main>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { ref, onMounted, onUnmounted } from 'vue'

const route = useRoute()
const career = ref(route.params.career)
const modData = ref(null)
const modName = ref('')
const errorMsg = ref(false)
const isSticky = ref(false)
const stickySection = ref(null)
const selectedModId = ref(null)

const modFiles = {
  1: 'tourney-balance.json',
  2: 'class-balance.json',
  3: 'core-balance.json',
  4: 'linesman-balance.json',
}

const handleModSelection = async (mod) => {
  modName.value = mod.label
  selectedModId.value = mod.id
  try {
    const fileName = modFiles[mod.id]
    const response = await fetch(`/data/${fileName}`)

    if (!response.ok) {
      throw new Error(`Error while loading of : ${fileName}`)
      errorMsg.value = true
    }

    const jsonData = await response.json()

    modData.value = jsonData[career.value] || null
    errorMsg.value = false
  } catch (error) {
    console.error('Error:', error)
    modData.value = null
    errorMsg.value = true
  }
}

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => {
      isSticky.value = entry.intersectionRatio < 1
    },
    {
      threshold: [1],
      rootMargin: '-1px 0px 0px 0px',
    },
  )
  if (stickySection.value) {
    observer.observe(stickySection.value)
  }
})

onUnmounted(() => {
  if (stickySection.value) {
    observer.unobserve(stickySection.value)
  }
})

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth',
  })
}
</script>

<style scoped>
.title-mods {
  position: sticky;
  top: 0;
  z-index: 10;
  transition:
    background-color 0.5s ease,
    box-shadow 0.5s ease,
    padding 0.5s ease;
}

.title-mods-scrolled {
  background-color: var(--black-400);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  padding-block: 1rem;
  border-bottom: 1px solid #38362a;
}

.title-infos {
  display: flex;
  align-items: center;
  gap: 1rem;
  justify-content: center;
}

h1,
.title-mod-name {
  font-size: 46px;
  text-align: center;
  margin-block: 1rem;
  text-transform: uppercase;
  font-family: 'Rubik Marker Hatch', system-ui;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 1px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.title-mod-name {
  background: linear-gradient(to right, #fdd90c, #ff5e00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.sub-h1 {
  font-size: 32px;
  color: var(--grey-400);
  margin-left: 1rem;
}

h2 {
  font-family: 'Quintessential', serif;
  letter-spacing: 1px;
  text-decoration: underline;
  font-size: 28px;
}

h4 {
  color: var(--yellow-400);
  margin-top: 0.7rem;
  font-size: 24px;
  list-style: none;
}

/* CAREERS CHANGES */
.articles-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-inline: 2rem;
  margin-top: 5rem;
}

@keyframes slideInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.article-animate {
  animation: slideInUp 1s ease-out forwards;
  animation-delay: calc(var(--article-index) * 0.1s);
  opacity: 0;
}

article {
  display: flex;
  align-items: start;
  gap: 2rem;
  position: relative;
  max-width: 1440px;
  width: 100%;
  margin-inline: auto;
}

li {
  overflow-wrap: break-word;
  font-family: 'Lexend Deca', sans-serif;
  font-size: 20px;
}

.passives li {
  list-style: disc;
  margin-left: 1.35rem;
  color: var(--grey-400);
  margin-bottom: 0.3rem;
}

.talents {
  margin-top: 1.7rem;
}

.sub-talent-ul li {
  color: var(--grey-400);
  list-style: disc;
  margin-left: 1.35rem;
}

@keyframes scaleup {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(1);
  }
}

.err {
  font-family: 'Rubik Marker Hatch', system-ui;
  font-weight: 400;
  font-style: normal;
  text-align: center;
  font-size: 38px;
  background: linear-gradient(to top, rgba(253, 217, 12, 0.3), rgba(255, 94, 0, 0.4));
  margin-inline: auto;
  display: block;
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  padding-block: 5rem;
  transform: translateY(-50%);
  animation: scaleup 0.3s ease-out forwards;
}

.untouched {
  text-transform: uppercase;
  font-family: 'Rubik Marker Hatch', system-ui;
  letter-spacing: 1px;
  display: block;
  position: absolute;
  top: 6rem;
  background: linear-gradient(to left, #fdd90c, #ff5e00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.back-to-top {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 1rem;
  margin-bottom: 18rem;
  margin-inline: auto;
  width: 14rem;
  padding: 1rem 2rem;
  font-size: 18px;
  font-family: 'Lexend Deca', sans-serif;
  font-weight: 600;
  color: var(--black-400);
  background: linear-gradient(to right, #fdd90c, #ff5e00);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(253, 217, 12, 0.3);
}

@media (hover: hover) {
  .back-to-top:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(253, 217, 12, 0.5);
  }
}

.back-to-top:active {
  transform: translateY(-1px);
}

.back-to-top svg {
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px);
  }
}

@media (max-width: 750px) {
  .back-to-top {
    font-size: 16px;
    padding: 0.8rem 1.5rem;
  }
}

@media (max-width: 1130px) {
  .title-mods {
    padding-inline: 2rem;
  }
}

@media (max-width: 950px) {
  h1,
  .title-mod-name {
    font-size: 36px;
  }
}

@media (max-width: 750px) {
  article {
    display: block;
  }
  article img {
    width: 8rem;
    margin-inline: auto;
  }
  .talents {
    margin-top: 1rem;
  }
  .passives li,
  .sub-talent-ul li {
    margin-left: 1.5rem;
  }
  .untouched {
    display: block;
    position: relative;
    top: auto;
    text-align: center;
    margin-top: 1rem;
  }
  .title-mods,
  .articles-container {
    padding-inline: 1.2rem;
  }
  .title-infos {
    display: block;
    margin-bottom: 1rem;
    gap: 0;
  }
  .sub-h1 {
    display: none;
  }
  h1 {
    margin-block: 0;
  }
}

@media (max-width: 550px) {
  h1,
  .title-mod-name {
    font-size: 28px;
    margin: 0;
  }
  .title-infos {
    display: block;
    margin-bottom: 0rem;
  }
  .title-mods-scrolled {
    padding-block: 0.5rem;
  }
}
</style>
