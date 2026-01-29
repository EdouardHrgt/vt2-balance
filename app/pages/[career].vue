<template>
  <main v-if="career">
    <h1>{{ career }} : {{ modName }}</h1>

    <ModPicker @modSelected="handleModSelection" />

    <div class="articles-container">
      <article v-if="modData" v-for="cls in modData.careers" :key="cls" class="blur-box">
        <img :src="cls.icon" :alt="'picture of ' + cls.name" />

        <div class="content">
          <div class="passives" v-if="cls.passives.length >= 1">
            <h2>Passives:</h2>
            <ul v-for="passive in cls.passives" :key="passive" class="passives-ul">
              <li>{{ passive }}</li>
            </ul>
          </div>

          <div class="talents" v-if="cls.talents">
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

    <section class="weapons blur-box">
      <h2>Weapons:</h2>
      <ul v-for="weapon in modData.weapons" :key="weapon.name">
        <h3>{{ weapon.name }} :</h3>
        <li v-for="wpc in weapon.changes">{{ wpc }}</li>
      </ul>
    </section>
  </main>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { ref } from 'vue'
const route = useRoute()
const career = ref(route.params.career)

const modData = ref(null)
const modName = ref('')
const modFiles = {
  1: 'tourney-balance.json',
  2: 'class-balance.json',
  3: 'core-balance.json',
  4: 'linesman-balance.json',
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

    console.log('Datas loaded:', modData.value)
  } catch (error) {
    console.error('Error:', error)
    modData.value = null
  }
}
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
}

@media (max-width: 550px) {
  main {
    padding-inline: 1rem;
  }
}
</style>
