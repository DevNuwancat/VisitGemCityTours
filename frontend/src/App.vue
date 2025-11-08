<script setup lang="ts">
// Remove this unused import line:
// import HelloWorld from './components/HelloWorld.vue'

import { ref, computed } from 'vue'

const props = defineProps<{
  formspreeId?: string  // Also add this missing prop
  subject?: string      // Also add this missing prop
  next?: string
}>()

const endpoint = computed(() => `https://formspree.io/f/${props.formspreeId || import.meta.env.VITE_FORMSPREE_ID || 'xvgvevqv'}`)
// ...existing code...
const subject = computed(() => props.subject ?? 'New Booking Request - Visit Gem City')
const next = computed(() => props.next ?? '')

const loading = ref(false)
const status = ref<{ ok: boolean; message: string }>({ ok: false, message: '' })

const model = ref({
  name: '',
  email: '',
  phone: '',
  guests: 2 as number | string,
  date: '',
  package: 'Gem City Essentials (Full‑day)',
  pickup: '',
  notes: ''
})

async function onSubmit(e: Event) {
  const form = e.target as HTMLFormElement
  loading.value = true
  status.value = { ok: false, message: 'Sending…' }

  try {
    const formData = new FormData(form)
    // Include v-model state in case some fields are not synced by name
    Object.entries(model.value).forEach(([k, v]) => formData.set(k, String(v ?? '')))

    const res = await fetch(endpoint.value, {
      method: 'POST',
      headers: { Accept: 'application/json' },
      body: formData
    })

    if (res.ok) {
      status.value = { ok: true, message: 'Thanks! We received your request. We’ll email you to confirm shortly.' }
      // Clear form
      Object.keys(model.value).forEach((k) => ((model.value as any)[k] = ''))
      model.value.guests = 2
      if (next.value) {
        window.location.href = next.value
      }
    } else {
      const data = await res.json().catch(() => ({} as any))
      const msg = data?.errors?.[0]?.message || 'Could not send. Please try again.'
      status.value = { ok: false, message: msg }
    }
  } catch (err) {
    status.value = { ok: false, message: 'Network error. Please try again.' }
  } finally {
    loading.value = false
  }
}
</script>


<template>
<body class="bg-linear-to-b from-sky-50 via-white to-white dark:from-slate-950 dark:via-slate-900 dark:to-slate-950 text-slate-800 dark:text-slate-100 min-h-screen">
  
<header class="sticky top-0 z-50">
  <nav class="items-center justify-between flex
              mx-auto max-w-7xl px-2 sm:px-6 lg:px-8 py-2
              bg-gray-900 rounded-b-2xl shadow-lg glass">

              <!--logo-->
              <div class="flex">
              <a href="">
              <img src="./assets/logo_solo_t.png" alt="VistGem Logo" class="h-12 w-auto shadow-2xl" />
              </a>
              <a href="" class="justify-center items-center flex ml-2">
              <img src="./assets/logo_name.png" alt="VistGem Text" class="h-8 w-auto" />
              </a>
              </div>

              <div class="hidden sm:flex gap-6 text-sm font-sans items-center ">
                <a href="#story" class="hover:text-cyan-500 duration-150 ease-in transition-all">Gem City Stoy</a>
                <a href="#attraction" class="hover:text-cyan-500 duration-150 ease-in transition-all">Attraction</a>
                <a href="#packages" class="hover:text-cyan-500 duration-150 ease-in transition-all">Packages</a>
                <a href="#whyus" class="hover:text-cyan-500 duration-150 ease-in transition-all">Why Us</a>
                

                <div class="relative flex">
                <a href="#" class="inline-flex relative px-4 py-2 rounded-md
                                   bg-linear-to-r from-cyan-600 to-blue-600 text-white shadow hover:opacity-90 border-2 border-gray-800">Book</a>
                <span class="absolute -right-1.5 -top-1 h-4 w-4 rounded-full bg-sky-300 opacity-90
                 shadow-lg"></span>
                <span class="absolute -right-1.5 -top-1 h-4 w-4 rounded-full bg-sky-400 opacity-75 animate-ping"></span>
                </div>
              </div>            
  </nav>
