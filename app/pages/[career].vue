<template>
  <main v-if="career">
    <h1>{{ career.name }} : {{ modName }}</h1>

    <ModPicker @modSelected="handleModSelection" />
    <div class="grid">
      <section class="heroes">
        <article v-for="cls in career.classes" :key="cls.name">
          <div class="content">
            <img :src="cls.image" :alt="cls.name" />
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Odio sed dolores corrupti
              cupiditate ullam. Maxime harum minus
            </p>
            <pre v-if="modData">{{ modData }}</pre>
          </div>
        </article>
      </section>
      <section class="weapons"></section>
    </div>
  </main>

  <div v-else>
    <p>Career not found :c</p>
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { careers } from '../../data/careers'
import { ref } from 'vue'

const route = useRoute()

const career = computed(() => careers.find((c) => c.name === route.params.career))

const modData = ref(null)
const modName = ref('')
const modFiles = {
  1: 'tourney-balance.json',
  2: 'class-balance.json',
  3: 'core-balance.json',
  4: 'linesman-balance.json',
}

const handleModSelection = async (mod) => {
  console.log('Mod sélected:', mod.label, mod.id)
  modName.value = mod.label
  try {
    const fileName = modFiles[mod.id]
    const response = await fetch(`/data/${fileName}`)

    if (!response.ok) {
      throw new Error(`Error while loading of : ${fileName}`)
    }

    modData.value = await response.json()

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
}

h1 {
  font-size: 32px;
  text-align: center;
  margin-block: 1rem;
  text-transform: uppercase;
}

.content {
  display: flex;
  align-items: center;
  gap: 2rem;
}

article {
  margin-bottom: 2rem;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  /* box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(5px); */
  border: 1px solid rgba(255, 255, 255, 0.3);
}

@media (max-width: 750px) {
  article {
    padding: 1.5rem 1rem;
  }

  .content {
    flex-direction: column;
    gap: 1rem;
  }

  .content p {
    text-align: center;
  }

  .content img {
    width: 6rem;
  }

  h1 {
    max-width: 400px;
    margin-inline: auto;
  }
}
</style>
