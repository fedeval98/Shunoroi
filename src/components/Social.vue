<script setup>
import { links } from '../datos.js'
function openLinks(url){
  window.open(url);
}
</script>

<template>
    <button v-for="link in links" class="flex justify-center items-center bg-gray-300/90 h-16 w-[95%] md:w-[60%] lg:w-[40%] border-[3px] border-black rounded-2xl btn z-5"
      :data-content="link.nombre" @click="openLinks(link.url)"
    ></button>
</template>

<style scoped>
.btn {
  box-shadow: 0 1em 1em rgba(0, 0, 0, 0.5);
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.btn::before {
  content: '';
  position: absolute;
  width: 1100%;
  height: 3500%;
  background: var(--conic-gradient);
}

.btn::after {
  content: attr(data-content);
  position: absolute;
  background-color:#2F3044;
  width: 96%;
  height: 86%;
  border-radius: 0.4em;
  font-size: 2.5rem;
  color: #fff;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn:hover {
  transform: scale(1.1);
}

.btn:hover::before {
  animation: spin 2s infinite linear;
}

.btn:hover::after{
  background-color: #471bac;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>