</header>


 <!--HERO inspired layout -->
 <section id="home" class="mx-auto my-10 max-w-7xl p-4 sm:p-6 lg:p-8">
    <!--Image Cover-->
    <div class="h-[62vh] sm:h-[72vh] relative rounded-2xl overflow-hidden">
      <!--image-->
      <img class="absolute inset-0 object-cover h-full w-full " src="./assets/images/cover4.png" alt="Ratnapura Gem tourism,">
      <!--gradient overlay-->
      <div class="absolute inset-0 bg-linear-to-b from-transparent to-black opacity-50"></div>

      <div class="relative z-10 h-full flex flex-col justify-between">

        <!--Text Content-->

        <div class="flex item-center justify-between p-5">
          <div class="flex items-center gap-2  text-cyan-00/90">
            <span class="inline-flex items-center gap-2  py-1 rounded-full text-3xl"></span>
            <span class="hidden sm:inline text-xs bg-gray-700/40 px-2 py-1 rounded-full border ">Local Guides • Small Groups  • Real Experience </span>
          </div>
          
          <div>
            <a href="#book" class="hidden sm:inline-flex items-center gap-2 px-4 py-2 rounded-full bg-white/0 ring-1 ring-white/80 text-white text-sm hover:bg-white/20">Let's Go →</a>
          </div>
           
        </div>

          <div class="p-6 sm:p-10">
          <h1 class="text-xl sm:text-7xl font-black leading-tight">Visit</h1>
          <h1 class="text-5xl sm:text-7xl font-black leading-tight">Gem City</h1>

          <p class="mt-3 max-w-2xl text-white/80 text-2xl">Home of precious gems, nature and beautiful people.</br>
          
          </p>

        </div>

          <div class="p-4  sm:p-2 grid sm:grid-cols-2 gap-4">
            
            <div class="rounded-lg bg-blue-400/10 backdrop-blur p-4 text-sm ">
            <div class="font-semibold">What is Gem City Tour</div>
            <p class="text-white/80 mt-1">Short, honest tours created by locals. Ethical basics of gem trading, respect for nature, and warm community visits with in-hand fully immersive experience tour.</p>
            </div>

            <div class="rounded-lg bg-blue-400/10 backdrop-blur p-4 text-sm ">
            <div class="font-semibold">About the Experience</div>
            <p class="text-white/80 mt-1">Real moments — A peaceful journey where you learn, touch, and connect with Sri Lanka’s gem heart and nature.</p>
            </div>

          </div>

      </div>

    </div>


 </section>



