<template>
  <section>
    <div v-for="mod in mods" :key="mod.id" @click="selectMod(mod)">
      <p>{{ mod.label }}</p>
    </div>
  </section>
</template>

<script setup>
const emit = defineEmits(['modSelected'])

const mods = {
  tb: { label: 'tourney balance', id: 1 },
  clb: { label: 'class balance', id: 2 },
  cB: { label: 'core balance', id: 3 },
  lb: { label: 'linesman balance', id: 4 },
}

const selectMod = (mod) => {
  emit('modSelected', { label: mod.label, id: mod.id })
}
</script>

<style scoped>
section {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.25rem;
  margin-bottom: 1.25rem;
}

div {
  padding: 0.5rem 1rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  cursor: pointer;
  transition: background 0.3s ease;
}

p {
  text-transform: capitalize;
}

@media (hover: hover) {
  div:hover {
    background: rgba(255, 255, 255, 0.3);
  }
}

@media (max-width: 1024px) {
  p {
    text-align: center;
  }

  section {
    flex-wrap: wrap;
    gap: 1rem;
  }
}
</style>
