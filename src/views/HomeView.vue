<script setup lang="ts">
import { RouterLink } from 'vue-router'
import { ref, onMounted } from 'vue'

const currentYear = new Date().getFullYear()

const isFAQOpen = ref<Record<number, boolean>>({})

const toggleFAQ = (index: number) => {
  isFAQOpen.value[index] = !isFAQOpen.value[index]
}

const faqs = [
  {
    question: "Is ApplySure free?",
    answer: "Yes. ApplySure is completely free to use with no subscription or sign-up required."
  },
  {
    question: "Does ApplySure store my resume on a server?",
    answer: "No. Your resume is stored only on your own device using Chrome's built-in local storage. It is never uploaded to any external server by us."
  },
  {
    question: "Which job sites does it work on?",
    answer: "Currently LinkedIn and Naukri. More platforms may be added in future updates."
  },
  {
    question: "Does it work on job search listing pages?",
    answer: "No. Navigate to a specific individual job posting page (not a search results list) and then click Analyze Match."
  },
  {
    question: "How do I delete my saved resume?",
    answer: "Click \"Replace Resume\" or \"Clear Results\" inside the extension panel at any time."
  }
]

// 3-Stage Match Score Animation Logic
const currentScore = ref(0)
const scoreStage = ref(0) // 0: scanning, 1: weak, 2: better, 3: excellent
const scoreColorClass = ref('text-slate-400')
const strokeColorClass = ref('text-slate-200')
const scoreLabel = ref('Scanning...')

const animateScore = () => {
  // Stage 0: Reset & Initial Scan
  currentScore.value = 0
  scoreStage.value = 0
  scoreLabel.value = 'Scanning Resume...'
  scoreColorClass.value = 'text-slate-400'
  strokeColorClass.value = 'text-slate-100'
  
  // Stage 1: Worst (25%)
  setTimeout(() => {
    currentScore.value = 25
    scoreStage.value = 1
    scoreColorClass.value = 'text-red-500'
    strokeColorClass.value = 'text-red-500'
    scoreLabel.value = 'Low Match'
  }, 2000)

  // Stage 2: Better (60%)
  setTimeout(() => {
    currentScore.value = 60
    scoreStage.value = 2
    scoreColorClass.value = 'text-yellow-500'
    strokeColorClass.value = 'text-yellow-500'
    scoreLabel.value = 'Good Potential'
  }, 4500)

  // Stage 3: Excellent (90%)
  setTimeout(() => {
    currentScore.value = 90
    scoreStage.value = 3
    scoreColorClass.value = 'text-emerald-500'
    strokeColorClass.value = 'text-emerald-500'
    scoreLabel.value = 'Perfect Match!'
  }, 7000)

  // Loop back to start after a delay
  setTimeout(() => {
    animateScore()
  }, 10500)
}

onMounted(() => {
  animateScore()
})

const getDashArray = () => {
  return `${currentScore.value}, 100` // 100 is max, based on the SVG path length
}

</script>