<!--Why GemCity-->
<section class="py-20 bg-white dark:bg-slate-950" id="story">
  <div class="mx-auto max-w-5xl px-4 sm:px-6 lg:px-8">
    
    <!-- Hero Header -->
    <div class="text-center mb-16">
      <span class="inline-block px-4 py-2 bg-cyan-100 dark:bg-cyan-900/30 text-cyan-700 dark:text-cyan-300 text-sm font-medium rounded-full mb-4">
        The Untold Story
      </span>
      <h1 class="text-4xl md:text-5xl font-bold text-slate-900 dark:text-white mb-6 leading-tight">
        Where Royal Gems <br/> Meet Wild Nature
      </h1>
      <p class="text-xl text-slate-600 dark:text-slate-400 max-w-3xl mx-auto">
        From the British Crown Jewels to hidden waterfalls — discover why Ratnapura holds secrets that shaped empires and nature that takes your breath away.
      </p>
    </div>

    <!-- Main Content Grid -->
    <div class="grid lg:grid-cols-2 gap-16 items-start">
      
      <!-- Text Content -->
      <div class="space-y-8">
        
        <!-- Royal Heritage Section -->
        <div>
          <h2 class="text-2xl font-bold text-slate-900 dark:text-white mb-4 flex items-center">
            <span class="w-8 h-8 bg-gradient-to-r from-yellow-400 to-orange-500 rounded-full mr-3 flex items-center justify-center">
              👑
            </span>
            The Royal Connection
          </h2>
          <p class="text-slate-700 dark:text-slate-300 leading-relaxed mb-4">
            The famous <strong>St. Edward's Sapphire</strong> in the British Crown — it came from these very hills. For centuries, Ratnapura's sapphires have adorned royal collections worldwide. The <em>Star of India</em>, the <em>Logan Sapphire</em> — all traced back to our ancient riverbeds.
          </p>
          <div class="bg-gradient-to-r from-blue-50 to-indigo-50 dark:from-slate-800 dark:to-slate-700 p-6 rounded-2xl border-l-4 border-blue-500">
            <p class="text-slate-600 dark:text-slate-300 text-sm font-medium">
              <strong>Did you know?</strong> The 104-carat Stuart Sapphire in the British Imperial State Crown is believed to have Sri Lankan origins — possibly from Ratnapura's historic mines.
            </p>
          </div>
        </div>

        <!-- Nature Section -->
        <div>
          <h2 class="text-2xl font-bold text-slate-900 dark:text-white mb-4 flex items-center">
            <span class="w-8 h-8 bg-gradient-to-r from-green-400 to-blue-500 rounded-full mr-3 flex items-center justify-center">
              🏞️
            </span>
            Nature's Hidden Gems
          </h2>
          <p class="text-slate-700 dark:text-slate-300 leading-relaxed mb-6">
            Beyond the gemstones lies untouched wilderness. <strong>Bopath Ella</strong> — a waterfall shaped like a perfect Bo tree leaf, sacred to locals. <strong>Sinharaja Forest</strong> nearby, a UNESCO World Heritage rainforest where endemic species thrive in misty silence. And in the heart of Ratnapura stands the <strong>Sabaragamuwa Maha Saman Dewalaya</strong>, a temple of deep devotion and timeless culture, connecting nature, faith, and the spirit of this land.
          </p>
          
          <!-- Nature Highlights -->
          <div class="grid grid-cols-2 gap-4">
            <div class="bg-white dark:bg-slate-800 p-4 rounded-xl border border-slate-200 dark:border-slate-700">
              <h4 class="font-semibold text-slate-900 dark:text-white mb-2">Bopath Ella</h4>
              <p class="text-sm text-slate-600 dark:text-slate-400">Sacred waterfall, 30m drop, perfect for meditation</p>
            </div>
            <div class="bg-white dark:bg-slate-800 p-4 rounded-xl border border-slate-200 dark:border-slate-700">
              <h4 class="font-semibold text-slate-900 dark:text-white mb-2">Saman Dewalaya</h4>
              <p class="text-sm text-slate-600 dark:text-slate-400">Ancient heritage, spiritual rhythm, vibrant culture</p>
            </div>
          </div>
        </div>

        <!-- Modern Reality -->
        <div>
          <h2 class="text-2xl font-bold text-slate-900 dark:text-white mb-4">Today's Reality</h2>
          <p class="text-slate-700 dark:text-slate-300 leading-relaxed">
            While the world changed, Ratnapura remained authentic. Families still pan for gems using techniques from their great-grandfathers. Morning mist still rises from Bopath Ella. The difference? Now you can experience both worlds — royal history and raw nature — in a single day.
          </p>
        </div>

        <!-- CTA -->
        <div class="pt-6">
          <a href="#book" class="inline-flex items-center px-6 py-3 bg-slate-900 dark:bg-white text-white dark:text-slate-900 rounded-full font-medium hover:opacity-90 transition-opacity">
            Experience Both Worlds
            <svg class="ml-2 w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"/>
            </svg>
          </a>
        </div>

      </div>

      <!-- Visual Content -->
      <div class="space-y-6">
        
        <!-- Main Hero Image -->
        <div class="relative rounded-3xl overflow-hidden shadow-2xl">
          <img src="./assets/images/blog11.png" alt="Ratnapura Gem Mining" class="w-full h-80 object-cover">
          <div class="absolute bottom-4 left-4 bg-black/50 backdrop-blur-sm text-white px-4 py-2 rounded-full text-sm">
            Traditional gem mining
          </div>
        </div>

        <!-- Image Grid -->
        <div class="grid grid-cols-2 gap-4">
          <div class="relative rounded-2xl overflow-hidden shadow-lg">
            <img src="./assets/images/blog2.png" alt="Bopath Ella Waterfall" class="w-full h-40 object-cover">
            <div class="absolute bottom-2 left-2 bg-white/90 dark:bg-slate-800/90 text-slate-800 dark:text-white px-3 py-1 rounded-full text-xs font-medium">
              Natural Gem Stones
            </div>
          </div>
          <div class="relative rounded-2xl overflow-hidden shadow-lg">
            <img src="./assets/images/blog3.png" alt="Gem Market" class="w-full h-40 object-cover">
            <div class="absolute bottom-2 left-2 bg-white/90 dark:bg-slate-800/90 text-slate-800 dark:text-white px-3 py-1 rounded-full text-xs font-medium">
              Bopath fall
            </div>
          </div>
        </div>

        <!-- Stats Card -->
        <div class="bg-gradient-to-br from-slate-50 to-white dark:from-slate-800 dark:to-slate-700 p-6 rounded-2xl border border-slate-200 dark:border-slate-600">
          <div class="grid grid-cols-3 gap-4 text-center">
            <div>
              <div class="text-2xl font-bold text-cyan-600 dark:text-cyan-400">2000+</div>
              <div class="text-xs text-slate-600 dark:text-slate-400">Years of history</div>
            </div>
            <div>
              <div class="text-2xl font-bold text-emerald-600 dark:text-emerald-400">15+</div>
              <div class="text-xs text-slate-600 dark:text-slate-400">Gem varieties</div>
            </div>
            <div>
              <div class="text-2xl font-bold text-purple-600 dark:text-purple-400">3</div>
              <div class="text-xs text-slate-600 dark:text-slate-400">UNESCO sites nearby</div>
            </div>
          </div>
        </div>

      </div>

    </div>

  </div>
