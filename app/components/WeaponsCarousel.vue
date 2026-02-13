<script setup>
import { ref } from 'vue'

defineProps({
  weapons: Array,
  mod: String,
})

const carouselRef = ref(null)
const selectedWeapon = ref(null)
const isModalOpen = ref(false)

const scrollToNext = () => {
  if (carouselRef.value) {
    const scrollAmount = 832
    carouselRef.value.scrollBy({ left: scrollAmount, behavior: 'smooth' })
  }
}

const scrollToPrev = () => {
  if (carouselRef.value) {
    const scrollAmount = 832
    carouselRef.value.scrollBy({ left: -scrollAmount, behavior: 'smooth' })
  }
}

const openModal = (weapon) => {
  selectedWeapon.value = weapon
  isModalOpen.value = true
  document.body.style.overflow = 'hidden'
}

const closeModal = () => {
  isModalOpen.value = false
  selectedWeapon.value = null
  document.body.style.overflow = ''
}
</script>

<template>
  <section class="carousel" v-if="weapons.length > 1">
    <div class="weapons" ref="carouselRef">
      <article class="weapon" v-for="weapon in weapons" :key="weapon.name">
        <div class="weapon-header">
          <img :src="weapon.icon" :alt="weapon.name" v-if="weapon.icon" />
          <h2 class="weapon-h2">{{ weapon.name }}</h2>
        </div>
        <button class="see-more-btn" @click="openModal(weapon)">See Details</button>
      </article>
    </div>
    <section class="btns">
      <button class="carousel-btn carousel-btn-left" @click="scrollToPrev">
        <p>&lt;</p>
      </button>
      <button class="carousel-btn carousel-btn-right" @click="scrollToNext">
        <p>&gt;</p>
      </button>
    </section>

    <!-- Modal -->
    <Teleport to="body">
      <Transition name="modal">
        <div v-if="isModalOpen" class="modal-overlay" @click="closeModal">
          <div class="modal-content" @click.stop>
            <button class="modal-close" @click="closeModal">
              <span>&times;</span>
            </button>
            <div v-if="selectedWeapon" class="modal-body">
              <div class="modal-header">
                <h2 class="modal-title">{{ selectedWeapon.name }}</h2>
                <strong>[ {{ mod }} ]</strong>
              </div>
              <ul class="modal-list">
                <li v-for="change in selectedWeapon.changes" :key="change">
                  {{ change }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </Transition>
    </Teleport>
  </section>
</template>

<style scoped>
.carousel {
  position: relative;
  margin-inline: auto;
  margin-block: 2rem;
  max-width: 1440px;
  border: 2px solid black;
  border-radius: 8px;
}

.weapons {
  display: flex;
  align-items: stretch;
  gap: 2rem;
  background-color: var(--black-100);
  width: 100%;
  overflow-x: auto;
  padding: 2rem;
  scroll-snap-type: x mandatory;
  scroll-behavior: smooth;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.weapons::-webkit-scrollbar {
  display: none;
}

.weapon {
  width: 400px;
  display: flex;
  flex-direction: column;
  flex-shrink: 0;
  overflow: hidden;
  scroll-snap-align: center;
  padding-inline: 1.5rem;
  padding-block: 1rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.weapon-header img {
  display: block;
  margin-inline: auto;
  aspect-ratio: 1/1;
  width: 85px;
}

.weapon-header h2 {
  font-size: var(--fs-2);
  text-align: center;
  text-transform: uppercase;
  font-family: 'Rubik Marker Hatch', system-ui;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 1px;
  background: linear-gradient(to right, #fdd90c, #ff5e00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.5rem;
}

.see-more-btn {
  margin-top: auto;
  padding: 0.75rem 1.5rem;
  background-color: var(--grey-400);
  color: var(--black-900);
  border: none;
  border-radius: 6px;
  font-family: 'Lexend Deca', sans-serif;
  font-size: var(--fs-4);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 1px;
  width: 14rem;
  margin-inline: auto;
}

@media (hover: hover) {
  .see-more-btn:hover {
    background-color: var(--black-100);
    color: var(--white-900);
  }
}

.btns {
  position: absolute;
  bottom: -35px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 5rem;
}

.carousel-btn {
  background-color: var(--black-900);
  border: 2px solid #e7c9a9;
  border-radius: 8px;
  cursor: pointer;
  z-index: 10;
  transition: background-color 0.3s;
  position: relative;
  display: grid;
  place-content: center;
}

.carousel-btn p {
  color: var(--white-900);
  font-size: 28px;
  padding: 0.2rem 2rem;
}

@media (hover: hover) {
  .carousel-btn:hover {
    background-color: var(--white-900);
    border: 2px solid var(--black-900);
  }
  .carousel-btn:hover.carousel-btn p {
    color: var(--black-900);
  }
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.85);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 2rem;
}

.modal-content {
  position: relative;
  background: linear-gradient(135deg, rgba(0, 0, 0, 0.95) 0%, rgba(20, 20, 20, 0.95) 100%);
  border: 2px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  max-width: 800px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  padding: 3rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 1;
}

.modal-close span {
  color: var(--white-100);
  font-size: 32px;
  line-height: 1;
}

@media (hover: hover) {
  .modal-close:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: rotate(90deg);
  }
}

.modal-body {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.modal-header {
  border-bottom: 2px solid var(--yellow-400);
  padding-bottom: 1rem;
  display: flex;
  align-items: baseline;
  gap: 1rem;
  justify-content: space-between;
}

.modal-header strong {
  font-size: var(--fs-body);
  text-transform: uppercase;
  font-family: 'Rubik Marker Hatch', system-ui;
  font-weight: 400;
  font-style: normal;
  letter-spacing: 1px;
  background: linear-gradient(to right, #fdd90c, #ff5e00);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.modal-title {
  font-family: 'Quintessential', serif;
  font-size: var(--fs-1);
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--white-900);
  margin: 0;
}

.modal-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.modal-list li {
  position: relative;
  padding-left: 2rem;
  margin-bottom: 1.5rem;
  line-height: 1.8;
  font-family: 'Lexend Deca', sans-serif;
  font-size: var(--fs-body);
  overflow-wrap: break-word;
}

.modal-list li::before {
  content: '▸';
  position: absolute;
  left: 0;
  font-size: 20px;
}

.modal-list li:nth-child(odd) {
  color: var(--grey-400);
}

.modal-list li:nth-child(odd)::before {
  color: var(--grey-400);
}

.modal-list li:nth-child(even) {
  color: var(--yellow-400);
}

.modal-list li:nth-child(even)::before {
  color: var(--yellow-400);
}

/* Modal Transitions */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-active .modal-content,
.modal-leave-active .modal-content {
  transition: transform 0.3s ease;
}

.modal-enter-from .modal-content,
.modal-leave-to .modal-content {
  transform: scale(0.9);
}

/* Scrollbar 'modal' */
.modal-content::-webkit-scrollbar {
  width: 8px;
}

.modal-content::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 4px;
}

.modal-content::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 4px;
}

@media (hover: hover) {
  .modal-content::-webkit-scrollbar-thumb:hover {
    background: rgba(255, 255, 255, 0.3);
  }
}

/* Responsive */
@media (max-width: 1510px) {
  .carousel {
    margin-inline: 2rem;
  }
}

@media (max-width: 1024px) {
  .weapon {
    padding-inline: 1rem;
    padding-block: 0.5rem;
  }

  .modal-header {
    display: block;
  }
}

@media (max-width: 768px) {
  .weapon-header h2 {
    font-size: var(--fs-body);
  }
  .modal-content {
    padding: 2rem 1.5rem;
    margin: 1rem;
  }

  .carousel {
    margin-inline: 0rem;
    border: none;
  }

  .weapon {
    width: 45%;
    min-width: 200px;
    padding-inline: 0.75rem;
    padding-block: 1rem;
  }

  .see-more-btn {
    width: 11rem;
  }

  .btns {
    display: none;
  }

  .modal-close {
    top: 0.5rem;
    right: 0.5rem;
    width: 25px;
    height: 25px;
  }
  .modal-close span {
    font-size: 20px;
  }
  .weapon-header img {
    width: 75px;
  }
}

@media (max-width: 450px) {
  .modal-content {
    padding: 2rem 0.5rem;
  }
  .modal-title,
  .modal-header strong {
    font-size: 15px;
  }
  .modal-list li {
    padding-left: 1.3rem;
    font-size: 14px;
  }
}
</style>
