<script setup>
import axios from 'axios'
import { ref, watch } from 'vue'

const isOnline = ref(false)

const clientId = import.meta.env.VITE_CLIENT_ID
const clientSecret = import.meta.env.VITE_CLIENT_SECRET
const broadcasterId = '802802564'
const tokenUrl = 'https://id.twitch.tv/oauth2/token'
const helix = `https://api.twitch.tv/helix/streams?user_id=${broadcasterId}`

const formData = new URLSearchParams()
formData.append('client_id', clientId)
formData.append('client_secret', clientSecret)
formData.append('grant_type', 'client_credentials')

axios.post(tokenUrl, formData, {
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
  },
})
.then(tokenResponse => {
  const accessToken = tokenResponse.data.access_token

  axios.get(helix, {
    headers:{
      'Authorization': `Bearer ${accessToken}`,
      'Client-Id': clientId,
    },
  })
  .then(streamResponse => {
    const streamInfo = streamResponse.data.data[0]
    isOnline.value = !!streamInfo

    console.log('Estado del canal:', isOnline.value)
  })
  .catch(error => {
    console.error('Error al obtener información del canal:', error)
  })
})
.catch(error => {
  console.error('Error al obtener el token de acceso:', error)
})


watch(isOnline, newValue => {
  console.log('El estado del canal ha cambiado:', newValue)
})
</script>

<template>
  <header class="w-full flex justify-center items-center z-8 mt-4 mb-2">
    <div class="flex justify-center items-center flex-col gap-2">
      <div class="flex justify-around items-center gap-10">
        <picture class="flex justify-center items-center flex-col">
          <img class="h-16 md:h-24 rounded-full" src="../assets/img/profile.png" alt="profile_logo" />
          <div>
            <h2 v-if="isOnline" class="bg-green-500 px-4 text-2xl font-mono text-white font-bold rounded-full">EN DIRECTO</h2>
            <h2 v-else class="bg-red-500 px-4 text-2xl font-mono text-white font-bold rounded-full">OFFLINE</h2>
          </div>
        </picture>
          <h1 class="text-white text-5xl md:text-6xl lg:text-8xl">Shunoroi</h1>
      </div>
      <h2 class="text-white text-4xl">Todas mis redes sociales:</h2>
    </div>
  </header>
</template>