</section>



 <!-- Attraction -->
 <section class="py-16 md:py-16" id="attraction">
  <div class="mx-auto max-w-7xl px-4 sm:px-6 lg-px-8">
    <div class="mb-10 justify-between flex gap-4">
      <div class=" mx-left">
        <h2 class="text-3xl md:text-4xl font-extrabold">Selected Attractions</h2>
        <p class="mt-2 text-slate-600 dark:text-slate-300">Shortlist of spots we actually visit</p>
      </div>
      <div class="mx-right">
        hello
      </div>

        
    </div>

      <div class="grid md:grid-cols-3 gap-6">
        <!-- Card 1 -->
        <article class="rounded-2xl  dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <img src="./assets/images/image1.png" alt="Gem Museum" class="w-full h-40 object-cover rounded-t-2xl">
          <div class="p-5">
            <h3 class="font-bold text-lg">Gem Mining Experience — Ratnapura Fields</h3>
            <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">Dig, wash, and discover. Join local miners to uncover raw gems hidden beneath the earth. Learn traditional mining methods and how nature shapes every stone.</p>
            <ul class="mt-3 text-sm list-disc list-inside space-y-1 text-slate-600 dark:text-slate-300">

            <li>Hands-on experience</li>
            <li>Guided by local experts</li>
          </ul>
          </div>
        
        </article>

        <!-- Card 2 -->
        <article class="rounded-2xl  dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <img src="./assets/images/image3.png" alt="Gem Museum" class="w-full h-40 object-cover rounded-t-2xl">
          <div class="p-5">
            <h3 class="font-bold text-lg">The Underground Gem Journey — Discover the Source</h3>
            <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">Go beyond the surface.Walk through the quiet gem fields and learn how miners uncover stones with care, balance, and respect for nature.</p>
            <ul class="mt-3 text-sm list-disc list-inside space-y-1 text-slate-600 dark:text-slate-300">

            <li>Safe guided pit visit</li>
            <li>See how sustainable mining works</li>
          </ul>
          </div>
        
        </article>

        <!-- Card 3 -->
        <article class="rounded-2xl  dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <img src="./assets/images/image6.png" alt="Gem Museum" class="w-full h-40 object-cover rounded-t-2xl">
          <div class="p-5">
            <h3 class="font-bold text-lg">Gem Cutting Workshop — Shape the Beauty</h3>
            <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">See how a rough stone becomes a sparkling gem. Learn the art of cutting and polishing with local craftsmen, and shape your own piece under gentle guidance.</p>
            <ul class="mt-3 text-sm list-disc list-inside space-y-1 text-slate-600 dark:text-slate-300">

            <li>Hands-on gem cutting session</li>
            <li>Guided by master cutters</li>
          </ul>
          </div>
        
        </article>
      </div>

      

  </div>
 </section>



 <!-- Attraction -->
 <section class="-mt-8 md:mt-0">
  <div class="mx-auto max-w-7xl px-4 sm:px-6 lg-px-8">
    

      <div class="grid md:grid-cols-3 gap-6">
        <!-- Card 1 -->
        <article class="rounded-2xl  dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <img src="./assets/images/image8.png" alt="Gem Museum" class="w-full h-40 object-cover rounded-t-2xl">
          <div class="p-5">
            <h3 class="font-bold text-lg">Gem Market Walk — Ratnapura Town</h3>
            <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">See traders in action and feel the vibrant pulse of Sri Lanka’s gem capital.identify genuine gems with expert guidance. Meet real gem buyers, explore their unique collections, and shop safely with confidence.</p>
            <ul class="mt-3 text-sm list-disc list-inside space-y-1 text-slate-600 dark:text-slate-300">

            <li>Best on weekday mornings</li>
            <li>Expert help to avoid scams</li>
          </ul>
          </div>
        
        </article>

        <!-- Card 2 -->
        <article class="rounded-2xl  dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <img src="./assets/images/image7.png" alt="Gem Museum" class="w-full h-40 object-cover rounded-t-2xl">
          <div class="p-5">
            <h3 class="font-bold text-lg">Sabaragamuwa Maha Saman Dewalaya — Spirit of the City</h3>
            <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">Step into one of Sri Lanka’s most sacred temples, dedicated to Saman Deviyo — the guardian of the mountain and protector of the land. Feel the calm rhythm of drums, the scent of incense, and the warmth of devotion that unites people and nature</p>
            <ul class="mt-3 text-sm list-disc list-inside space-y-1 text-slate-600 dark:text-slate-300">

            <li>Learn the story of Saman Deviyo</li>
            <li>Experience local rituals and blessings</li>
          </ul>
          </div>
        
        </article>

        <!-- Card 3 -->
        <article class="rounded-2xl  dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <img src="./assets/images/image3.jpg" alt="Gem Museum" class="w-full h-40 object-cover rounded-t-2xl">
          <div class="p-5">
            <h3 class="font-bold text-lg">Bopath Ella Waterfall — Nature’s Calm Escape</h3>
            <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">A waterfall shaped like a sacred Bo leaf, flowing gently through emerald forests. Spend quiet moments by the water, breathe in the fresh mist, and let nature guide your peace.</p>
            <ul class="mt-3 text-sm list-disc list-inside space-y-1 text-slate-600 dark:text-slate-300">

            <li>Refreshing nature stop</li>
            <li>Perfect for meditation and reflection</li>
          </ul>
          </div>
        
        </article>
      </div>
      </div>
 </section>

