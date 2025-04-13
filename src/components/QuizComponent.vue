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
    title: "Personality & Work Preferences",
    items: [
      {
        q: "Do you prefer working independently or in a team?",
        options: ["Independently", "In a team", "A mix of both"]
      },
      {
        q: "How do you handle pressure and deadlines?",
        options: ["I thrive under pressure", "I prefer to work at a steady pace", "I get overwhelmed easily"]
      },
      {
        q: "What best describes your decision-making style?",
        options: ["Logical and analytical", "Intuitive and emotional", "Creative and out-of-the-box"]
      },
      {
        q: "How do you feel about routine tasks?",
        options: ["I prefer variety and new challenges", "I don't mind some routine", "I like structured and predictable work"]
      },
      {
        q: "How do you feel about risk-taking?",
        options: ["I take calculated risks", "I avoid risks", "I enjoy taking risks"]
      },
      {
        q: "What motivates you the most in a job?",
        options: ["Problem-solving and challenges", "Helping others", "Creative freedom", "Stability and security"]
      },
      {
        q: "Do you prefer working with data, people, or objects?",
        options: ["Data", "People", "Objects and tools"]
      },
      {
        q: "How important is financial stability in your career choice?",
        options: ["Very important", "Somewhat important", "Not a priority"]
      },
      {
        q: "Do you enjoy leading and making decisions for a group?",
        options: ["Yes, I like being in charge", "I prefer collaboration without leading", "I don't like leading"]
      },
      {
        q: "How do you like to express yourself?",
        options: ["Through writing or speaking", "Through visual or artistic means", "Through logical problem-solving"]
      }
    ]
  },
  {
    title: "Thinking Style & Problem-Solving",
    items: [
      {
        q: "What type of problems do you enjoy solving the most?",
        options: ["Logical and mathematical puzzles", "Social and ethical issues", "Hands-on mechanical or technical problems"]
      },
      {
        q: "Are you more of a big-picture thinker or detail-oriented?",
        options: ["Big-picture", "Detail-oriented", "A mix of both"]
      },
      {
        q: "How do you approach learning something new?",
        options: ["Research and analyze deeply", "Learn by doing and experimenting", "Ask for guidance from others"]
      },
      {
        q: "Do you prefer structured rules or flexibility in your work?",
        options: ["I like clear rules and guidelines", "I prefer flexibility and open-ended work", "A mix of both"]
      },
      {
        q: "What is your strongest cognitive skill?",
        options: ["Logical reasoning", "Creativity and innovation", "Emotional intelligence and communication"]
      }
    ]
  },
  {
    title: "Interests & Passion Areas",
    items: [
      {
        q: "Which of these activities excites you the most?",
        options: ["Coding and problem-solving", "Designing and creating new things", "Helping people through advice or action"]
      },
      {
        q: "How do you prefer to spend your free time?",
        options: ["Reading about new technologies or science", "Creating art, music, or stories", "Talking to people and socializing"]
      },
      {
        q: "Which of these work environments would you enjoy the most?",
        options: ["A lab or office with computers and data", "A dynamic workplace with people interaction", "A creative studio or outdoor setting"]
      },
      {
        q: "If you could work on anything for a year, what would it be?",
        options: ["Developing a new AI technology", "Writing a book or making a movie", "Running a social impact project"]
      },
      {
        q: "Do you enjoy working with your hands?",
        options: ["Yes, I love hands-on work", "Somewhat, but I prefer thinking-based tasks", "Not really, I prefer working on a computer"]
      }
    ]
  },
  {
    title: "Future Vision & Career Values",
    items: [
      {
        q: "What kind of legacy do you want to leave behind?",
        options: ["A groundbreaking innovation or discovery", "A lasting impact on people's lives", "A creative masterpiece"]
      },
      {
        q: "If money were not a factor, what career would you choose?",
        options: ["Tech entrepreneur or scientist", "Artist, writer, or filmmaker", "Doctor, teacher, or humanitarian worker"]
      },
      {
        q: "How important is work-life balance for you?",
        options: ["I am willing to work long hours for success", "I want a balance between work and life", "I prefer a relaxed and low-stress career"]
      },
      {
        q: "Do you prefer working with cutting-edge technology?",
        options: ["Yes, I love being at the forefront of innovation", "I like technology, but I prefer human interactions", "I don't care much about tech"]
      },
      {
        q: "Which best describes your ideal career path?",
        options: ["Climbing the corporate ladder or running a business", "A stable, well-paying job with clear growth", "A career based on passion and creativity"]
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
</script>

<template>
  <div class="quiz-container">
    <div v-if="!emailSubmitted" class="email-gate">
      <h2>Welcome to the Career Assessment</h2>
      <input v-model="userEmail" type="email" placeholder="Enter your email to begin" />
      <button @click="submitEmail">Start Quiz</button>
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
                <div class="options">
                  <label v-for="(opt, oIndex) in q.options" :key="oIndex"
                    :class="{ selected: answers[currentSection * 5 + qIndex] === oIndex }">
                    <input type="radio" :name="`q-${currentSection}-${qIndex}`" :value="oIndex"
                      v-model="answers[currentSection * 5 + qIndex]">
                    <span class="radio-custom"></span>
                    <span class="option-text">{{ opt }}</span>
                  </label>
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
  transition: all 0.3s ease;
  border-left: 4px solid transparent;
}

.section-item:hover {
  background: rgba(255, 255, 255, 0.05);
}

.section-item.active {
  background: rgba(138, 43, 226, 0.2);
  border-left-color: #8a2be2;
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
  background: rgba(255, 255, 255, 0.05);
  padding: 2rem;
  margin: 1.5rem 0;
  border-radius: 1rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.question-header {
  margin-bottom: 1.5rem;
}

.question-number {
  display: block;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--neon-purple);
  margin-bottom: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.question h3 {
  font-family: 'Rajdhani', sans-serif;
  font-size: 1.4rem;
  line-height: 1.4;
  margin: 0;
  color: white;
}

.options {
  display: grid;
  gap: 1.2rem;
}

.options label {
  position: relative;
  padding: 1.5rem 2rem 1.5rem 4rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 0.75rem;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  overflow: hidden;
  line-height: 1.4;
}

.options label:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateX(8px);
  border-color: rgba(255, 0, 255, 0.3);
  box-shadow: 0 0 15px rgba(138, 43, 226, 0.2);
}

.options label.selected {
  background: rgba(138, 43, 226, 0.2);
  border-color: var(--neon-purple);
  box-shadow:
    0 0 20px rgba(138, 43, 226, 0.3),
    inset 0 0 10px rgba(255, 0, 255, 0.2);
}

.radio-custom {
  position: absolute;
  left: 1.25rem;
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
}

.options label.selected .radio-custom {
  border-color: #8a2be2;
  background: #8a2be2;
  box-shadow: 0 0 10px rgba(138, 43, 226, 0.5);
}

.options label.selected .radio-custom::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 8px;
  height: 8px;
  background: white;
  border-radius: 50%;
}

input[type="radio"] {
  display: none;
}

.option-text {
  font-family: 'Inter', sans-serif;
  font-size: 1.1rem;
}

.nav-button {
  background: rgba(255, 255, 255, 0.1);
  border: 2px solid var(--neon-purple);
  padding: 1rem 2.5rem;
  border-radius: 0.75rem;
  color: white;
  font-family: 'Rajdhani', sans-serif;
  font-weight: 600;
  font-size: 1.1rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  transition: all 0.3s ease;
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
  background: var(--primary-gradient);
  transform: translateY(-2px);
  box-shadow: 0 0 20px var(--glow-color);
}

.nav-button.submit {
  background: var(--primary-gradient);
}

.email-gate {
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, rgba(255, 0, 255, 0.1), rgba(138, 43, 226, 0.1));
}

