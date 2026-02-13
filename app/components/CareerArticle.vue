<template>
  <article
    :style="{ '--article-index': index }"
    class="blur-box article-animate">
    <img :src="career.icon" :alt="'picture of ' + career.name" />
    <div class="content">
      <h3 v-if="career.untouched" class="untouched">/.\ No changes made /.\</h3>
      
      <div class="passives" v-if="career.passives?.length">
        <h2>Passives:</h2>
        <ul class="passives-ul">
          <li v-for="passive in career.passives" :key="passive">{{ passive }}</li>
        </ul>
      </div>

      <div class="talents" v-if="career.talents?.length">
        <h2>Talents:</h2>
        <ul v-for="talent in career.talents" :key="talent" class="talents-ul">
          <li>
            <h4>Level:{{ talent.level }} : {{ talent.name }}</h4>
            <ul class="sub-talent-ul">
              <li v-for="change in talent.changes" :key="change">{{ change }}</li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </article>
</template>

<script setup>
defineProps({
  career: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    required: true
  }
})
</script>

<style scoped>
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

h2 {
  font-family: 'Quintessential', serif;
  letter-spacing: 1px;
  text-decoration: underline;
  font-size: var(--fs-2);
}

h4 {
  color: var(--yellow-400);
  margin-top: 0.7rem;
  font-size: var(--fs-3);
  list-style: none;
}

li {
  overflow-wrap: break-word;
  font-family: 'Lexend Deca', sans-serif;
  font-size: var(--fs-body);
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
}
</style>