<!--Simple photo gallery-->
<section>
  <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-10">
    <h2 class="text-3xl md:text-4xl font-extrabold text-center">Photo Gallery</h2>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-6 mt-6">
      <img src="./assets/images/image2.jpg" alt="Gallery Image 1" class="w-full h-auto object-cover rounded-lg aspect-square overflow-hidden shadow-lg">
      <img src="./assets/images/gallery1.png" alt="Gallery Image 2" class="w-full h-auto object-cover aspect-square rounded-lg overflow-hidden shadow-lg">
      <img src="./assets/images/image3.jpg" alt="Gallery Image 3" class="w-full h-auto object-cover aspect-square rounded-lg overflow-hidden shadow-lg">
      <img src="./assets/images/gallery2.png" alt="Gallery Image 3" class="w-full h-auto object-cover aspect-square rounded-lg overflow-hidden shadow-lg">
      <img src="./assets/images/gallery3.png" alt="Gallery Image 3" class="w-full h-auto object-cover aspect-square rounded-lg overflow-hidden shadow-lg">
      <img src="./assets/images/blog3.png" alt="Gallery Image 3" class="w-full h-auto object-cover aspect-square rounded-lg overflow-hidden shadow-lg">
      
    </div>
  </div>
</section>

 <!--few videos-->
<!--section>
  <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-10">
    <div class="mb-10 text-center">
      <h2 class="text-3xl md:text-4xl font-extrabold">Experience Highlights</h2>
      <p class="mt-2 text-slate-600 dark:text-slate-300">Short clips from our tours</p>
    </div>
    <div class="grid md:grid-cols-3 gap-6">
      
      <div class="rounded-2xl overflow-hidden shadow-lg">
        <video controls class="w-full h-auto">
          <source src="./assets/videos/video1.mp4" type="video/mp4">
          Your browser does not support the video tag.
        </video>
      </div>
      
      <div class="rounded-2xl overflow-hidden shadow-lg">
        <video controls class="w-full h-auto">
          <source src="./assets/videos/video2.mp4" type="video/mp4">
          Your browser does not support the video tag.
        </video>
      </div>
      
      <div class="rounded-2xl overflow-hidden shadow-lg">
        <video controls class="w-full h-auto">
          <source src="./assets/videos/video3.mp4" type="video/mp4">
          Your browser does not support the video tag.
        </video>
      </div>
    </div>
  </div>
