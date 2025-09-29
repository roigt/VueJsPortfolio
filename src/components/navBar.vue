<script setup>
import { ref } from "vue"
import { GoogleGenerativeAI } from '@google/generative-ai'

const menuItems = ref([
  { name: "Accueil", href: "#" },
  { name: "Projets", href: "#projets" },
  { name: "Compétences", href: "#skills" },
  { name: "Contact", href: "#contact" },
])



// ia
const API_KEY = 'AIzaSyDdOEK_BnS_sXzW26PoYSMJ3diSYOt8RIc'

const userMessage = ref('')
const messages = ref([])

const genAI = new GoogleGenerativeAI(API_KEY)
const model = await genAI.getGenerativeModel({ model: 'gemini-2.0-flash-001' })
const chat = model.startChat({ history: [] })

const sendMessage = async () => {
  if (!userMessage.value.trim()) return

  // Affiche le message utilisateur
  const content = userMessage.value
  messages.value.push({ role: 'user', content })
  userMessage.value = ''

  try {
    const result = await chat.sendMessage(content)
    const response = await result.response
    const aiReply = response.text()

    // Affiche la réponse IA
    messages.value.push({ role: 'ai', content: aiReply })
  } catch (error) {
    messages.value.push({ role: 'ai', content: "❌ Erreur de réponse de l'IA." })
    console.error(error)
  }
}
</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <!-- NAVBAR -->
    <nav class="fixed top-0 left-0 w-full mb-8 bg-gradient-to-r from-indigo-600 to-purple-600 shadow-lg rounded-2xl">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-16 items-center">
          <!-- Logo -->
          <div class="flex-shrink-0 text-white text-2xl font-bold tracking-wide">
             <svg width="50" height="50" viewBox="0 0 160 170" fill="none" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <linearGradient id="gradient" x1="0" y1="0" x2="1" y2="1">
                <stop offset="0%" stop-color="#4533EA
                "/>
                <stop offset="100%" stop-color="#ffff"/>
              </linearGradient>
              <filter id="shadow" x="-20%" y="-20%" width="140%" height="140%" color-interpolation-filters="sRGB">
                <feDropShadow dx="0" dy="6" stdDeviation="6" flood-color="#000000" flood-opacity="0.15"/>
              </filter>
            </defs>

            <rect x="5" y="5" width="140" height="140" rx="30" ry="30" fill="url(#gradient)" filter="url(#shadow)"/>
            <path d="M60 50V110C60 125 100 125 100 110" stroke="white" stroke-width="12" stroke-linecap="round" />
            <path d="M100 50V110C100 130 130 110 110 85C130 60 100 60 100 50Z" fill="white" />
            <text x="100" y="160"  text-anchor="middle" font-family="sans-serif" font-weight="700" font-size="50" fill="#ffff"> JDev </text>
          </svg>

          </div>

          <!-- Menu -->
          <div class="hidden md:flex space-x-6  ">
            <a
              v-for="item in menuItems"
              :key="item.name"
              :href="item.href"
              class=" text-white hover:text-yellow-300 transition duration-200"
            >
              {{ item.name }}
            </a>
          </div>
        </div>
      </div>
    </nav>

   
      <!-- About me -->
    <div class="mt-16 p-8 flex flex-col items-center gap-6">

      <!-- Titre centré au-dessus -->
      <h2 class="text-2xl font-bold text-center text-gray-800">
        À propos de moi
      </h2>

      <!-- Contenu principal : cercle + texte -->
      <div class="flex flex-col md:flex-row items-center justify-center gap-6 w-full">

        <!-- Cercle -->
        <div class="w-64 h-64 bg-yellow-300 rounded-full flex items-center justify-center text-2xl font-bold hover:shadow-xl">
          01
        </div>

        <!-- Rectangle -->
        <div class="w-full md:w-1/3 bg-pink-300 p-4 rounded-lg shadow hover:shadow-xl">
          <p class="text-gray-800 text-lg">
            02 – Ceci est un bloc de texte responsive. Il s'adapte à la taille de l'écran. Tu peux y mettre ta bio ou une description.
            02 – Ceci est un bloc de texte responsive. Il s'adapte à la taille de l'écran. Tu peux y mettre ta bio ou une description.
            02 – Ceci est un bloc de texte responsive. Il s'adapte à la taille de l'écran. Tu peux y mettre ta bio ou une description.
          </p>
        </div>

      </div>
    </div>


    <!-- DASHBOARD -->
    <main class="p-8">
      <h3 class="text-2xl font-bold text-gray-800 mb-6 text-center">Mon Dashboard</h3>

      <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
        <!-- Card 1 -->
        <div
          class="bg-white p-6 rounded-2xl shadow hover:shadow-xl transition duration-300"
        >
          <h2 class="text-lg font-semibold text-gray-700">Projets</h2>
          <p class="mt-2 text-gray-500">Nombre total : 12</p>
        </div>

        <!-- Card 2 -->

        <div class="bg-white p-6 rounded-2xl shadow hover:shadow-xl transition duration-300 max-w-2xl mx-auto">
          <!-- Titre -->
          <h2 class="text-lg font-semibold text-gray-700">Compétences</h2>
          <p class="mt-2 text-gray-500">Technos maîtrisées : 16</p>

          <!-- Liste des technos en deux colonnes -->
          <div class="mt-4 grid grid-cols-1 md:grid-cols-2 gap-y-4 gap-x-8">
            <!-- Colonne 1 -->
            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" alt="HTML5" class="w-6 h-6" />
              <span class="text-gray-700">HTML5</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" alt="CSS3" class="w-6 h-6" />
              <span class="text-gray-700">CSS3</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" alt="JavaScript" class="w-6 h-6" />
              <span class="text-gray-700">JavaScript</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/tailwindcss/tailwindcss-original.svg" alt="Tailwind CSS" class="w-6 h-6" />
              <span class="text-gray-700">Tailwind CSS</span>
            </div>
             <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/quarkus/quarkus-original.svg" alt="Tailwind CSS" class="w-6 h-6" />
              <span class="text-gray-700">Quarkus</span>
            </div>
             <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/php/php-original.svg" alt="Tailwind CSS" class="w-6 h-6" />
              <span class="text-gray-700">PHP</span>
            </div>

            <!-- Colonne 2 -->
            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" alt="React" class="w-6 h-6" />
              <span class="text-gray-700">React</span>
            </div>

             <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/reactnative/reactnative-original.svg" alt="React" class="w-6 h-6" />
              <span class="text-gray-700">React Native</span>
            </div>
            

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" alt="Node.js" class="w-6 h-6" />
              <span class="text-gray-700">Node.js</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original.svg" alt="Node.js" class="w-6 h-6" />
              <span class="text-gray-700">Java</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original.svg" alt="MongoDB" class="w-6 h-6" />
              <span class="text-gray-700">MongoDB</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" alt="Git" class="w-6 h-6" />
              <span class="text-gray-700">Git</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" alt="Docker" class="w-6 h-6" />
              <span class="text-gray-700">Docker</span>
            </div>

             <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postgresql/postgresql-original.svg" alt="postgresql" class="w-6 h-6" />
              <span class="text-gray-700">PostgreSQL</span>
            </div>
            
            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original.svg" alt="spring" class="w-6 h-6" />
              <span class="text-gray-700">Spring Boot</span>
            </div>

            <div class="flex items-center gap-4">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodered/nodered-line.svg" alt="node-red" class="w-6 h-6" />
              <span class="text-gray-700">Node-RED</span>
            </div>
          </div>
        </div>




        <!-- Card 3 -->
       <div class="bg-white p-6 rounded-2xl shadow hover:shadow-xl transition duration-300 max-w-2xl mx-md">
          <!-- Titre -->
          <h2 class="text-lg font-semibold text-gray-700">Contact</h2>
          <p class="mt-2 text-gray-500">3 messages récents</p>

          <!-- Infos de contact -->
          <div class="mt-4 space-y-4 text-gray-700">
            
            <!-- Ligne Email -->
            <div class="flex items-center gap-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 12H8m0 0l4 4m-4-4l4-4" />
              </svg>
              <a href="mailto:tonemail@example.com" class="hover:underline">griezmanjulio@yahoo.com</a>
            </div>

            <!-- Ligne Téléphone -->
            <div class="flex items-center gap-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 text-green-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M3 5h2l3.6 7.59L5.25 18H19v-2H7l1.1-2h7.45a1 1 0 0 0 .92-.63L21 4H5.21l-.94-2H1v2h2z" />
              </svg>
              <a href="tel:+1234567890" class="hover:underline">0763924350</a>
            </div>

            <!-- Ligne LinkedIn -->
            <div class="flex items-center gap-3">
              <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linkedin/linkedin-original.svg" class="w-5 h-5" alt="LinkedIn">
              <a href="https://linkedin.com/in/ton-profil" target="_blank" class="hover:underline">https://www.linkedin.com/in/julio-roigt-07b6071b4/</a>
            </div>

          </div>
        </div>

        <!-- Section Chat IA -->
  <div class="w-screen mt-12 flex justify-center px-4">
    <div class="bg-white p-6 rounded-2xl shadow hover:shadow-xl transition duration-300 max-w-3xl w-full">
      <h2 class="text-xl font-bold text-gray-800 text-center mb-2">💬 Discuter avec mon IA (Gemini)</h2>
      <p class="text-sm text-gray-500 text-center mb-6">
        Pose toutes tes questions à mon assistant IA pour en savoir plus sur mon profil, mes projets, mes compétences, ou même juste discuter !
      </p>

      <!-- Boîte de discussion -->
      <div
        ref="chatContainer"
        class="h-[500px] overflow-y-auto border border-gray-200 rounded p-4 bg-gray-50 mb-4 flex flex-col gap-3"
      >
        <div v-for="(msg, index) in messages" :key="index" class="flex" :class="msg.role === 'user' ? 'justify-end' : 'justify-start'">
          <div
            class="max-w-xs px-4 py-2 rounded-xl text-sm whitespace-pre-wrap"
            :class="msg.role === 'user' ? 'bg-blue-500 text-white rounded-br-none' : 'bg-gray-200 text-gray-800 rounded-bl-none'"
          >
            {{ msg.content }}
          </div>
        </div>
      </div>

      <!-- Formulaire de saisie -->
      <form @submit.prevent="sendMessage" class="flex gap-2">
        <input
          v-model="userMessage"
          type="text"
          placeholder="Écris un message..."
          class="flex-1 border border-gray-300 rounded px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
        <button
          type="submit"
          class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 text-sm rounded"
        >
          Envoyer
        </button>
      </form>
    </div>
  </div>
  </div>
    </main>
    <footer class="bg-gradient-to-r from-indigo-600 to-purple-600 shadow-lg rounded-2xl text-gray-300 py-6 mt-10">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex flex-col md:flex-row justify-between items-center">
        <!-- Logo ou titre -->
        <div class="mb-4 md:mb-0">
          <h1 class="text-xl font-bold text-white">Julio Roigt</h1>
          <p class="text-sm text-white">© 2025 MonSite. Tous droits réservés.</p>
        </div>

        <!-- Liens -->
        <div class="flex space-x-6">
          <a href="#" class="hover:text-white transition">Accueil</a>
          <a href="#" class="hover:text-white transition">À propos</a>
          <a href="#" class="hover:text-white transition">Contact</a>
        </div>

        <!-- Réseaux sociaux -->
        <div class="flex space-x-4 mt-4 md:mt-0">
          <a href="#" class="hover:text-white transition" aria-label="Twitter">
            <i class="fab fa-twitter"></i>
          </a>
          <a href="#" class="hover:text-white transition" aria-label="LinkedIn">
            <i class="fab fa-linkedin"></i>
          </a>
          <a href="#" class="hover:text-white transition" aria-label="GitHub">
            <i class="fab fa-github"></i>
          </a>
        </div>
      </div>
    </div>
  </footer>


   
   

  </div>
</template>

<style scoped>
/* Tu peux ajouter un peu d’animation custom ici si tu veux */
</style>
