<template>
  <header>
    <NuxtLink to="/">
      <img src="/images/logo.webp" alt="logo Vermintide 2" />
    </NuxtLink>
  </header>
</template>

<script setup></script>

<style scoped>
header {
  display: flex;
  align-items: center;
  padding-top: 1rem;
}

a {
  display: block;
  margin: auto;
}

img {
  width: 20rem;
  margin-inline: auto;
}

@media (max-width: 1024px) {
  img {
    width: 12rem;
  }
}
</style>