</section-->


 <!--Packages-->
  <section id="packages" class="py-16 md:py-24">
    <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
      <div class="mb-10 text-center">
        <h2 class="text-3xl md:text-4xl font-extrabold">Simple Packages</h2>
        <p class="mt-2 text-slate-600 dark:text-slate-300">Pick one. We'll tailor the timing for your day.</p>
      </div>
      <div class="grid md:grid-cols-2 gap-6">
        <!-- Package A -->
        <div class="rounded-3xl p-6 bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow flex flex-col">
          <div class="flex items-center justify-between">
            <h3 class="text-2xl font-bold">Gem City Essentials (Full‑day)</h3>
            <span class="px-3 py-1 rounded-full text-xs bg-emerald-100 text-emerald-700 dark:bg-emerald-900/40 dark:text-emerald-200">Best Seller</span>
          </div>
          <p class="mt-2 text-slate-600 dark:text-slate-300">Enjoy a complete gem journey — from the Gem Market Walk to an authentic Gem Mining Experience, followed by a Gem Cutting Workshop, and a relaxing Tea Break with Lunch Stop.<i class="fa fa-stop"></i>.</p>
          <ul class="mt-4 text-sm space-y-2 text-slate-700 dark:text-slate-300">
            <li>⏱️ 8-9 hours</li>
            <li>👥 2–8 guests</li>
            <li>🛺 🚙 Private vehicle included (Price may vary depending on your pick-up and drop-off location)</li>
            <li class="text-gray-500">If you use your own transport or come by taxi, we’ll adjust the price and reduce the transport cost</li>
          </ul>
          <div class="mt-6 flex items-end gap-3">
            <div>
              <div class="text-3xl font-extrabold text-gray">$120 <span class="text-sm text-gray-400">LKR 35,500</span></div>
              <div class="text-xs text-slate-500">per person</div>
            </div>
            <div class="ml-auto">
              <a href="#book" class="inline-flex items-center px-5 py-3 rounded-xl bg-linear-to-r from-cyan-600 to-blue-600 text-white font-semibold shadow hover:opacity-90" data-package="Gem City Essentials (‑day)">Book this</a>
            </div>
          </div>
        </div>
        <!-- Package B -->
        <div class="rounded-3xl p-6 bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow flex flex-col">
          <div class="flex items-center justify-between">
            <h3 class="text-2xl font-bold">Gem & Nature Explorer (Two‑days)</h3>
            <span class="px-3 py-1 rounded-full text-xs bg-cyan-100 text-cyan-700 dark:bg-cyan-900/40 dark:text-cyan-200">Most Scenic</span>
          </div>
          <p class="mt-2 text-slate-600 dark:text-slate-300">All packages include the Gem City Essentials experience, an overnight stay at a luxury hotel with dinner, and a morning breakfast, followed by a nature exploration to Bopath Ella Waterfall, Saman Dewalaya, and scenic areas around Ratnapura.</p>
          <ul class="mt-4 text-sm space-y-2 text-slate-700 dark:text-slate-300">
            <li>⏱️ 2 days</li>
            <li>👥 2–10 guests</li>
            <li >🛺 🚙 Private vehicle included (Price may vary depending on your pick-up and drop-off location)</li>
            <li class="text-gray-500">If you use your own transport or come by taxi, we’ll adjust the price and reduce the transport cost</li>
          </ul>
          <div class="mt-6 flex items-end gap-3">
            <div>
              <div class="text-3xl font-extrabold text-gray">$220 <span class="text-sm text-gray-400">LKR 67,000</span></div>
              <div class="text-xs text-slate-500">per person</div>
            </div>
            <div class="ml-auto">
              <a href="#book" class="inline-flex items-center px-5 py-3 rounded-xl bg-linear-to-r from-cyan-600 to-blue-600 text-white font-semibold shadow hover:opacity-90" data-package="Gem & Nature Explorer (Two‑days)">Book this</a>
            </div>
          </div>
        </div>
      </div>
      <p class="mt-6 text-center text-xs text-slate-500">* Prices are indicative; confirm in your booking email. Add‑ons: hotel pickup, drone footage, pro photos.</p>
    </div>
  </section>

  <!--Why With us-->
  <!-- WHY US -->
  <section id="whyus" class="py-16 md:py-20 bg-slate-50/60 dark:bg-slate-900/40">
    <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 grid md:grid-cols-3 gap-6 items-start">
      <div class="md:col-span-1">
        <h2 class="text-3xl md:text-4xl font-extrabold">Why travel with us?</h2>
        <p class="mt-3 text-slate-600 dark:text-slate-300">Local team from Ratnapura with honest pricing and friendly pacing for introverts too.</p>
      </div>
      <div class="md:col-span-2 grid sm:grid-cols-2 gap-6">
        <div class="p-5 rounded-2xl bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <h3 class="font-bold">Local Access</h3>
          <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">See real gem trading and meet makers — not just tourist shops.</p>
        </div>
        <div class="p-5 rounded-2xl bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <h3 class="font-bold">Small & Flexible</h3>
          <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">We adjust start times, pace, and stops for your vibe.</p>
        </div>
        <div class="p-5 rounded-2xl bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <h3 class="font-bold">Media Add‑ons</h3>
          <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">Optional photos/video and quick reels for your socials.</p>
        </div>
        <div class="p-5 rounded-2xl bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow">
          <h3 class="font-bold">Community Impact</h3>
          <p class="mt-2 text-sm text-slate-600 dark:text-slate-300">Part of your fee goes to local crafts & education.</p>
        </div>
      </div>
    </div>
  </section>

