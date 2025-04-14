<script setup lang="ts">
import { ref, computed } from 'vue'

const currentSection = ref(0)
const userEmail = ref('')
const emailSubmitted = ref(false)

interface Question {
  q: string
  options: string[]
}

interface Section {
  title: string
  items: Question[]
}

const questions: Section[] = [
  {
    title: "Your Work Style",
    items: [
      {
        q: "Think of the last time you worked on a school assignment or group project. What did you do most?",
        options: [
          "I organized the work and made a plan",
          "I helped explain things or encouraged others",
          "I focused on getting tasks done quickly",
          "I just did what I was told or stayed quiet"
        ]
      },
      {
        q: "You're assigned a long homework task. How do you usually react?",
        options: [
          "I finish it early so I don't forget",
          "I wait until the deadline gets close",
          "I forget about it unless I'm reminded",
          "I only really do it if it's interesting"
        ]
      },
      {
        q: "You have free time at home. What are you most likely to do?",
        options: [
          "Try a new app, tool, or code something",
          "Watch or make videos, draw, or write something",
          "Talk to friends or help someone with something",
          "Build, fix, or take apart something"
        ]
      }
    ]
  },
  {
    title: "What You Like",
    items: [
      {
        q: "Pick the class you usually enjoy the most (or find easiest):",
        options: [
          "Math or Science",
          "English, History, or Literature",
          "Art, Drama, or Music",
          "Tech class or PE"
        ]
      },
      {
        q: "Imagine a school fair is happening. Which role would you actually want?",
        options: [
          "Design the event poster or video",
          "Run a booth or guide people",
          "Handle the tech setup or sound system",
          "Come up with the idea or lead the team"
        ]
      },
      {
        q: "Which of these projects would you most likely start on your own?",
        options: [
          "Build a game, app, or website",
          "Make a short film, comic, or animation",
          "Help with a fundraiser or social cause",
          "Customize a gadget or fix something broken"
        ]
      }
    ]
  },
  {
    title: "Your Career Dealbreakers",
    items: [
      {
        q: "Think of a future job. Which of these matters most to you?",
        options: [
          "It pays well",
          "It helps others",
          "I enjoy doing it every day",
          "It gives me freedom and independence"
        ]
      },
      {
        q: "Which kind of work feels more comfortable to you?",
        options: [
          "Quiet work where I can focus alone",
          "Group work where people share ideas",
          "Moving around, hands-on tasks",
          "A mix — I don't like doing just one thing"
        ]
      }
    ]
  },
  {
    title: "How You Solve Problems",
    items: [
      {
        q: "Pick the option that sounds most like something you've done:",
        options: [
          "Used a program or website and thought, \"I could make this better\"",
          "Watched a video and thought, \"I could make something like this\"",
          "Helped a friend through a tough situation",
          "Opened up a broken item to figure out how it worked"
        ]
      },
      {
        q: "Imagine you have to present something to the class. What would you rather do?",
        options: [
          "Show a working prototype or coded thing",
          "Perform something or make a visual presentation",
          "Explain a big idea or social issue",
          "I'd rather not present — just build it and show it working"
        ]
      }
    ]
  }
]

// Calculate total number of questions
const totalQuestions = computed(() => {
  return questions.reduce((acc, section) => acc + section.items.length, 0)
})

// Initialize answers array with correct size
const answers = ref<number[]>(Array(totalQuestions.value).fill(-1))

const isComplete = computed(() => answers.value.every(a => a !== -1))

function submitEmail() {
  if (userEmail.value) {
    emailSubmitted.value = true
  }
}

// Add scroll handling
function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

// Add computed property for total sections
const totalSections = computed(() => questions.length)

// Update progress calculation
const progress = computed(() => ((currentSection.value + 1) / totalSections.value) * 100)

// Update section change validation
function changeSection(newSection: number) {
  if (newSection >= 0 && newSection < totalSections.value) {
    currentSection.value = newSection
    scrollToTop()
  }
}

function selectAnswer(sectionIndex: number, questionIndex: number, optionIndex: number) {
  console.log('Selecting answer:', { sectionIndex, questionIndex, optionIndex });
  answers.value[sectionIndex * 5 + questionIndex] = optionIndex;
}
</script>

