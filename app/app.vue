<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { Ref } from 'vue'

// Набор вдохновляющих цитат
const quotes: string[] = [
  'Ты невероятная, и я всегда буду поддерживать тебя. 😊',
  'Помни, что после дождя всегда появляется радуга. 🌈',
  'Каждый твой шаг, даже самый маленький — это уже прогресс. 🚀',
  'Ты сильнее, чем ты думаешь. Не сдавайся!',
  'Отдыхай. Тебе это нужно. Ты это заслужила. ✨',
  'Мир ярче, потому что в нём есть ты. ☀️',
  'Твои мечты важны. Следуй за ними.',
  'Не бойся ошибаться. Ошибки — это ступени к успеху.',
]

// Явно указываем тип Ref<string>
const currentQuote: Ref<string> = ref('')

// Функция для получения новой случайной цитаты
const showNewQuote = () => {
  // Добавим проверку на случай, если массив пуст
  if (quotes.length === 0) return;

  let newQuote: string;
  do {
    // Используем non-null assertion (`!`), чтобы TypeScript не сомневался в типе
    newQuote = quotes[Math.floor(Math.random() * quotes.length)]!;
  } while (newQuote === currentQuote.value && quotes.length > 1) // Убедимся, что новая цитата не повторяет старую

  currentQuote.value = newQuote;
}

// Устанавливаем первую цитату при загрузке компонента
onMounted(() => {
  // Проверяем наличие элементов и используем non-null assertion (`!`)
  if (quotes.length > 0) {
    currentQuote.value = quotes[0]!;
  }
})
</script>

<template>
  <div class="relative min-h-screen overflow-hidden bg-gray-900 flex items-center justify-center p-4 font-sans">
    <!-- Анимированный фон с "плавающими" кругами -->
    <div class="absolute top-0 left-0 w-full h-full">
      <div class="circle -top-40 -left-40 bg-pink-500"></div>
      <div class="circle -bottom-40 -right-40 bg-teal-400"></div>
      <div class="circle -bottom-60 left-20 bg-purple-500"></div>
    </div>

    <!-- "Стеклянная" карточка с эффектом размытия -->
    <div
        class="relative bg-white/10 backdrop-blur-xl rounded-2xl shadow-lg p-8 max-w-md w-full text-center border border-white/20 transform hover:scale-105 transition-transform duration-500"
    >
      <h1 class="text-3xl font-bold text-white mb-4">✨ Маленькое напоминание ✨</h1>

      <!-- Контейнер для цитаты с анимацией смены текста -->
      <div class="h-24 flex items-center justify-center">
        <Transition name="fade" mode="out-in">
          <p :key="currentQuote" class="text-white/90 text-xl italic">{{ currentQuote }}</p>
        </Transition>
      </div>

      <button
          @click="showNewQuote"
          class="mt-6 bg-white/10 text-white font-semibold py-3 px-6 rounded-lg border border-white/30 shadow-md hover:bg-white/20 transform hover:-translate-y-1 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-900 focus:ring-white"
      >
        Получить поддержку
      </button>
    </div>
  </div>
</template>

<style scoped>
/* Стили для анимированных кругов на фоне */
.circle {
  position: absolute;
  border-radius: 50%;
  filter: blur(120px); /* Сильное размытие для мягкости */
  animation: float 20s infinite ease-in-out;
}
.circle:nth-child(1) {
  width: 300px;
  height: 300px;
  animation-delay: 0s;
}
.circle:nth-child(2) {
  width: 400px;
  height: 400px;
  animation-delay: 5s;
  animation-duration: 25s;
}
.circle:nth-child(3) {
  width: 250px;
  height: 250px;
  animation-delay: 10s;
  animation-duration: 18s;
}

/* Анимация "плавания" для кругов */
@keyframes float {
  0%,
  100% {
    transform: translateY(0) translateX(0);
  }
  50% {
    transform: translateY(-60px) translateX(30px) scale(1.1);
  }
}

/* Анимация плавного появления/исчезновения текста цитаты */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>