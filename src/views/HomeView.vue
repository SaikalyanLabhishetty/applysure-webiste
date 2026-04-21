<script setup lang="ts">
import { RouterLink } from 'vue-router'
import { computed, onMounted, onUnmounted, ref } from 'vue'

const currentYear = new Date().getFullYear()
const chromeWebStoreUrl =
  'https://chromewebstore.google.com/detail/applysure/oedpnodmknpdcmhamamfmghlnhjhepkn'

const openChromeWebStore = () => {
  if (typeof window !== 'undefined') {
    window.open(chromeWebStoreUrl, '_blank', 'noopener,noreferrer')
  }
}

const navLinks = [
  { label: 'Why ApplySure', href: '#features' },
  { label: 'How It Works', href: '#how-it-works' },
  { label: 'Demo', href: '#demo' },
  { label: 'FAQ', href: '#faq' },
]

const proofPoints = [
  {
    label: 'Where it works',
    value: 'LinkedIn and Naukri',
    description: 'Open the job post, launch the side panel, and evaluate fit without breaking flow.',
  },
  {
    label: 'Privacy posture',
    value: 'Resume stays local',
    description: 'Your resume is saved inside Chrome storage on your own device, not on our servers.',
  },
  {
    label: 'What you get',
    value: 'Score, gaps, rewrite',
    description: 'Know the match, see what is missing, and generate a sharper version before applying.',
  },
  {
    label: 'Friction',
    value: 'No sign-up required',
    description: 'Install, upload once, and start reviewing jobs immediately.',
  },
]

const featureCards = [
  {
    eyebrow: 'Match confidence',
    title: 'See whether the role is worth your time before you burn an application.',
    body: 'ApplySure reads the posting, compares it against your resume, and turns vague intuition into an actual signal. That changes how you prioritize.',
    featured: true,
  },
  {
    eyebrow: 'Gap mapping',
    title: 'The missing pieces are called out clearly.',
    body: 'Required skills, tools, or experience themes are surfaced fast so you know whether to tailor, strengthen, or skip.',
    featured: false,
  },
  {
    eyebrow: 'Rewrite support',
    title: 'A tighter resume version for the exact job in front of you.',
    body: 'Reposition your experience around the role instead of sending the same generic resume everywhere.',
    featured: false,
  },
  {
    eyebrow: 'Side-panel workflow',
    title: 'You stay on the job page while the extension does the work.',
    body: 'No copy-paste loop, no bouncing between tabs, no messy process. The product feels immediate because it lives where decisions happen.',
    featured: false,
  },
]

const operatingPrinciples = [
  'Serious visual hierarchy so the product feels like a decision tool, not a toy.',
  'Clearer proof points around privacy, supported platforms, and workflow speed.',
  'A hero mockup that looks specific to ApplySure instead of a generic SaaS card.',
]

const steps = [
  {
    index: '01',
    title: 'Upload your resume once.',
    body: 'ApplySure reads your PDF and stores it locally in Chrome so the extension is ready whenever you open a job posting.',
    detail: 'Stored on-device',
  },
  {
    index: '02',
    title: 'Open a real job post on LinkedIn or Naukri.',
    body: 'The extension works on individual listing pages, where it can evaluate the description against your existing profile and experience.',
    detail: 'Built for live job pages',
  },
  {
    index: '03',
    title: 'Analyze the match and act immediately.',
    body: 'Get the score, missing signals, and a tailored rewrite so you know whether to apply, edit, or move on with confidence.',
    detail: 'From uncertainty to action',
  },
]

const faqs = [
  {
    question: 'Is ApplySure free?',
    answer: 'Yes. ApplySure is completely free to use with no subscription or sign-up required.',
  },
  {
    question: 'Does ApplySure store my resume on a server?',
    answer:
      "No. Your resume is stored only on your own device using Chrome's built-in local storage. It is never uploaded to any external server by us.",
  },
  {
    question: 'Which job sites does it work on?',
    answer: 'Currently LinkedIn and Naukri. More platforms may be added in future updates.',
  },
  {
    question: 'Does it work on search results pages?',
    answer: 'No. Open a specific job posting page and then run Analyze Match from the extension panel.',
  },
  {
    question: 'How do I delete my saved resume?',
    answer: 'Use Replace Resume or Clear Results inside the extension panel whenever you want to remove or refresh your data.',
  },
]

const isFAQOpen = ref<Record<number, boolean>>({ 0: true })

const toggleFAQ = (index: number) => {
  isFAQOpen.value[index] = !isFAQOpen.value[index]
}

const currentScore = ref(0)
const scoreStage = ref(0)
const scoreLabel = ref('Parsing role')
const scoreColorClass = ref('text-slate-200')
const strokeColorClass = ref('text-slate-600')
const scoreTimeouts: number[] = []