<template>
  <div class="quiz-container">
    <div v-if="!emailSubmitted" class="email-gate">
      <div class="gate-layout">
        <div class="title-section">
          <h1>Avante</h1>
          <h2>Career Pathways</h2>
          <p class="subtitle">AI-Powered Career Assessment</p>
        </div>
        <div class="input-section">
          <input v-model="userEmail" type="email" placeholder="Your email address"
            :class="{ 'has-value': userEmail.length > 0 }" />
          <button @click="submitEmail">Start Assessment</button>
        </div>
      </div>
      <div class="decorative-elements">
        <div class="circle-glow"></div>
        <div class="corner-accent top-left"></div>
        <div class="corner-accent bottom-right"></div>
      </div>
    </div>

    <div v-else class="quiz-content">
      <aside class="section-nav">
        <div v-for="(section, index) in questions" :key="index"
          :class="['section-item', { active: currentSection === index }]" @click="changeSection(index)">
          <span class="section-number">{{ index + 1 }}</span>
          <span class="section-title">{{ section.title }}</span>
        </div>
      </aside>

      <div class="main-content">
        <div class="progress-bar">
          <div :style="{ width: `${progress}%` }" class="progress"></div>
        </div>

        <transition name="fade" mode="out-in">
          <div :key="currentSection" class="section">
            <h2>{{ questions[currentSection].title }}</h2>
            <div class="questions">
              <div v-for="(q, qIndex) in questions[currentSection].items" :key="qIndex" class="question">
                <div class="question-header">
                  <span class="question-number">Question {{ qIndex + 1 }}</span>
                  <h3>{{ q.q }}</h3>
                </div>
                <div class="options-list">
                  <div v-for="(opt, oIndex) in q.options" :key="oIndex" class="answer-choice"
                    :class="{ active: answers[currentSection * 5 + qIndex] === oIndex }"
                    @click.stop="selectAnswer(currentSection, qIndex, oIndex)">
                    <div class="circle"></div>
                    {{ opt }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </transition>

        <div class="navigation">
          <button v-if="currentSection > 0" @click="changeSection(currentSection - 1)" class="nav-button prev">
            Previous Section
          </button>
          <button v-if="currentSection < totalSections - 1" @click="changeSection(currentSection + 1)"
            class="nav-button next">
            Next Section
          </button>
          <button v-if="currentSection === totalSections - 1 && isComplete" @click="console.log('Submit')"
            class="nav-button submit">
            Submit Assessment
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;600;700&family=Rajdhani:wght@400;500;600;700&display=swap');

.quiz-container {
  width: 100%;
  min-height: 100vh;
  display: flex;
}

.quiz-content {
  display: flex;
  width: 100%;
  position: relative;
}

.section-nav {
  width: 280px;
  position: sticky;
  top: 0;
  height: 100vh;
  background: rgba(0, 0, 0, 0.9);
  padding: 2rem 0;
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  overflow-y: auto;
  background: var(--bg-darker);
  backdrop-filter: blur(10px);
}

.main-content {
  flex: 1;
  padding: 2rem 4rem;
  min-height: 100vh;
  background: rgba(18, 18, 18, 0.95);
}

.section-item {
  padding: 1rem 1.5rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 1rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border-left: 4px solid transparent;
}

.section-item:hover {
  background: rgba(255, 255, 255, 0.05);
}

.section-item.active {
  background: rgba(138, 43, 226, 0.2);
  border-left-color: var(--neon-purple);
}

.section-number {
  background: rgba(255, 255, 255, 0.1);
  width: 28px;
  height: 28px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Montserrat', sans-serif;
  font-weight: 600;
}

.section-title {
  font-family: 'Inter', sans-serif;
  font-weight: 500;
}

.main-content {
  flex: 1;
  padding: 2rem 4rem;
  margin: 0;
  width: 100%;
}

.question {
  background: rgba(255, 255, 255, 0.03);
  padding: 2rem;
  margin: 1.5rem 0;
  border-radius: 1rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.3s ease;
  position: relative;
}

.question:hover {
  transform: translateY(-4px);
  border-color: var(--neon-purple);
  background: rgba(138, 43, 226, 0.05);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

.question-header,
.options-list {
  position: relative;
  z-index: 2;
}

.answer-choice {
  display: flex;
  align-items: center;
  padding: 1.2rem 1.5rem;
  margin: 0.75rem 0;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 0.75rem;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.answer-choice:hover {
  background: rgba(255, 255, 255, 0.08);
  transform: translateX(8px);
  border-color: var(--neon-purple);
  box-shadow: 0 0 20px rgba(138, 43, 226, 0.1);
}

.answer-choice.active {
  background: rgba(138, 43, 226, 0.15);
  border-color: var(--neon-purple);
  box-shadow: 0 0 30px rgba(138, 43, 226, 0.15);
}

.circle {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  margin-right: 1.2rem;
  transition: all 0.3s ease;
  position: relative;
  flex-shrink: 0;
}

.answer-choice:hover .circle {
  border-color: rgba(138, 43, 226, 0.5);
  box-shadow: 0 0 10px rgba(138, 43, 226, 0.2);
}

.answer-choice.active .circle {
  border-color: var(--neon-purple);
  background: var(--neon-purple);
  box-shadow: 0 0 15px rgba(138, 43, 226, 0.4);
}

.answer-choice.active .circle::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 8px;
  height: 8px;
  background: #E0E0E0;
  border-radius: 50%;
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.8);
}

.text {
  color: #E0E0E0;
  font-size: 1.1rem;
}

.nav-button {
  background: rgba(255, 255, 255, 0.1);
  border: 2px solid var(--neon-purple);
  padding: 1rem 2.5rem;
  border-radius: 0.75rem;
  color: #E0E0E0;
  font-family: 'Rajdhani', sans-serif;
  font-weight: 600;
  font-size: 1.1rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.nav-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: 0.5s;
}

.nav-button:hover::before {
  left: 100%;
}

.nav-button:hover {
  transform: translateY(-4px) scale(1.02);
  box-shadow:
    0 10px 20px rgba(0, 0, 0, 0.2),
    0 0 30px var(--glow-color);
}

.nav-button.submit {
  background: var(--secondary-gradient);
  animation: float 3s infinite;
}

.email-gate {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #0a0014, #1a0033);
  overflow: hidden;
}

.gate-layout {
  position: relative;
  z-index: 2;
  display: grid;
  gap: 3rem;
  width: min(90%, 600px);
  padding: 2rem;
}

.title-section {
  text-align: center;
}

.title-section h1 {
  font-family: 'Orbitron', sans-serif;
  font-size: 5rem;
  margin: 0;
  background: linear-gradient(135deg, #ff00ff, #8a2be2);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  line-height: 1;
}

.title-section h2 {
  font-size: 2.5rem;
  margin: 0.5rem 0;
  color: #E0E0E0;
}

.subtitle {
  font-family: 'Rajdhani', sans-serif;
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.8);
  margin-top: 1rem;
}

.input-section {
  display: grid;
  gap: 1rem;
  width: min(100%, 400px);
  margin: 0 auto;
}

.input-section input {
  width: 100%;
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
  background: rgba(255, 255, 255, 0.07);
  border: 2px solid rgba(138, 43, 226, 0.3);
  border-radius: 0.75rem;
  color: #E0E0E0;
  transition: all 0.3s ease;
}

.input-section button {
  padding: 1rem;
  font-size: 1.1rem;
  font-family: 'Orbitron', sans-serif;
  background: linear-gradient(135deg, #ff00ff, #8a2be2);
  border: none;
  border-radius: 0.75rem;
  color: #E0E0E0;
  cursor: pointer;
  transition: all 0.3s ease;
}

.email-gate button {
  position: relative;
  padding: 1rem 2rem;
  font-family: 'Orbitron', sans-serif;
  font-size: 1.2rem;
  background: linear-gradient(135deg, #ff00ff, #8a2be2);
  border: none;
  border-radius: 0.75rem;
  color: #E0E0E0;
  cursor: pointer;
  transition: all 0.3s ease;
}

.email-gate button:hover {
  transform: translateY(-2px);
  box-shadow: 0 0 30px rgba(138, 43, 226, 0.4);
}

.email-gate button:active {
  transform: translateY(1px);
}

.background-effects {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.glow-circle {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 100%;
  background: radial-gradient(circle at center,
      rgba(138, 43, 226, 0.15) 0%,
      rgba(255, 0, 255, 0.1) 25%,
      transparent 70%);
}

.glow-lines {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(90deg, transparent 0%,
      rgba(138, 43, 226, 0.1) 50%,
      transparent 100%);
  opacity: 0.5;
}

.progress-bar {
  width: 100%;
  height: 8px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
  margin-bottom: 2rem;
  overflow: hidden;
}

.progress {
  height: 100%;
  background: var(--primary-gradient);
  border-radius: 4px;
  transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.section {
  animation: fadeIn 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.section h2 {
  font-family: 'Orbitron', sans-serif;
  font-size: 2.5rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 2rem;
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.navigation {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 3rem;
  padding: 1rem 0;
}

.navigation button {
  background: rgba(255, 255, 255, 0.05);
  border: none;
  padding: 1rem 2rem;
  border-radius: 0.75rem;
  color: #E0E0E0;
  font-weight: 500;
  font-family: 'Outfit', sans-serif;
  font-size: 1rem;
  transition: all 0.3s ease;
  cursor: pointer;
}

.navigation button:hover {
  background: var(--primary-gradient);
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(138, 43, 226, 0.3);
}

.navigation button.submit {
  background: var(--primary-gradient);
  box-shadow: 0 5px 15px rgba(138, 43, 226, 0.3);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.fade-enter-from {
  opacity: 0;
  transform: translateX(50px) scale(0.95);
}

.fade-leave-to {
  opacity: 0;
  transform: translateX(-50px) scale(0.95);
}

.question-number {
  display: inline-block;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--neon-purple);
  margin-bottom: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  padding: 0.4rem 0.8rem;
  background: rgba(138, 43, 226, 0.08);
  border-radius: 4px;
  border: 1px solid rgba(138, 43, 226, 0.15);
  box-shadow: 0 0 10px rgba(138, 43, 226, 0.05);
  transition: all 0.3s ease;
}

.question:hover .question-number {
  background: rgba(138, 43, 226, 0.12);
  border-color: rgba(138, 43, 226, 0.2);
  box-shadow: 0 0 15px rgba(138, 43, 226, 0.1);
}

.question h3 {
  font-family: 'Rajdhani', sans-serif;
  font-size: 1.6rem;
  line-height: 1.4;
  margin: 0;
  color: #E0E0E0;
  font-weight: 500;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes pulseText {
  0% {
    opacity: 0.8;
  }

  50% {
    opacity: 1;
  }

  100% {
    opacity: 0.8;
  }
}
</style>
