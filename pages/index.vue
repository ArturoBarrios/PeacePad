<script setup>
import { ref, onMounted } from 'vue'


const showModal = ref(false)
const email = ref('')
const phone = ref('')
const message = ref('')
const isSending = ref(false)
const sent = ref(false)
const error = ref('')
const successMessage = ref('Thanks! We’ll keep you updated.')
const config = useRuntimeConfig()
const contentSection = ref(null)

const isMuted = ref(true)
const bgVideo = ref(null)




onMounted(() => {
  if (bgVideo.value) {
    console.log("Video element is ready:", bgVideo.value)
  } else {
    console.warn("bgVideo ref is still null onMounted")
  }
})
const toggleMute = () => {
    console.log("Toggling mute value: " + bgVideo.value);
  if (bgVideo.value) {
    isMuted.value = !isMuted.value
    bgVideo.value.muted = isMuted.value
  }
}


const scrollToContent = () => {
  if (contentSection.value) {
    contentSection.value.scrollIntoView({ behavior: 'smooth' })
  }
}


const handleSubmit = async () => {
  error.value = ''

  if (!email.value.trim()) {
    error.value = 'Please enter an email address.'
    return
  }

  isSending.value = true
  const { server } = config.public

  try {
    const response = await $fetch('/graphql', {
      baseURL: server,
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: {
        query: `
          mutation SendTestEmail($to: String!) {
            sendTestEmail(to: $to)
          }
        `,
        variables: {
          to: email.value,
        },
      },
    })

    if (response?.data?.sendTestEmail === true) {
      sent.value = true
      showModal.value = false

      // fire off user creation in the background
      $fetch('/graphql', {
        baseURL: server,
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: {
          query: `
            mutation CreateUser($email: String!) {
              createUser(email: $email) {
                id
              }
            }
          `,
          variables: {
            email: email.value,
          },
        },
      }).catch((err) => {
        console.error('[CreateUser background error]', err)
      })
    } else {
      error.value = 'Something went wrong. Please try again.'
    }
  } catch (err) {
    console.error('[Email Error]', err)
    error.value = 'Failed to send email. Try again later.'
  } finally {
    isSending.value = false
  }
}
</script>
<template>
    <section class="relative min-h-screen flex items-center justify-center text-white overflow-hidden">
        <video
  ref="bgVideo"
  autoplay
  muted
  loop
  playsinline
  class="absolute inset-0 w-full h-full object-cover"
>
  <source src="/rain.mp4" type="video/mp4" />
</video>



<div class="absolute inset-0 bg-opacity-20"></div>
<!-- Mute/Unmute Button -->
<button
   @click="toggleMute"
   class="absolute top-4 right-4 z-20 bg-black/40 text-white px-3 py-1 rounded cursor-pointer text-sm"
 >
   {{ isMuted ? 'Unmute' : 'Mute' }}
 </button>

  <div class="relative z-10 max-w-2xl mx-auto text-center px-6 py-12">
    <h1 class="text-4xl font-bold tracking-tight sm:text-5xl">
      Welcome to PeacePad
    </h1>
    <p class="mt-4 text-lg text-gray-100">
      Helping students and international renters find peaceful, noise-free homes.
    </p>

    <div v-if="!sent" class="mt-8 flex flex-col items-center space-y-3">
  <div class="flex flex-col sm:flex-row gap-3">
    <button
      @click="showModal = true"
      class="px-6 py-2 rounded-md bg-blue-600 hover:bg-blue-700 text-white font-medium transition cursor-pointer"
    >
      Stay in Touch
    </button>

    <NuxtLink
      to="/signup"
      class="px-6 py-2 rounded-md bg-white text-blue-600 hover:bg-gray-100 font-medium transition cursor-pointer border border-blue-600"
    >
      Create Account
    </NuxtLink>
  </div>

  <button
    @click="scrollToContent"
    class="text-white hover:text-blue-200 underline text-sm cursor-pointer mt-2"
  >
    Learn More
  </button>
</div>



    <div class="mt-4 h-5 text-sm">
      <transition name="fade">
        <p v-if="error" class="text-red-300">{{ error }}</p>
      </transition>
    </div>

    <transition name="fade">
      <div v-if="sent" class="mt-10 p-4 rounded-md bg-green-100 text-green-800 font-medium">
        {{ successMessage }}
      </div>
    </transition>
  </div>
</section>


    


  
    <div v-if="showModal" class="fixed inset-0 z-50 backdrop-blur-sm bg-black/20 flex items-center justify-center">
        <div class="bg-white p-6 rounded-2xl shadow-2xl max-w-md w-full border border-gray-200">
        <h2 class="text-xl font-semibold mb-4">Stay in Touch</h2>
        <input
          v-model="email"
          type="email"
          placeholder="Email *"
          class="w-full mb-3 px-4 py-2 border border-gray-300 rounded"
        />
        <input
          v-model="phone"
          type="tel"
          placeholder="Phone (optional)"
          class="w-full mb-3 px-4 py-2 border border-gray-300 rounded"
        />
        <textarea
          v-model="message"
          placeholder="Message (optional)"
          class="w-full mb-4 px-4 py-2 border border-gray-300 rounded"
        ></textarea>
  
        <div class="flex justify-end gap-2">
          <button @click="showModal = false" class="cursor-pointer px-4 py-2 text-gray-600">Cancel</button>
          <button
            @click="handleSubmit"
            :disabled="isSending"
            class="cursor-pointer px-4 py-2 rounded bg-blue-600 hover:bg-blue-700 text-white"
          >
            <span v-if="!isSending">Send</span>
            <span v-else>Sending...</span>
          </button>
        </div>
      </div>
    </div>

    <section ref="contentSection" class="bg-white py-20 px-6 text-center border-t border-gray-200">
  <div class="max-w-4xl mx-auto">
    <h2 class="text-3xl font-bold text-gray-800 mb-6">What is PeacePad?</h2>
    <p class="text-gray-600 text-lg">
      PeacePad helps students and international renters find quiet, reliable living spaces with verified low-noise environments.
    </p>

    <div class="mt-12 grid grid-cols-1 sm:grid-cols-3 gap-6 text-left">
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Noise Scoring</h3>
        <p class="text-gray-600">Each listing is acoustically profiled for quietness and soundproofing.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Verified Properties</h3>
        <p class="text-gray-600">We work with landlords to guarantee a peaceful environment.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Tailored For You</h3>
        <p class="text-gray-600">International student? Sensitive to noise? We match based on your needs.</p>
      </div>
    </div>
  </div>