const stageMatrix: [number, number, number][] = [
  [18, 12, 78],
  [42, 31, 61],
  [68, 57, 34],
  [91, 94, 8],
]

const analysisRows = computed(() => {
  const currentStage: [number, number, number] = stageMatrix[scoreStage.value] ?? [91, 94, 8]
  const [requirements, keywordCoverage, missingSignals] = currentStage

  return [
    { label: 'Role requirements', value: requirements, tone: 'bg-[#7fb1ff]' },
    { label: 'Keyword coverage', value: keywordCoverage, tone: 'bg-[#d8bf8a]' },
    { label: 'Missing signals', value: missingSignals, tone: 'bg-[#53647f]' },
  ]
})

const missingSignalChips = computed(() => {
  const chipSets: string[][] = [
    ['System design', 'Product metrics', 'Stakeholder leadership'],
    ['Product metrics', 'Cross-functional leadership', 'Hiring signals'],
    ['Stakeholder storytelling', 'Growth loops'],
    ['Leadership narrative', 'Exec-ready outcomes'],
  ]

  return chipSets[scoreStage.value] ?? chipSets[chipSets.length - 1]
})

const rewriteFocus = computed(() => {
  const bulletSets: string[][] = [
    [
      'Mirror the role title and core scope in the resume header.',
      'Pull measurable outcomes higher so value shows faster.',
      'Shift generic bullets into job-specific language.',
    ],
    [
      'Lead with the operating metrics the role cares about.',
      'Make collaboration with product and engineering explicit.',
      'Surface tools and methods that map to the posting.',
    ],
    [
      'Keep only the strongest proof and trim the rest.',
      'Promote work that shows ownership.',
      'Sharpen the summary so seniority reads instantly.',
    ],
    [
      'Lock the strongest quantified wins at the top.',
      'Tune the summary to match the hiring team vocabulary.',
      'Preserve credibility while tightening for ATS readability.',
    ],
  ]

  return bulletSets[scoreStage.value] ?? bulletSets[bulletSets.length - 1]
})

const getDashArray = computed(() => `${currentScore.value}, 100`)

const clearScoreTimers = () => {
  for (const timeoutId of scoreTimeouts) {
    window.clearTimeout(timeoutId)
  }

  scoreTimeouts.length = 0
}

const queueStage = (delay: number, callback: () => void) => {
  if (typeof window === 'undefined') {
    return
  }

  const timeoutId = window.setTimeout(callback, delay)
  scoreTimeouts.push(timeoutId)
}

const animateScore = () => {
  clearScoreTimers()
  currentScore.value = 0
  scoreStage.value = 0
  scoreLabel.value = 'Parsing role'
  scoreColorClass.value = 'text-slate-200'
  strokeColorClass.value = 'text-slate-600'

  queueStage(1300, () => {
    currentScore.value = 42
    scoreStage.value = 1
    scoreLabel.value = 'Signal forming'
    scoreColorClass.value = 'text-[#d8bf8a]'
    strokeColorClass.value = 'text-[#d8bf8a]'
  })

  queueStage(3100, () => {
    currentScore.value = 68
    scoreStage.value = 2
    scoreLabel.value = 'Strong potential'
    scoreColorClass.value = 'text-[#7fb1ff]'
    strokeColorClass.value = 'text-[#7fb1ff]'
  })

  queueStage(5000, () => {
    currentScore.value = 91
    scoreStage.value = 3
    scoreLabel.value = 'Ready to apply'
    scoreColorClass.value = 'text-[#f3eee3]'
    strokeColorClass.value = 'text-[#7fb1ff]'
  })

  queueStage(8200, animateScore)
}

onMounted(() => {
  animateScore()
})

onUnmounted(() => {
  clearScoreTimers()
})
</script>

