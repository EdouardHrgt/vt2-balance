<template>
  <main v-if="career">
    <h1>{{ career }} : {{ modName }}</h1>

    <ModPicker @modSelected="handleModSelection" />    
    <div class="articles-container">
      <article v-if="modData" v-for="cls in modData.careers" :key="cls" class="blur-box">
        <img :src="cls.icon" :alt="'picture of ' + cls.name" />

        <div class="content">
          <div class="passives" v-if="cls.passives?.length">
            <h2>Passives:</h2>
            <ul v-for="passive in cls.passives" :key="passive" class="passives-ul">
              <li>{{ passive }}</li>
            </ul>
          </div>

          <div class="talents" v-if="cls.talents?.length">
            <h2>Talents:</h2>
            <ul v-for="talent in cls.talents" :key="talent" class="talents-ul">
              <li>
                Level:{{ talent.level }} : {{ talent.name }}
                <p v-for="change in talent.changes">{{ change }}</p>
              </li>
            </ul>
          </div>
        </div>
      </article>
    </div>

    <section class="weapons blur-box" @click="openModal" v-if="modData.weapons?.length">
      <h2>Weapons: <span class="click-hint">(Click to expand)</span></h2>
      <ul v-for="weapon in modData.weapons" :key="weapon.name">
        <h3>{{ weapon.name }} :</h3>
        <li v-for="wpc in weapon.changes">{{ wpc }}</li>
      </ul>
    </section>

    <Transition name="modal">
      <div v-if="isModalOpen" class="modal-overlay" @click="closeModal">
        <div class="modal-content blur-box" @click.stop>
          <button class="close-btn" @click="closeModal">✕</button>
          <h2>Weapons:</h2>
          <ul v-if="modData" v-for="weapon in modData.weapons" :key="weapon.name">
            <h3>{{ weapon.name }} :</h3>
            <li v-for="wpc in weapon.changes">{{ wpc }}</li>
          </ul>
        </div>
      </div>
    </Transition>
  </main>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { ref, onMounted } from 'vue'
const route = useRoute()
const career = ref(route.params.career)

const modData = ref(null)
const modName = ref('Core Balance')
const isModalOpen = ref(false)

const modFiles = {
  1: 'tourney-balance.json',
  2: 'class-balance.json',
  3: 'core-balance.json',
  4: 'linesman-balance.json',
}

const openModal = () => {
  isModalOpen.value = true
  document.body.style.overflow = 'hidden'
}

const closeModal = () => {
  isModalOpen.value = false
  document.body.style.overflow = ''
}

const loadModData = async (fileName) => {
  try {
    const response = await fetch(`/data/${fileName}`)

    if (!response.ok) {
      throw new Error(`Error while loading of : ${fileName}`)
    }

    const jsonData = await response.json()
    modData.value = jsonData[career.value] || null
  } catch (error) {
    console.error('Error:', error)
    modData.value = null
  }
}

const handleModSelection = async (mod) => {
  modName.value = mod.label
  try {
    const fileName = modFiles[mod.id]
    const response = await fetch(`/data/${fileName}`)

    if (!response.ok) {
      throw new Error(`Error while loading of : ${fileName}`)
    }

    const jsonData = await response.json()

    modData.value = jsonData[career.value] || null
  } catch (error) {
    console.error('Error:', error)
    modData.value = null
  }
}

onMounted(() => {
  loadModData('core-balance.json')
})
</script>

<style scoped>
main {
  padding-inline: 2rem;
  display: grid;
  grid-template-columns: 1fr 500px;
  gap: 2rem;
  align-items: start;
}

h1 {
  grid-column: 1 / -1;
  font-size: 32px;
  text-align: center;
  margin-block: 1rem;
  text-transform: uppercase;
  font-family: 'Quintessential', serif;
  letter-spacing: 1px;
}

h2 {
  font-family: 'Quintessential', serif;
  letter-spacing: 1px;
  text-decoration: underline;
  font-size: 26px;
}

main > :nth-child(2) {
  grid-column: 1 / -1;
}

.articles-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

article {
  display: flex;
  align-items: start;
  gap: 2rem;
}

.talents {
  margin-top: 2rem;
}

.weapons {
  position: sticky;
  top: 2rem;
  height: fit-content;
  cursor: pointer;
  transition: transform 0.2s ease;
  margin-bottom: 1rem;
}

.weapons:hover {
  transform: scale(1.01);
}

.click-hint {
  font-size: 14px;
  color: var(--grey-400);
  font-weight: normal;
  text-decoration: none;
}

li {
  list-style: disc;
  margin-left: 1rem;
  font-size: 18px;
  color: var(--grey-400);
}

.talents-ul li,
h3 {
  color: rgb(255, 176, 72);
}

h3 {
  font-size: 18px;
  margin-top: 1.5rem;
}

li p {
  font-size: 15px;
  color: var(--grey-400);
}

.err {
  text-align: center;
  width: 100%;
  color: rgb(91, 255, 99);
}

.weapons li {
  margin-bottom: 0.5rem;
  margin-left: 1.3rem;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 2rem;
}

.modal-content {
  position: relative;
  max-width: 800px;
  max-height: 90vh;
  width: 100%;
  padding: 2rem;
  overflow-y: auto;
}

.close-btn {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 24px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.3s ease;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-active .modal-content,
.modal-leave-active .modal-content {
  transition: transform 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-content {
  transform: scale(0.8) translateY(-50px);
}

.modal-leave-to .modal-content {
  transform: scale(0.8) translateY(50px);
}

@media (max-width: 1130px) {
  article {
    display: block;
  }
  article img {
    margin-inline: auto;
  }
  .content {
    margin-top: 1rem;
  }
}

@media (max-width: 950px) {
  main {
    padding-inline: 2rem;
    display: grid;
    grid-template-columns: 1fr;
    gap: 2rem;
    align-items: start;
  }
}

@media (max-width: 750px) {
  article {
    display: block;
  }
  article img {
    width: 8rem;
  }
  h1 {
    max-width: 400px;
    margin-inline: auto;
  }
  .talents {
    margin-top: 1rem;
  }
  li {
    margin-left: 1.5rem;
  }
  li p {
    margin-left: 0rem;
  }

  .modal-overlay {
    padding: 1rem;
  }

  .modal-content {
    padding: 1.5rem;
  }
}

@media (max-width: 550px) {
  main {
    padding-inline: 1rem;
  }

  .click-hint {
    display: block;
    margin-top: 0.5rem;
  }
}
</style>