.email-gate h2 {
  font-family: 'Orbitron', sans-serif;
  font-size: 3.5rem;
  font-weight: 700;
  margin-bottom: 2rem;
  background: linear-gradient(135deg, #ff00ff, #8a2be2);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  text-shadow: 0 0 20px var(--glow-color);
  animation: pulseText 2s infinite;
}

.email-gate input {
  width: 100%;
  max-width: 400px;
  padding: 1rem;
  margin: 1rem 0;
  border: 2px solid rgba(138, 43, 226, 0.3);
  border-radius: 0.75rem;
  font-size: 1rem;
  background: rgba(255, 255, 255, 0.05);
  color: white;
  transition: all 0.3s ease;
}

.email-gate input:focus {
  border-color: #8a2be2;
  outline: none;
  box-shadow: 0 0 20px rgba(138, 43, 226, 0.2);
}

.email-gate button {
  margin-top: 2rem;
  padding: 1rem 3rem;
  font-size: 1.2rem;
  font-family: 'Rajdhani', sans-serif;
  font-weight: 600;
  background: linear-gradient(135deg, #ff00ff, #8a2be2);
  color: white;
  border: none;
  border-radius: 0.75rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 0 20px var(--glow-color);
}

.email-gate button:hover {
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 0 30px var(--glow-color);
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
  color: white;
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
  transition: all 0.3s ease;
}

.fade-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.fade-leave-to {
  opacity: 0;
  transform: translateX(-30px);
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
