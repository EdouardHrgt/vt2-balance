<template>
  <div :style="{ backgroundImage: `url(${backgroundImage})` }" class="page">
    <Header />
    <NuxtPage />
    <DocumentationLinks />
  </div>
</template>

<script setup>
const backgroundImage = ref('')

onMounted(() => {
  const images = [
    // '/images/shared/vt2-background-1.webp',
    // '/images/shared/vt2-background-2.webp',
    '/images/shared/vt2-background-3.webp',
    '/images/shared/vt2-background-4.webp',
  ]

  const randomIndex = Math.floor(Math.random() * images.length)
  backgroundImage.value = images[randomIndex]
})
</script>

<style scoped>
.page {
  width: 100%;
  min-height: 100vh;
  background-color: black;
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  padding-bottom: 1rem;
  position: relative;
}
</style>