<template>
  <div class="relative min-h-screen bg-slate-50 text-slate-900 font-sans overflow-x-hidden">
    <!-- Background Blobs (Recreated with Tailwind) -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none z-0">
      <div class="absolute rounded-full blur-[80px] opacity-20 animate-[float_10s_infinite_alternate_ease-in-out] -top-10 -left-10 w-[50vw] h-[50vw] bg-[#3399f5]"></div>
      <div class="absolute rounded-full blur-[80px] opacity-10 animate-[float_10s_infinite_alternate_ease-in-out] top-[20%] -right-[20%] w-[60vw] h-[60vw] bg-[#143e80] animation-delay-[-5s]"></div>
      <div class="absolute rounded-full blur-[80px] opacity-20 animate-[float_10s_infinite_alternate_ease-in-out] -bottom-[20%] left-[10%] w-[40vw] h-[40vw] bg-[#3399f5] animation-delay-[-2s]"></div>
    </div>

    <!-- Navbar -->
    <header class="fixed top-0 w-full z-50 bg-white/70 backdrop-blur-md border-b border-white/50 shadow-sm">
      <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
        <div class="flex items-center gap-3">
          <!-- Logo increased to h-16 as requested -->
          <img src="/logo.png" alt="ApplySure Logo" class="h-16 w-auto object-contain" />
          <span class="text-3xl font-extrabold tracking-tight bg-gradient-to-r from-[#3399f5] to-[#143e80] bg-clip-text text-transparent">ApplySure</span>
        </div>
        <nav class="hidden md:flex items-center gap-8">
          <a href="#how-it-works" class="text-slate-600 hover:text-[#143e80] font-medium transition-colors">How It Works</a>
          <a href="#features" class="text-slate-600 hover:text-[#143e80] font-medium transition-colors">Features</a>
          <a href="#faq" class="text-slate-600 hover:text-[#143e80] font-medium transition-colors">FAQ</a>
          <a href="#" class="px-5 py-2.5 rounded-full bg-gradient-to-r from-[#3399f5] to-[#143e80] text-white font-semibold flex items-center gap-2 hover:-translate-y-0.5 hover:shadow-lg hover:shadow-[#3399f5]/30 transition-all border-none">Add to Chrome</a>
        </nav>
      </div>
    </header>

    <main class="relative z-10 pt-28">
      <!-- Hero Section -->
      <section class="max-w-7xl mx-auto px-6 py-20 lg:py-32 flex flex-col lg:flex-row items-center justify-between gap-16 min-h-[90vh]">
        
        <div class="flex-1 max-w-2xl text-center lg:text-left">
          <div class="inline-flex items-center gap-2 px-4 py-2 bg-white rounded-full text-sm font-semibold text-[#143e80] shadow-sm border border-blue-50 mb-8 animate-[fadeInUp_0.8s_ease-out]">
            <span class="w-2 h-2 rounded-full bg-[#3399f5] animate-pulse"></span>
            Know Before You Apply
          </div>
          
          <h1 class="text-5xl lg:text-7xl font-extrabold leading-tight tracking-tight mb-6 animate-[fadeInUp_0.8s_ease-out_0.1s_both]">
            Stop Guessing.<br/>
            <span class="bg-gradient-to-r from-[#3399f5] to-[#143e80] bg-clip-text text-transparent">Start Knowing.</span>
          </h1>
          
          <p class="text-xl text-slate-600 mb-10 leading-relaxed max-w-3xl mx-auto lg:mx-0 animate-[fadeInUp_0.8s_ease-out_0.2s_both]">
            ApplySure analyzes your resume against any job description on LinkedIn or Naukri in seconds.
            Get your ATS match score, see exactly what skills you're missing, and generate a tailored
            job-ready resume — <span class="font-semibold text-slate-900 border-b-2 border-blue-100">all without leaving the page.</span>
          </p>
          
          <div class="flex flex-col items-center lg:items-start gap-4 animate-[fadeInUp_0.8s_ease-out_0.3s_both]">
            <a href="#" class="px-8 py-4 rounded-full bg-gradient-to-r from-[#3399f5] to-[#143e80] text-white font-bold text-lg flex items-center gap-3 hover:-translate-y-1 hover:shadow-xl hover:shadow-[#3399f5]/30 transition-all border-none group">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" class="group-hover:rotate-12 transition-transform">
                <path d="M12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2Z" fill="currentColor"/>
                <path d="M8 12L11 15L16 9" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              Add to Chrome — It's Free
            </a>
            <p class="text-sm font-medium text-slate-500">Works on LinkedIn & Naukri · No sign-up required · 100% Private</p>
          </div>
        </div>
        
        <!-- Animated Hero Visual -->
        <div class="flex-1 flex justify-center w-full lg:w-1/2 mt-12 lg:mt-0 relative animate-[fadeInUp_0.8s_ease-out_0.4s_both]">
          
          <div class="absolute inset-0 bg-gradient-to-tr from-[#3399f5] to-[#143e80] rounded-[3rem] blur-3xl opacity-30"></div>

          <!-- Main Interactive Mockup Panel -->
          <div class="relative w-full max-w-lg p-8 bg-white/70 backdrop-blur-2xl border border-white/60 shadow-2xl rounded-[2.5rem] transform hover:-translate-y-2 transition-all duration-500 overflow-hidden">
            
            <!-- Window Controls -->
            <div class="flex gap-2 mb-10">
              <span class="w-3.5 h-3.5 rounded-full bg-red-400 shadow-sm"></span>
              <span class="w-3.5 h-3.5 rounded-full bg-yellow-400 shadow-sm"></span>
              <span class="w-3.5 h-3.5 rounded-full bg-green-400 shadow-sm"></span>
            </div>

            <!-- Dynamic Body -->
            <div class="flex flex-col items-center gap-8 pb-4">
              
              <!-- Large ATS Score Circle - Now 3 Stage Animated -->
              <div class="relative w-56 h-56 flex items-center justify-center">
                
                <!-- Outer decoration -->
                <div class="absolute inset-0 rounded-full border border-slate-200 shadow-inner"></div>

                <!-- SVG Score Chart -->
                <svg viewBox="0 0 36 36" class="absolute w-full h-full transform -rotate-90">
                  <path class="text-slate-100" stroke-width="2.5" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                  <!-- Path transitioning dasharray -->
                  <path :class="strokeColorClass" class="transition-all duration-1000 ease-out" :stroke-dasharray="getDashArray()" stroke-width="2.5" stroke-linecap="round" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                </svg>
                
                <div class="flex flex-col items-center justify-center z-10 transition-colors duration-1000">
                  <span class="text-6xl font-black tracking-tighter transition-all duration-1000" :class="scoreColorClass">{{ currentScore }}%</span>
                  <span class="text-sm font-bold text-slate-500 uppercase tracking-widest mt-2 transition-all duration-500">{{ scoreLabel }}</span>
                </div>
              </div>

              <!-- Animated Data Lines representing analysis processing -->
              <div class="w-full mt-6 space-y-5 px-4">
                
                <!-- 1: Skills -->
                <div class="flex items-center gap-4">
                  <div class="w-12 h-12 rounded-2xl bg-slate-100 flex items-center justify-center text-[#143e80]">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                  </div>
                  <div class="flex-1">
                    <div class="h-3 bg-slate-100 rounded-full overflow-hidden">
                      <!-- Animates from 20% -> 35% -> 70% -> 100% -->
                      <div class="h-full bg-[#143e80] transition-all duration-1000 ease-out" 
                           :style="`width: ${scoreStage === 3 ? '100%' : (scoreStage === 2 ? '70%' : (scoreStage === 1 ? '35%' : '20%'))}`"></div>
                    </div>
                  </div>
                </div>
                
                <!-- 2: Keywords -->
                <div class="flex items-center gap-4">
                  <div class="w-12 h-12 rounded-2xl bg-slate-100 flex items-center justify-center text-[#3399f5]">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 21h10a2 2 0 002-2V9.414a1 1 0 00-.293-.707l-5.414-5.414A1 1 0 0012.586 3H7a2 2 0 00-2 2v14a2 2 0 002 2z"></path></svg>
                  </div>
                  <div class="flex-1">
                    <div class="h-3 bg-slate-100 rounded-full overflow-hidden">
                      <!-- Animates from 10% -> 25% -> 55% -> 95% -->
                      <div class="h-full bg-[#3399f5] transition-all duration-1000 ease-out" 
                           :style="`width: ${scoreStage === 3 ? '95%' : (scoreStage === 2 ? '55%' : (scoreStage === 1 ? '25%' : '10%'))}`"></div>
                    </div>
                  </div>
                </div>

                <!-- 3: Missing -->
                <div class="flex items-center gap-4">
                  <div class="w-12 h-12 rounded-2xl bg-slate-100 flex items-center justify-center text-slate-400">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path></svg>
                  </div>
                  <div class="flex-1">
                    <div class="h-3 bg-slate-100 rounded-full overflow-hidden">
                       <!-- Reduces from 75% -> 65% -> 35% -> 5% -->
                      <div class="h-full bg-slate-300 transition-all duration-1000 ease-out" 
                           :style="`width: ${scoreStage === 3 ? '5%' : (scoreStage === 2 ? '35%' : (scoreStage === 1 ? '65%' : '75%'))}`"></div>
                    </div>
                  </div>
                </div>
              </div>

            </div>
          </div>
        </div>
      </section>

      <!-- Platforms Section with Tailwind -->
      <section class="max-w-4xl mx-auto px-6 py-12 mb-12">
        <p class="text-center text-sm font-bold tracking-widest text-[#143e80] opacity-80 mb-8 uppercase">Seamlessly Integrates With Your Favorite Job Boards</p>
        <div class="flex justify-center flex-wrap gap-6">
          <div class="flex items-center gap-3 px-8 py-5 bg-white rounded-2xl shadow-sm border border-slate-100 hover:-translate-y-1 hover:shadow-md transition-all">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-8 h-8 fill-[#143e80]">
              <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
            </svg>
            <span class="font-bold text-xl text-slate-800">LinkedIn</span>
          </div>
          <div class="flex items-center gap-3 px-8 py-5 bg-white rounded-2xl shadow-sm border border-slate-100 hover:-translate-y-1 hover:shadow-md transition-all">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-8 h-8 fill-[#3399f5]">
              <path d="M20 6h-4v-2c0-1.103-.897-2-2-2h-4c-1.103 0-2 .897-2 2v2h-4c-1.103 0-2 .897-2 2v12c0 1.103.897 2 2 2h16c1.103 0 2-.897 2-2v-12c0-1.103-.897-2-2-2zm-10-2h4v2h-4v-2zm10 14h-16v-9h16v9z"/>
              <path d="M12 18c1.654 0 3-1.346 3-3s-1.346-3-3-3-3 1.346-3 3 1.346 3 3 3zm0-4c.552 0 1 .449 1 1s-.448 1-1 1-1-.449-1-1 .448-1 1-1z"/>
            </svg>
            <span class="font-bold text-xl text-slate-800 tracking-tight">Naukri</span>
          </div>
        </div>
      </section>

      <!-- How it Works Section Tailwind -->
      <section id="how-it-works" class="max-w-7xl mx-auto px-6 py-24">
        <div class="text-center mb-16">
          <h2 class="text-4xl lg:text-5xl font-extrabold tracking-tight mb-4 text-[#143e80]">How It Works</h2>
          <p class="text-xl text-slate-500">Three simple steps to the perfect pitch.</p>
        </div>
        
        <div class="flex flex-col md:flex-row justify-between gap-8 relative items-center md:items-stretch">
          
          <!-- Step 1 -->
          <div class="flex-1 max-w-sm bg-white/60 backdrop-blur-lg border border-white p-10 rounded-3xl shadow-[0_8px_30px_rgb(0,0,0,0.04)] text-center flex flex-col items-center z-10 hover:-translate-y-2 hover:shadow-xl transition-all">
            <div class="relative w-24 h-24 rounded-full bg-slate-50 flex items-center justify-center text-[#3399f5] mb-8 border border-slate-100">
              <span class="absolute -top-3 -right-2 bg-[#143e80] text-white w-8 h-8 rounded-full flex items-center justify-center font-bold border-4 border-white">1</span>
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-10 h-10"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-4 text-[#143e80]">Upload Your Resume</h3>
            <p class="text-slate-500 leading-relaxed">Upload your PDF once. ApplySure reads and saves it locally on your device. No cloud uploads, absolute privacy.</p>
          </div>
          
          <!-- Step 2 -->
          <div class="flex-1 max-w-sm bg-white/60 backdrop-blur-lg border border-white p-10 rounded-3xl shadow-[0_8px_30px_rgb(0,0,0,0.04)] text-center flex flex-col items-center z-10 hover:-translate-y-2 hover:shadow-xl transition-all">
            <div class="relative w-24 h-24 rounded-full bg-slate-50 flex items-center justify-center text-[#3399f5] mb-8 border border-slate-100">
              <span class="absolute -top-3 -right-2 bg-[#143e80] text-white w-8 h-8 rounded-full flex items-center justify-center font-bold border-4 border-white">2</span>
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-10 h-10"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-4 text-[#143e80]">Open Any Job Page</h3>
            <p class="text-slate-500 leading-relaxed">Navigate to any job listing on LinkedIn or Naukri. ApplySure automatically preps the description in the background.</p>
          </div>

          <!-- Step 3 -->
          <div class="flex-1 max-w-sm bg-white/60 backdrop-blur-lg border border-white p-10 rounded-3xl shadow-[0_8px_30px_rgb(0,0,0,0.04)] text-center flex flex-col items-center z-10 hover:-translate-y-2 hover:shadow-xl transition-all">
            <div class="relative w-24 h-24 rounded-full bg-slate-50 flex items-center justify-center text-[#3399f5] mb-8 border border-slate-100">
              <span class="absolute -top-3 -right-2 bg-[#143e80] text-white w-8 h-8 rounded-full flex items-center justify-center font-bold border-4 border-white">3</span>
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-10 h-10"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-4 text-[#143e80]">Click "Analyze Match"</h3>
            <p class="text-slate-500 leading-relaxed">Hit the side panel button. Instantly get your ATS score, missing skills, and a rewritten tailored resume.</p>
          </div>
        </div>
      </section>

      <!-- Features Section Tailwind -->
      <section id="features" class="max-w-7xl mx-auto px-6 py-24">
        <div class="text-center mb-16">
          <h2 class="text-4xl lg:text-5xl font-extrabold tracking-tight mb-4 text-[#143e80]">Everything You Need to Get Hired</h2>
          <p class="text-xl text-slate-500">Powerful AI tools directly inside your browser.</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          
          <div class="bg-white/60 backdrop-blur-md p-10 rounded-3xl border border-white shadow-sm hover:shadow-[0_10px_40px_rgba(0,0,0,0.06)] hover:-translate-y-1 transition-all group">
            <div class="w-16 h-16 rounded-2xl bg-gradient-to-br from-[#3399f5] to-[#143e80] opacity-90 flex items-center justify-center text-white mb-6 transform group-hover:rotate-6 transition-transform">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-8 h-8"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-3 text-[#143e80]">ATS Match Score</h3>
            <p class="text-slate-500 leading-relaxed">Precise percentage score showing how well your resume matches. Our AI uses strict domain-matching rules for accuracy.</p>
          </div>

          <div class="bg-white/60 backdrop-blur-md p-10 rounded-3xl border border-white shadow-sm hover:shadow-[0_10px_40px_rgba(0,0,0,0.06)] hover:-translate-y-1 transition-all group">
            <div class="w-16 h-16 rounded-2xl bg-gradient-to-br from-[#3399f5] to-[#143e80] opacity-90 flex items-center justify-center text-white mb-6 transform group-hover:rotate-6 transition-transform">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-8 h-8"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-3 text-[#143e80]">Skill Gap Analysis</h3>
            <p class="text-slate-500 leading-relaxed">See exactly which skills the job requires that you are missing. Stop wondering why you're not getting callbacks.</p>
          </div>

          <div class="bg-white/60 backdrop-blur-md p-10 rounded-3xl border border-white shadow-sm hover:shadow-[0_10px_40px_rgba(0,0,0,0.06)] hover:-translate-y-1 transition-all group">
            <div class="w-16 h-16 rounded-2xl bg-gradient-to-br from-[#3399f5] to-[#143e80] opacity-90 flex items-center justify-center text-white mb-6 transform group-hover:rotate-6 transition-transform">
               <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-8 h-8"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-3 text-[#143e80]">Job-Ready Resume</h3>
            <p class="text-slate-500 leading-relaxed">ApplySure rewrites your resume to align with the specific job — highlighting the right keywords and experience perfectly.</p>
          </div>

          <div class="bg-white/60 backdrop-blur-md p-10 rounded-3xl border border-white shadow-sm hover:shadow-[0_10px_40px_rgba(0,0,0,0.06)] hover:-translate-y-1 transition-all group">
            <div class="w-16 h-16 rounded-2xl bg-gradient-to-br from-[#3399f5] to-[#143e80] opacity-90 flex items-center justify-center text-white mb-6 transform group-hover:rotate-6 transition-transform">
               <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-8 h-8"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-3 text-[#143e80]">Easy Apply Detection</h3>
            <p class="text-slate-500 leading-relaxed">Instantly detects Easy Apply jobs on LinkedIn so you can filter and prioritize your applications effectively.</p>
          </div>

          <div class="bg-white/60 backdrop-blur-md p-10 rounded-3xl border border-white shadow-sm hover:shadow-[0_10px_40px_rgba(0,0,0,0.06)] hover:-translate-y-1 transition-all group">
            <div class="w-16 h-16 rounded-2xl bg-gradient-to-br from-[#3399f5] to-[#143e80] opacity-90 flex items-center justify-center text-white mb-6 transform group-hover:rotate-6 transition-transform">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-8 h-8"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 10h16M4 14h16M4 18h16" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-3 text-[#143e80]">Works in Your Sidebar</h3>
            <p class="text-slate-500 leading-relaxed">No tedious tab switching. The extension lives straight in Chrome's side panel. Analyze jobs while viewing them.</p>
          </div>

          <div class="bg-white/60 backdrop-blur-md p-10 rounded-3xl border border-white shadow-sm hover:shadow-[0_10px_40px_rgba(0,0,0,0.06)] hover:-translate-y-1 transition-all group">
            <div class="w-16 h-16 rounded-2xl bg-gradient-to-br from-[#3399f5] to-[#143e80] opacity-90 flex items-center justify-center text-white mb-6 transform group-hover:rotate-6 transition-transform">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-8 h-8"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" /></svg>
            </div>
            <h3 class="text-2xl font-bold mb-3 text-[#143e80]">100% Private</h3>
            <p class="text-slate-500 leading-relaxed">Your resume text is stored locally. It's never saved on a server. Job descriptions are discarded immediately after.</p>
          </div>
        </div>
      </section>

      <!-- FAQ Section Tailwind -->
      <section id="faq" class="max-w-4xl mx-auto px-6 py-24">
        <div class="text-center mb-16">
          <h2 class="text-4xl font-extrabold tracking-tight text-[#143e80]">Frequently Asked Questions</h2>
        </div>
        <div class="flex flex-col gap-4">
          <div v-for="(faq, index) in faqs" :key="index" 
               class="bg-white rounded-2xl border border-slate-200 overflow-hidden transition-all duration-300"
               :class="{ 'border-[#3399f5] shadow-md': isFAQOpen[index] }">
            <button class="w-full flex justify-between items-center p-6 text-left focus:outline-none" @click="toggleFAQ(index)">
              <span class="text-lg font-semibold text-[#143e80]">{{ faq.question }}</span>
              <div class="text-[#3399f5] transform transition-transform duration-300" :class="{ 'rotate-180': isFAQOpen[index] }">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round">
                  <polyline points="6 9 12 15 18 9"></polyline>
                </svg>
              </div>
            </button>
            <div class="px-6 transition-all duration-300 max-h-0 overflow-hidden"
                 :class="{ 'max-h-96 pb-6': isFAQOpen[index] }">
              <p class="text-slate-500 leading-relaxed">{{ faq.answer }}</p>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Big CTA Banner Tailwind -->
      <section class="max-w-5xl mx-auto px-6 py-24">
        <div class="bg-gradient-to-r from-[#3399f5] to-[#143e80] rounded-[3rem] p-16 text-center text-white relative overflow-hidden shadow-2xl">
          <div class="relative z-10 max-w-2xl mx-auto flex flex-col items-center">
            <h2 class="text-4xl md:text-5xl font-extrabold tracking-tight mb-6">Start your smarter job search today.</h2>
            <p class="text-xl text-blue-100 mb-10">Get instant insights and a tailored resume for every application.</p>
            <a href="#" class="px-8 py-4 bg-white text-[#143e80] rounded-full font-bold text-xl flex items-center gap-3 hover:scale-105 hover:bg-slate-50 transition-all shadow-xl">
              Get ApplySure for Free
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" width="24" height="24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
              </svg>
            </a>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer Tailwind -->
    <footer class="bg-slate-900 border-t border-slate-800 pt-20 pb-10">
      <div class="max-w-7xl mx-auto px-6">
        <div class="flex flex-col md:flex-row justify-between gap-12 mb-16">
          
          <div class="max-w-xs">
            <div class="flex items-center gap-3 mb-4">
               <img src="/logo.png" alt="ApplySure Logo" class="h-16 w-auto" />
               <span class="text-2xl font-extrabold text-white">ApplySure</span>
            </div>
            <p class="text-slate-400">Know Before You Apply.</p>
          </div>

          <div class="flex gap-16 flex-wrap">
            <div class="flex flex-col gap-4">
              <h4 class="text-white font-semibold text-lg">Product</h4>
              <a href="#how-it-works" class="text-slate-400 hover:text-white transition-colors">How it Works</a>
              <a href="#features" class="text-slate-400 hover:text-white transition-colors">Features</a>
              <a href="#" class="text-slate-400 hover:text-white transition-colors">Chrome Web Store</a>
            </div>
            <div class="flex flex-col gap-4">
              <h4 class="text-white font-semibold text-lg">Legal</h4>
              <RouterLink to="/privacy" class="text-slate-400 hover:text-white transition-colors">Privacy Policy</RouterLink>
            </div>
          </div>
        </div>
        
        <div class="border-t border-slate-800 pt-8 text-center text-slate-500">
          <p>&copy; {{ currentYear }} ApplySure. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>
/* Safe custom animations to use with Tailwind arbitrary values if needed */
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes float {
  0% { transform: translate(0, 0) scale(1); }
  100% { transform: translate(5%, 10%) scale(1.1); }
}
</style>
