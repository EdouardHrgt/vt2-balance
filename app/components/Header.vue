<template>
  <header>
    <img src="/images/logo.webp" alt="logo Vermintide 2" />
  </header>
</template>

<script setup></script>

<style scoped>
header {
  display: flex;
  align-items: center;
  background-color: var(--black-100);
  padding-block: 1rem;
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
