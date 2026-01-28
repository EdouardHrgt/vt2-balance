<template>
  <main v-if="career">
    <h1>{{ career.name }}</h1>

    <div class="grid">
      <section class="heroes">
        <article v-for="cls in career.classes" :key="cls.name">
          <div class="content">
            <img :src="cls.image" :alt="cls.name" />
            <p>
              Lorem ipsum dolor sit, amet consectetur adipisicing elit. Odio sed dolores corrupti
              cupiditate ullam. Maxime harum minus, iure, ratione quae rerum possimus at numquam
              recusandae, perspiciatis nobis neque? Sapiente, asperiores similique. Magnam eveniet
              corporis molestiae.
            </p>
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

const route = useRoute()

const career = computed(() => careers.find((c) => c.name === route.params.career))
</script>

<style scoped>
main {
  padding-inline: 2rem;
}

h1 {
  font-size: 48px;
  text-align: center;
  padding-block: 2rem;
  text-transform: uppercase;
}

.heroes {
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
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}
</style>