<!--booking-->

  <!-- BOOKING FORM (Vue 3 + Formspree) -->
  <section id="book" class="py-16 md:py-24">
    <div class="mx-auto max-w-3xl px-4 sm:px-6 lg:px-8">
      <div class="mb-8 text-center">
        <h2 class="text-3xl md:text-4xl font-extrabold">Book by Email</h2>
        <p class="mt-2 text-slate-600 dark:text-slate-300">
          Fill this form and we’ll receive your request instantly. We’ll reply by email to confirm. No payment now.
        </p>
      </div>

      <form
        id="bookingForm"
        :action="endpoint"
        method="POST"
        class="rounded-3xl p-6 bg-white/80 dark:bg-slate-800/60 ring-1 ring-black/5 shadow grid gap-4"
        @submit.prevent="onSubmit"
      >
        <!-- Formspree helpers -->
        <input type="hidden" name="_subject" :value="subject" />
        <input v-if="next" type="hidden" name="_next" :value="next" />
        <!-- <input type=\"hidden\" name=\"_replyto\" :value=\"model.email\" />  -->

        <div class="grid md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium">Full Name</label>
            <input
              required
              name="name"
              v-model="model.name"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
              placeholder="e.g., Jane Doe"
            />
          </div>
          <div>
            <label class="block text-sm font-medium">Email</label>
            <input
              required
              type="email"
              name="email"
              v-model="model.email"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
              placeholder="your@email.com"
            />
          </div>
        </div>

        <div class="grid md:grid-cols-3 gap-4">
          <div>
            <label class="block text-sm font-medium">Phone / WhatsApp</label>
            <input
              name="phone"
              v-model="model.phone"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
              placeholder="+94…"
            />
          </div>
          <div>
            <label class="block text-sm font-medium">Guests</label>
            <input
              required
              type="number"
              min="1"
              name="guests"
              v-model.number="model.guests"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
            />
          </div>
          <div>
            <label class="block text-sm font-medium">Preferred Date</label>
            <input
              required
              type="date"
              name="date"
              v-model="model.date"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
            />
          </div>
        </div>

        <div class="grid md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium">Package</label>
            <select
              name="package"
              v-model="model.package"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
            >
              <option>Gem City Essentials (Full‑day)</option>
              <option>Gem & Nature Explorer (Two‑days)</option>
            </select>
          </div>
          <div>
            <label class="block text-sm font-medium">Pickup Location (Hotel/City)</label>
            <input
              name="pickup"
              v-model="model.pickup"
              class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
              placeholder="e.g., Colombo / Ratnapura"
            />
          </div>
        </div>

        <div>
          <label class="block text-sm font-medium">Notes</label>
          <textarea
            name="notes"
            rows="4"
            v-model="model.notes"
            class="mt-1 w-full rounded-xl border border-slate-300/60 dark:border-slate-700/60 bg-white/70 dark:bg-slate-900/40 px-3 py-2"
            placeholder="Anything we should know? Allergies, timing, kids, etc."
          ></textarea>
        </div>

        <!-- Honeypot for spam bots (hidden) -->
        <input type="text" name="_honeypot" tabindex="-1" autocomplete="off" class="hidden" />

        <div class="flex items-center gap-3">
          <button
            id="submitBtn"
            type="submit"
            class="px-5 py-3 rounded-xl bg-gradient-to-r from-cyan-600 to-blue-600 text-white font-semibold shadow disabled:opacity-60"
            :disabled="loading"
          >
            <span v-if="!loading">Send Booking Request</span>
            <span v-else>Sending…</span>
          </button>
          <span id="status" class="text-sm" :class="status.ok ? 'text-emerald-600' : 'text-slate-500'">
            {{ status.message }}
          </span>
        </div>

        <p class="text-xs text-slate-500">
          By emailing us you agree to our simple terms: respectful behavior, weather‑safe decisions, and fair rescheduling.
        </p>
      </form>
    </div>
  </section>

  <!--Partners-->
  <section>
    <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-8">
      <div class="mb-6 text-center">
        <h2 class="text-2xl md:text-3xl font-extrabold">Our Partners, Supporters and Community</h2>
        <p class="mt-2 text-slate-600 dark:text-slate-300">Proud collaborators in community and sustainability.</p>
      </div>
      <div class="flex flex-wrap justify-center items-center gap-8">
        <img src="./assets/Partnerlogo/p_logo_1.png" alt="Partner 1" class="h-24 opacity-80 hover:opacity-100 transition-opacity duration-150" />
        <img src="./assets/Partnerlogo/p_logo_3.png" alt="Partner 3" class="h-20 opacity-80 hover:opacity-100 transition-opacity duration-150" />
        <img src="./assets/Partnerlogo/p_logo_4.png" alt="Partner 4" class="h-18 opacity-80 hover:opacity-100 transition-opacity duration-150" />
         <img src="./assets/Partnerlogo/p_logo_5.png" alt="Partner 5" class="h-24 opacity-80 hover:opacity-100 transition-opacity duration-150" />
         <img src="./assets/Partnerlogo/p_logo_6.png" alt="Partner 5" class="h-6 opacity-80 hover:opacity-100 transition-opacity duration-150" />
      </div>
       <div class="flex flex-wrap justify-center items-center gap-8 py-4">
        
      
      </div>
    </div>
  </section>



  <!--Social media-->