</section>

<section class="bg-gray-50 py-20 px-6 text-center border-t border-gray-200">
  <div class="max-w-3xl mx-auto">
    <h2 class="text-3xl font-bold text-gray-800 mb-6">Why Quiet Matters</h2>
    <p class="text-gray-600 text-lg mb-8">
      Noise isn't just annoying — it's one of the top reasons people move out. Your peace of mind shouldn't be a luxury.
    </p>

    <div class="grid grid-cols-1 sm:grid-cols-3 gap-8 text-left">
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Sleep & Focus</h3>
        <p class="text-gray-600">Noise disrupts sleep and study, two essentials for students and remote workers alike.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Mental Wellness</h3>
        <p class="text-gray-600">A quiet environment reduces stress, anxiety, and improves emotional stability.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Long-Term Comfort</h3>
        <p class="text-gray-600">Quiet homes are more livable and sustainable over time — no more surprise headaches.</p>
      </div>
    </div>
  </div>
</section>


<section class="bg-gray-50 py-20 px-6 text-center border-t border-gray-200">
  <div class="max-w-4xl mx-auto">
    <h2 class="text-3xl font-bold text-gray-800 mb-6">How PeacePad Works</h2>
    <p class="text-gray-600 text-lg mb-10">
      We combine acoustic insight with personal preferences to help you find the most peaceful place to live.
    </p>

    <div class="grid grid-cols-1 sm:grid-cols-3 gap-8 text-left">
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Noise Scoring</h3>
        <p class="text-gray-600">
          Every property is evaluated using a mix of data, user feedback, and on-site assessments to score sound levels.
        </p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Verified Partners</h3>
        <p class="text-gray-600">
          We work with property owners and landlords who commit to quiet-first environments and tenant respect.
        </p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Personal Matching</h3>
        <p class="text-gray-600">
          You tell us what matters — sensitivity to noise, study hours, sleep schedule — and we tailor the search to you.
        </p>
      </div>
    </div>
  </div>
</section>

<section class="bg-white py-20 px-6 text-center border-t border-gray-200">
  <div class="max-w-4xl mx-auto">
    <h2 class="text-3xl font-bold text-gray-800 mb-6">Who It's For</h2>
    <p class="text-gray-600 text-lg mb-10">
      PeacePad is designed for anyone who values calm, comfort, and control over their space.
    </p>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8 text-left">
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Light Sleepers</h3>
        <p class="text-gray-600">If a creaky floor or late-night neighbors ruin your rest, you’ll feel at home here.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">International Students</h3>
        <p class="text-gray-600">We help you navigate unfamiliar rental markets and avoid chaotic neighborhoods.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Remote Workers</h3>
        <p class="text-gray-600">Quiet environments make deep focus and long meetings actually doable.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Neurodivergent Individuals</h3>
        <p class="text-gray-600">Noise sensitivity isn’t a preference — it’s a need. We get it, and we build for you.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Grad Students</h3>
        <p class="text-gray-600">Your thesis doesn’t need a roommate's band practice in the background.</p>
      </div>
      <div>
        <h3 class="font-semibold text-gray-800 mb-2">Anyone Who’s Had a Noisy Apartment</h3>
        <p class="text-gray-600">Once you’ve had a bad experience, you know peace is worth prioritizing.</p>
      </div>
    </div>
  </div>
</section>


<section class="bg-blue-50 py-20 px-6 text-center border-t border-gray-200">
  <div class="max-w-2xl mx-auto">
    <h2 class="text-3xl font-bold text-gray-800 mb-4">Let’s Find Your Quiet</h2>
    <p class="text-gray-600 text-lg mb-8">
      Ready to start your search or just want to talk? Share your info and we’ll be in touch.
    </p>
    <div v-if="!sent" class="mt-8 flex flex-col items-center space-y-3">
  <div class="flex flex-col sm:flex-row gap-3">
    <button
      @click="showModal = true"
      class="px-6 py-2 rounded-md bg-blue-600 hover:bg-blue-700 text-white font-medium transition cursor-pointer"
    >
      Stay in Touch
    </button>

    <NuxtLink
      to="/signup"
      class="px-6 py-2 rounded-md bg-white text-blue-600 hover:bg-gray-100 font-medium transition cursor-pointer border border-blue-600"
    >
      Create Account
    </NuxtLink>
  </div>

  <button
    @click="scrollToContent"
    class="text-white hover:text-blue-200 underline text-sm cursor-pointer mt-2"
  >
    Learn More
  </button>
</div>
  </div>
</section>


  </template>
  
<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