<template>
  <div class="relative min-h-screen overflow-x-hidden bg-[#07111f] text-[#f3eee3]">
    <div class="pointer-events-none absolute inset-0 overflow-hidden">
      <div class="absolute inset-0 bg-[radial-gradient(circle_at_top_left,rgba(127,177,255,0.18),transparent_28%),radial-gradient(circle_at_82%_8%,rgba(216,191,138,0.16),transparent_22%),linear-gradient(180deg,#07111f_0%,#0a1424_56%,#0d1728_100%)]"></div>
      <div class="page-grid absolute inset-0 opacity-[0.08]"></div>
      <div class="absolute -left-28 top-10 h-80 w-80 rounded-full bg-[#1f58d6]/20 blur-[120px] motion-safe:animate-[drift_18s_ease-in-out_infinite_alternate]"></div>
      <div class="absolute right-0 top-32 h-72 w-72 rounded-full bg-[#d8bf8a]/12 blur-[110px] motion-safe:animate-[drift_14s_ease-in-out_infinite_alternate-reverse]"></div>
    </div>

    <header class="fixed inset-x-0 top-0 z-50 border-b border-white/10 bg-[#07111f]/78 backdrop-blur-xl">
      <div class="mx-auto flex max-w-7xl items-center justify-between px-4 py-3 sm:px-6 sm:py-4">
        <a href="#top" class="flex items-center gap-3 sm:gap-4">
          <img src="/logo.png" alt="ApplySure Logo" class="h-10 w-auto object-contain sm:h-14" />
          <div>
            <p class="font-display text-xl tracking-tight text-white sm:text-3xl">ApplySure</p>
            <p class="hidden text-[0.65rem] font-semibold tracking-wide text-[#8fa0ba] sm:block sm:text-[0.72rem]">
              Know before you apply
            </p>
          </div>
        </a>

        <nav class="hidden items-center gap-8 lg:flex">
          <a
            v-for="link in navLinks"
            :key="link.href"
            :href="link.href"
            class="text-sm font-semibold tracking-normal text-[#a6b4c8] transition hover:text-white"
          >
            {{ link.label }}
          </a>
        </nav>

        <a
          :href="chromeWebStoreUrl"
          target="_blank"
          rel="noopener noreferrer"
          @click.prevent="openChromeWebStore"
          style="color: #07111f"
          class="hidden rounded-full border border-[#d8bf8a]/40 bg-[#f3eee3] px-5 py-3 text-sm font-bold tracking-normal text-[#07111f] transition hover:-translate-y-0.5 hover:bg-white md:inline-flex"
        >
          Add to Chrome
        </a>
      </div>
    </header>

    <main class="relative z-10">
      <section id="top" class="mx-auto max-w-5xl px-4 pb-14 pt-28 text-center sm:px-6 sm:pt-32 lg:pt-40">
        <div class="flex flex-col items-center motion-safe:animate-[liftIn_800ms_cubic-bezier(0.16,1,0.3,1)_both]">
          <div class="inline-flex max-w-full flex-wrap items-center justify-center gap-2 rounded-full border border-[#d8bf8a]/25 bg-white/5 px-3 py-2 text-center text-[0.64rem] font-semibold tracking-wide text-[#d8bf8a] sm:gap-3 sm:px-4 sm:text-[0.72rem]">
            <span class="h-2 w-2 rounded-full bg-[#d8bf8a]"></span>
            Premium decision support for job seekers
          </div>

          <h1 class="font-display mt-8 max-w-4xl text-[1.9rem] leading-[1.02] tracking-[-0.04em] text-white sm:text-6xl sm:leading-[0.92] lg:text-7xl xl:text-[5.6rem]">
            The job search should look
            <span class="text-[#d8bf8a]">deliberate</span>, not desperate.
          </h1>

          <p class="mt-8 max-w-3xl text-sm leading-6 text-[#c4cfdd] sm:text-xl sm:leading-8">
            ApplySure reads the role in front of you, scores your resume against it, highlights what is missing,
            and helps you tighten the story before you apply. It feels powerful because it works where the decision happens.
          </p>

          <div class="mt-10 flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-center">
            <a
              :href="chromeWebStoreUrl"
              target="_blank"
              rel="noopener noreferrer"
              @click.prevent="openChromeWebStore"
              style="color: #07111f"
              class="inline-flex w-full items-center justify-center gap-3 rounded-full bg-[#f3eee3] px-7 py-4 text-sm font-bold tracking-normal text-[#07111f] transition hover:-translate-y-0.5 hover:bg-white sm:w-auto"
            >
              <img src="/chrome.svg" alt="" aria-hidden="true" class="h-7 w-7 rounded-full object-cover" />
              Install the extension
            </a>
            <a
              href="#demo"
              class="inline-flex w-full items-center justify-center rounded-full border border-white/14 px-7 py-4 text-sm font-bold tracking-normal text-white transition hover:border-white/30 hover:bg-white/5 sm:w-auto"
            >
              Watch the demo
            </a>
          </div>

          <p class="mt-4 text-xs tracking-wide text-[#8fa0ba] sm:text-sm">
            Free to use. No sign-up. Resume stays on your device.
          </p>

          <div class="mt-12 grid w-full gap-4 sm:grid-cols-3">
            <div class="rounded-[24px] border border-white/10 bg-white/[0.04] p-5 text-center backdrop-blur-sm">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#8fa0ba]">Move faster</p>
              <p class="mt-3 font-display text-xl text-white sm:text-2xl">Know the fit first.</p>
            </div>
            <div class="rounded-[24px] border border-white/10 bg-white/[0.04] p-5 text-center backdrop-blur-sm">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#8fa0ba]">Avoid guesswork</p>
              <p class="mt-3 font-display text-xl text-white sm:text-2xl">See the missing signals.</p>
            </div>
            <div class="rounded-[24px] border border-white/10 bg-white/[0.04] p-5 text-center backdrop-blur-sm">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#8fa0ba]">Apply cleaner</p>
              <p class="mt-3 font-display text-xl text-white sm:text-2xl">Rewrite for the role.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 pb-20 sm:px-6 sm:pb-24">
        <div class="relative motion-safe:animate-[liftIn_1000ms_cubic-bezier(0.16,1,0.3,1)_120ms_both]">
          <div class="absolute -left-6 top-12 hidden h-40 w-40 rounded-full border border-white/10 sm:block"></div>
          <div class="absolute -right-12 bottom-16 hidden h-52 w-52 rounded-full border border-[#d8bf8a]/20 sm:block"></div>

          <div class="hero-frame relative w-full overflow-hidden rounded-[34px] border border-white/10 bg-[#0d1728]/90 p-4 shadow-[0_40px_120px_rgba(0,0,0,0.45)] sm:p-5 lg:p-6">
            <div class="scan-line absolute left-8 right-8 top-24 hidden h-px bg-gradient-to-r from-transparent via-[#7fb1ff] to-transparent opacity-60 sm:block"></div>
            <div class="flex items-center justify-between gap-3 rounded-[24px] border border-white/8 bg-white/5 px-3 py-3 sm:px-5">
              <div class="flex items-center gap-2">
                <span class="h-3 w-3 rounded-full bg-[#ff7b72]"></span>
                <span class="h-3 w-3 rounded-full bg-[#f1bf58]"></span>
                <span class="h-3 w-3 rounded-full bg-[#5fd48b]"></span>
              </div>
              <span class="rounded-full border border-[#d8bf8a]/30 px-3 py-1 text-[0.6rem] font-bold tracking-wide text-[#d8bf8a]">
                Chrome side panel
              </span>
            </div>

            <div class="mt-4 grid gap-4 lg:grid-cols-[1.03fr_0.97fr]">
              <article class="rounded-[28px] bg-[#efe6d6] p-4 text-[#17233a] sm:p-6">
                <div class="flex flex-col items-start justify-between gap-3 sm:flex-row sm:gap-4">
                  <div>
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#7a6f5a]">Job brief</p>
                    <h2 class="font-display mt-3 text-xl leading-tight sm:text-3xl">Senior Product Designer</h2>
                    <p class="mt-2 text-sm font-medium text-[#5a6473]">B2B platform · Growth and experimentation</p>
                  </div>
                  <span class="rounded-full bg-[#17233a] px-3 py-1 text-[0.64rem] font-bold tracking-wide text-white">
                    LinkedIn
                  </span>
                </div>

                <div class="mt-8 space-y-5">
                  <div>
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#7a6f5a]">Need to demonstrate</p>
                    <div class="mt-3 flex flex-wrap gap-2">
                      <span class="rounded-full border border-[#d3c3a7] px-3 py-1 text-xs font-semibold tracking-normal text-[#17233a]">System thinking</span>
                      <span class="rounded-full border border-[#d3c3a7] px-3 py-1 text-xs font-semibold tracking-normal text-[#17233a]">Metrics ownership</span>
                      <span class="rounded-full border border-[#d3c3a7] px-3 py-1 text-xs font-semibold tracking-normal text-[#17233a]">Cross-functional leadership</span>
                    </div>
                  </div>

                  <div class="space-y-3">
                    <div class="h-2 rounded-full bg-[#d9d0c0]"></div>
                    <div class="h-2 w-[88%] rounded-full bg-[#d9d0c0]"></div>
                    <div class="h-2 w-[80%] rounded-full bg-[#d9d0c0]"></div>
                    <div class="h-2 w-[67%] rounded-full bg-[#d9d0c0]"></div>
                  </div>

                  <div class="rounded-[22px] border border-[#d3c3a7] bg-white/50 p-5">
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#7a6f5a]">Hiring signal</p>
                    <p class="mt-3 text-sm leading-6 text-[#253247] sm:text-base sm:leading-7">
                      The role values clarity, execution, and measurable product impact. Your resume needs that story sooner.
                    </p>
                  </div>
                </div>
              </article>

              <article class="relative overflow-hidden rounded-[28px] border border-[#21314b] bg-[#091423] p-4 sm:p-6">
                <div class="absolute inset-x-6 top-0 h-px bg-gradient-to-r from-transparent via-[#d8bf8a] to-transparent opacity-50"></div>
                <div class="flex items-start justify-between gap-4">
                  <div>
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#8fa0ba]">ApplySure analysis</p>
                    <h2 class="font-display mt-3 text-xl leading-tight text-white sm:text-3xl">Ready to submit?</h2>
                  </div>
                  <span class="rounded-full border border-white/10 bg-white/5 px-3 py-1 text-[0.62rem] font-bold tracking-wide text-[#d8bf8a]">
                    Live
                  </span>
                </div>

                <div class="mt-8 flex flex-col gap-6 sm:flex-row sm:items-center">
                  <div class="relative flex h-32 w-32 shrink-0 items-center justify-center self-center sm:h-40 sm:w-40">
                    <svg viewBox="0 0 36 36" class="absolute h-full w-full -rotate-90">
                      <path
                        class="text-[#1f2c40]"
                        stroke-width="2.2"
                        stroke="currentColor"
                        fill="none"
                        d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"
                      />
                      <path
                        :class="strokeColorClass"
                        class="transition-all duration-1000 ease-out"
                        :stroke-dasharray="getDashArray"
                        stroke-width="2.4"
                        stroke-linecap="round"
                        stroke="currentColor"
                        fill="none"
                        d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"
                      />
                    </svg>
                    <div class="relative z-10 flex flex-col items-center text-center">
                      <span
                        class="w-[6ch] text-center text-3xl font-black tracking-[-0.08em] transition-colors duration-1000 [font-variant-numeric:tabular-nums] sm:text-5xl"
                        :class="scoreColorClass"
                      >
                        {{ currentScore }}%
                      </span>
                      <span class="mt-2 flex min-h-[2.5rem] max-w-[9rem] items-center justify-center text-center text-[0.62rem] font-semibold tracking-wide text-[#8fa0ba] sm:text-[0.68rem]">
                        {{ scoreLabel }}
                      </span>
                    </div>
                  </div>

                  <div class="flex-1 rounded-[22px] border border-white/8 bg-white/[0.03] p-4">
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#8fa0ba]">Interpretation</p>
                    <p class="mt-3 text-sm leading-6 text-[#d2dae6] sm:text-base sm:leading-7">
                      High-fit role. Tighten the leadership narrative and bring measurable outcomes forward before you apply.
                    </p>
                  </div>
                </div>

                <div class="mt-8 space-y-4">
                  <div v-for="row in analysisRows" :key="row.label">
                    <div class="mb-2 flex items-center justify-between text-[0.68rem] font-semibold tracking-wide text-[#8fa0ba]">
                      <span>{{ row.label }}</span>
                      <span class="[font-variant-numeric:tabular-nums]">{{ row.value }}%</span>
                    </div>
                    <div class="h-2 rounded-full bg-white/8">
                      <div
                        :class="row.tone"
                        class="h-full rounded-full transition-all duration-1000 ease-out"
                        :style="{ width: `${row.value}%` }"
                      ></div>
                    </div>
                  </div>
                </div>

                <div class="mt-8 grid items-stretch gap-4 sm:grid-cols-2">
                  <div class="rounded-[22px] border border-white/8 bg-white/[0.03] p-4 sm:min-h-[10.5rem]">
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#8fa0ba]">Missing signals</p>
                    <div class="mt-3 flex min-h-[4.75rem] flex-wrap content-start gap-2">
                      <span
                        v-for="chip in missingSignalChips"
                        :key="chip"
                        class="rounded-full border border-[#31435f] px-3 py-1 text-xs font-semibold tracking-normal text-[#e5e0d3]"
                      >
                        {{ chip }}
                      </span>
                    </div>
                  </div>

                  <div class="rounded-[22px] border border-white/8 bg-white/[0.03] p-4 sm:min-h-[10.5rem]">
                    <p class="text-[0.68rem] font-semibold tracking-wide text-[#8fa0ba]">Resume rewrite focus</p>
                    <ul class="mt-3 min-h-[7.75rem] space-y-2 text-sm leading-6 text-[#d2dae6]">
                      <li v-for="bullet in rewriteFocus" :key="bullet" class="flex gap-3">
                        <span class="mt-2 h-1.5 w-1.5 shrink-0 rounded-full bg-[#d8bf8a]"></span>
                        <span>{{ bullet }}</span>
                      </li>
                    </ul>
                  </div>
                </div>
              </article>
            </div>
          </div>

          <div class="floating-note relative mt-5 max-w-full rounded-[26px] border border-white/10 bg-[#f4eee1] p-5 text-[#17233a] shadow-[0_30px_80px_rgba(0,0,0,0.28)] sm:absolute sm:-bottom-8 sm:left-8 sm:mt-0 sm:max-w-xs">
            <p class="text-[0.68rem] font-semibold tracking-wide text-[#7a6f5a]">Private by design</p>
            <p class="mt-3 text-base leading-7">
              Resume storage stays inside Chrome on your own machine. The product feels premium because it respects trust.
            </p>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 pb-20 sm:px-6 sm:pb-24">
        <div class="overflow-hidden rounded-[30px] border border-white/10 bg-white/[0.04] shadow-[0_24px_80px_rgba(0,0,0,0.22)]">
          <div class="grid gap-px bg-white/10 md:grid-cols-2 xl:grid-cols-4">
            <article v-for="point in proofPoints" :key="point.label" class="bg-[#0e1828] p-6 lg:p-7">
              <p class="text-[0.7rem] font-semibold tracking-wide text-[#8fa0ba]">{{ point.label }}</p>
              <h2 class="font-display mt-4 text-xl leading-tight text-white sm:text-3xl">{{ point.value }}</h2>
              <p class="mt-4 text-sm leading-7 text-[#c4cfdd]">{{ point.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="features" class="mx-auto max-w-7xl px-4 py-16 sm:px-6 sm:py-24">
        <div class="grid gap-12 lg:grid-cols-[0.76fr_1.24fr] lg:items-start">
          <div class="lg:pr-10">
            <p class="text-[0.72rem] font-semibold tracking-wide text-[#d8bf8a]">Why it converts</p>
            <h2 class="font-display mt-6 text-2xl leading-tight tracking-[-0.03em] text-white sm:text-5xl lg:text-6xl">
              This needs to feel like a sharp instrument, not a generic AI landing page.
            </h2>
            <p class="mt-6 text-sm leading-6 text-[#c4cfdd] sm:text-lg sm:leading-8">
              The product promise is simple: help people make better application decisions faster. The page now speaks in that tone and shows the workflow with enough confidence that install becomes the obvious next step.
            </p>

            <div class="mt-10 rounded-[30px] border border-white/10 bg-white/[0.04] p-6">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#8fa0ba]">Built for decisive applicants</p>
              <ul class="mt-5 space-y-4 text-sm leading-7 text-[#d2dae6]">
                <li v-for="principle in operatingPrinciples" :key="principle" class="flex gap-3">
                  <span class="mt-2 h-1.5 w-1.5 shrink-0 rounded-full bg-[#d8bf8a]"></span>
                  <span>{{ principle }}</span>
                </li>
              </ul>
            </div>
          </div>

          <div class="grid gap-6 md:grid-cols-2">
            <article
              v-for="card in featureCards"
              :key="card.title"
              class="rounded-[30px] border border-white/10 bg-white/[0.04] p-6 shadow-[0_20px_60px_rgba(0,0,0,0.18)] transition hover:-translate-y-1 hover:bg-white/[0.06] sm:p-7"
              :class="card.featured ? 'md:col-span-2 md:grid md:grid-cols-[0.72fr_1.28fr] md:items-start md:gap-8' : ''"
            >
              <div>
                <p class="text-[0.72rem] font-semibold tracking-wide text-[#d8bf8a]">{{ card.eyebrow }}</p>
                <h3 class="font-display mt-5 text-xl leading-tight tracking-[-0.03em] text-white sm:text-[2rem]">
                  {{ card.title }}
                </h3>
              </div>
              <p class="mt-5 text-sm leading-7 text-[#c4cfdd] md:mt-0 md:self-end">
                {{ card.body }}
              </p>
            </article>
          </div>
        </div>
      </section>

      <section id="how-it-works" class="mx-auto max-w-7xl px-4 py-16 sm:px-6 sm:py-24">
        <div class="overflow-hidden rounded-[36px] border border-white/10 bg-[#0c1626] shadow-[0_28px_90px_rgba(0,0,0,0.26)]">
          <div class="grid gap-10 p-6 lg:grid-cols-[0.72fr_1.28fr] lg:p-10 xl:p-12">
            <div class="lg:pr-10">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#d8bf8a]">How it works</p>
              <h2 class="font-display mt-6 text-2xl leading-tight tracking-[-0.03em] text-white sm:text-5xl">
                Three moves. No clutter.
              </h2>
              <p class="mt-6 text-sm leading-6 text-[#c4cfdd] sm:text-lg sm:leading-8">
                The best product pages make the workflow obvious. ApplySure only needs three steps, and each one removes friction from the job search instead of adding more.
              </p>
            </div>

            <div class="space-y-4">
              <article
                v-for="step in steps"
                :key="step.index"
                class="rounded-[28px] border border-white/10 bg-white/[0.04] p-6 transition hover:border-white/20"
              >
                <div class="grid gap-5 md:grid-cols-[96px_1fr] md:items-start">
                  <div>
                    <p class="font-display text-4xl leading-none text-[#d8bf8a] sm:text-5xl">{{ step.index }}</p>
                    <p class="mt-4 inline-flex rounded-full border border-white/10 px-3 py-1 text-[0.64rem] font-bold tracking-wide text-[#8fa0ba]">
                      {{ step.detail }}
                    </p>
                  </div>
                  <div>
                    <h3 class="font-display text-xl leading-tight text-white sm:text-3xl">{{ step.title }}</h3>
                    <p class="mt-4 text-sm leading-7 text-[#c4cfdd]">{{ step.body }}</p>
                  </div>
                </div>
              </article>
            </div>
          </div>
        </div>
      </section>

      <section id="demo" class="mx-auto max-w-7xl px-4 py-16 sm:px-6 sm:py-24">
        <div class="overflow-hidden rounded-[36px] border border-white/10 bg-[#0a1423] shadow-[0_28px_90px_rgba(0,0,0,0.24)]">
          <div class="p-6 lg:p-10 xl:p-12">
            <div class="mx-auto max-w-3xl text-center">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#d8bf8a]">Demo</p>
              <h2 class="font-display mt-6 text-2xl leading-tight tracking-[-0.03em] text-white sm:text-5xl">
                Show the motion, then ask for the install.
              </h2>
              <p class="mt-6 text-sm leading-6 text-[#c4cfdd] sm:text-lg sm:leading-8">
                Seeing the extension inside the workflow lowers resistance. The visitor should already understand the value before the Chrome button appears again.
              </p>
            </div>

            <div class="mx-auto mt-12 w-full max-w-5xl rounded-[30px] border border-white/10 bg-black p-2 shadow-[0_20px_60px_rgba(0,0,0,0.24)]">
              <div class="overflow-hidden rounded-[24px] border border-white/5">
                <iframe
                  class="w-full"
                  style="aspect-ratio: 16 / 9"
                  src="https://www.youtube.com/embed/oU7qxLPs-a8"
                  title="ApplySure Demo Video"
                  loading="lazy"
                  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                  referrerpolicy="strict-origin-when-cross-origin"
                  allowfullscreen
                ></iframe>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="faq" class="relative mt-10 bg-[#f3eee3] text-[#17233a]">
        <div class="absolute inset-x-0 top-0 h-24 bg-gradient-to-b from-[#07111f] to-transparent opacity-20"></div>
        <div class="mx-auto max-w-7xl px-4 py-16 sm:px-6 sm:py-24">
          <div class="grid gap-12 lg:grid-cols-[0.82fr_1.18fr] lg:items-start">
            <div class="lg:pr-10">
              <p class="text-[0.72rem] font-semibold tracking-wide text-[#7a6f5a]">FAQ</p>
              <h2 class="font-display mt-6 text-2xl leading-tight tracking-[-0.03em] text-[#17233a] sm:text-5xl">
                Remove the last objections.
              </h2>
              <p class="mt-6 text-sm leading-6 text-[#425268] sm:text-lg sm:leading-8">
                The questions that matter most here are cost, privacy, workflow, and where the extension works. Those are answered directly.
              </p>

              <div class="light-card mt-10 rounded-[30px] border border-[#d7c9af] bg-white/70 p-6">
                <p class="text-[0.72rem] font-semibold tracking-wide text-[#7a6f5a]">Final conversion push</p>
                <p class="mt-4 text-sm leading-6 text-[#253247] sm:text-base sm:leading-7">
                  By the time someone reaches this section, the install CTA should feel low-risk and obvious. The design now supports that instead of diluting it.
                </p>
              </div>
            </div>

            <div class="space-y-4">
              <article
                v-for="(faq, index) in faqs"
                :key="faq.question"
                class="light-card overflow-hidden rounded-[28px] border border-[#d7c9af] bg-white/80 transition"
                :class="isFAQOpen[index] ? 'border-[#bca371] shadow-[0_20px_60px_rgba(16,24,40,0.08)]' : ''"
              >
                <button
                  class="flex w-full items-center justify-between gap-4 p-5 text-left sm:p-6"
                  @click="toggleFAQ(index)"
                >
                  <span class="font-display text-lg leading-tight text-[#17233a] sm:text-2xl">{{ faq.question }}</span>
                  <span
                    class="flex h-11 w-11 shrink-0 items-center justify-center rounded-full border border-[#d7c9af] text-[#17233a] transition"
                    :class="isFAQOpen[index] ? 'rotate-180 bg-[#17233a] text-white' : 'bg-white'"
                  >
                    <svg viewBox="0 0 24 24" class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="2">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M6 9l6 6 6-6" />
                    </svg>
                  </span>
                </button>
                <div class="overflow-hidden px-6 transition-all duration-300" :class="isFAQOpen[index] ? 'max-h-64 pb-6' : 'max-h-0'">
                  <p class="max-w-3xl text-sm leading-6 text-[#425268] sm:text-base sm:leading-7">{{ faq.answer }}</p>
                </div>
              </article>
            </div>
          </div>

          <div class="mt-20 overflow-hidden rounded-[38px] bg-[#0d1728] text-[#f4f0e8] shadow-[0_32px_100px_rgba(0,0,0,0.26)]">
            <div class="grid gap-10 px-6 py-10 lg:grid-cols-[1fr_auto] lg:items-center lg:px-10 lg:py-12">
              <div>
                <p class="text-[0.72rem] font-semibold tracking-wide text-[#d8bf8a]">Install ApplySure</p>
                <h2 class="font-display mt-5 text-2xl leading-tight tracking-[-0.03em] text-white sm:text-5xl">
                  If the page feels confident, the product should be one click away.
                </h2>
                <p class="mt-5 max-w-3xl text-sm leading-6 text-[#c4cfdd] sm:text-lg sm:leading-8">
                  Make the decision once. Add the extension, open a job page, and stop applying blind.
                </p>
              </div>

              <a
                :href="chromeWebStoreUrl"
                target="_blank"
                rel="noopener noreferrer"
                @click.prevent="openChromeWebStore"
                style="color: #07111f"
                class="inline-flex w-full items-center justify-center rounded-full bg-[#f3eee3] px-8 py-4 text-sm font-bold tracking-normal text-[#07111f] transition hover:-translate-y-0.5 hover:bg-white sm:w-auto"
              >
                Get it on Chrome
              </a>
            </div>
          </div>
        </div>
      </section>
    </main>

    <footer class="relative z-10 border-t border-white/10 bg-[#091220]">
      <div class="mx-auto max-w-7xl px-4 py-12 sm:px-6">
        <div class="flex flex-col gap-10 lg:flex-row lg:items-end lg:justify-between">
          <div>
            <div class="flex items-center gap-3 sm:gap-4">
              <img src="/logo.png" alt="ApplySure Logo" class="h-10 w-auto object-contain sm:h-12" />
              <div>
                <p class="font-display text-lg text-white sm:text-2xl">ApplySure</p>
                <p class="text-[0.62rem] font-semibold tracking-wide text-[#8fa0ba] sm:text-[0.68rem]">
                  Know before you apply
                </p>
              </div>
            </div>
            <p class="mt-4 max-w-md text-sm leading-7 text-[#9ca9bb]">
              Evaluate the role, see the fit, and tailor the resume without leaving the job page.
            </p>
          </div>

          <div class="flex flex-wrap gap-10 text-sm text-[#9ca9bb]">
            <div class="space-y-3">
              <p class="text-[0.68rem] font-semibold tracking-wide text-[#d8bf8a]">Explore</p>
              <a href="#features" class="block transition hover:text-white">Why ApplySure</a>
              <a href="#how-it-works" class="block transition hover:text-white">How it works</a>
              <a href="#demo" class="block transition hover:text-white">Demo</a>
            </div>
            <div class="space-y-3">
              <p class="text-[0.68rem] font-semibold tracking-wide text-[#d8bf8a]">Legal</p>
              <RouterLink to="/privacy" class="block transition hover:text-white">Privacy Policy</RouterLink>
              <a
                :href="chromeWebStoreUrl"
                target="_blank"
                rel="noopener noreferrer"
                @click.prevent="openChromeWebStore"
                class="block transition hover:text-white"
              >
                Chrome Web Store
              </a>
            </div>
          </div>
        </div>

        <div class="mt-10 border-t border-white/10 pt-6 text-sm text-[#6f7d92]">
          &copy; {{ currentYear }} ApplySure. All rights reserved.
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.font-display {
  font-family: var(--font-display);
}

.page-grid {
  background-image:
    linear-gradient(to right, rgba(255, 255, 255, 0.9) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(255, 255, 255, 0.9) 1px, transparent 1px);
  background-size: 72px 72px;
}

.hero-frame {
  backdrop-filter: blur(18px);
}

.floating-note,
.light-card {
  backdrop-filter: blur(12px);
}

.scan-line {
  animation: scan 3.8s linear infinite;
}

@keyframes liftIn {
  from {
    opacity: 0;
    transform: translateY(24px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes drift {
  from {
    transform: translate3d(0, 0, 0) scale(1);
  }

  to {
    transform: translate3d(28px, 18px, 0) scale(1.08);
  }
}

@keyframes scan {
  0% {
    transform: translateY(0);
    opacity: 0;
  }

  15% {
    opacity: 0.7;
  }

  50% {
    transform: translateY(260px);
    opacity: 0.45;
  }

  100% {
    transform: translateY(520px);
    opacity: 0;
  }
}

@media (prefers-reduced-motion: reduce) {
  .scan-line {
    animation: none;
  }
}
</style>