<!--Social media-->
<section class="py-16 bg-slate-50/60 dark:bg-slate-900/40">
  <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
    <div class="text-center">
      <h2 class="text-3xl md:text-4xl font-extrabold mb-4">Follow Our Journey</h2>
      <p class="mt-2 text-slate-600 dark:text-slate-300 mb-8">Stay connected for latest updates and real moments from our tours</p>
      
      <!-- Social Media Links -->
      <div class="flex justify-center items-center gap-6 mb-8">
        <a href="#" class="group flex items-center justify-center w-12 h-12 rounded-full bg-gradient-to-r from-pink-500 to-orange-500 text-white hover:scale-110 transition-transform duration-200">
          <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
          </svg>
        </a>

        <a href="#" class="group flex items-center justify-center w-12 h-12 rounded-full bg-blue-600 text-white hover:scale-110 transition-transform duration-200">
          <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
            <path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"/>
          </svg>
        </a>

        <a href="#" class="group flex items-center justify-center w-12 h-12 rounded-full bg-blue-700 text-white hover:scale-110 transition-transform duration-200">
          <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
            <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
          </svg>
        </a>

        <a href="#" class="group flex items-center justify-center w-12 h-12 rounded-full bg-red-600 text-white hover:scale-110 transition-transform duration-200">
          <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
            <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
          </svg>
        </a>

        <a href="#" class="group flex items-center justify-center w-12 h-12 rounded-full bg-green-500 text-white hover:scale-110 transition-transform duration-200">
          <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
            <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893A11.821 11.821 0 0020.465 3.488"/>
          </svg>
        </a>
      </div>

      <!-- Social Media Feed Preview -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-4xl mx-auto">
        <div class="aspect-square rounded-lg overflow-hidden shadow-lg">
          <img src="./assets/images/image1.png" alt="Social Media Post 1" class="w-full h-full object-cover hover:scale-105 transition-transform duration-300">
        </div>
        <div class="aspect-square rounded-lg overflow-hidden shadow-lg">
          <img src="./assets/images/image4.png" alt="Social Media Post 2" class="w-full h-full object-cover hover:scale-105 transition-transform duration-300">
        </div>
        <div class="aspect-square rounded-lg overflow-hidden shadow-lg">
          <img src="./assets/images/image3.jpg" alt="Social Media Post 3" class="w-full h-full object-cover hover:scale-105 transition-transform duration-300">
        </div>
        <div class="aspect-square rounded-lg overflow-hidden shadow-lg">
          <img src="./assets/images/image3.png" alt="Social Media Post 4" class="w-full h-full object-cover hover:scale-105 transition-transform duration-300">
        </div>
      </div>

      <!-- Hashtags -->
      <div class="mt-8">
        <p class="text-cyan-600 dark:text-cyan-400 font-medium">
          #VisitGemCity #RatnapuraGems #SriLankaTourism #GemMining #EthicalTourism #LocalExperience
        </p>
      </div>
    </div>
  </div>
</section>

  <!-- Footer -->
  <section>
    <footer class="bg-gray-900 rounded-t-2xl shadow-lg glass mx-auto max-w-7xl px-2 sm:px-6 lg:px-8 py-6 mt-10">
      <div class="text-center text-sm text-gray-300">
        &copy; 2024 Visit Gem City. All rights reserved.
      </div>
    </footer>
  </section>


</body>

</template>

<style scoped>
    

</style>
