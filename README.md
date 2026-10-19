<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>중학 수학 입체도형 마스터 퀴즈</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <!-- Three.js CDN for 3D Polyhedron View -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <!-- OrbitControls CDN -->
  <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
  <!-- Canvas Confetti for Celebration -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@400;600;700;800&family=Jua&display=swap" rel="stylesheet">
  
  <style>
    body {
      font-family: 'Pretendard', sans-serif;
      user-select: none;
    }
    .title-font {
      font-family: 'Jua', sans-serif;
    }
    /* Custom Scrollbar */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #f1f5f9;
    }
    ::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 9999px;
    }
    .perspective-card {
      transition: transform 0.2s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.2s;
    }
    .perspective-card:hover {
      transform: translateY(-2px);
    }
    /* Subtle pulse animation for active buttons */
    @keyframes gentle-pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.85; }
    }
    .animate-gentle {
      animation: gentle-pulse 2s infinite ease-in-out;
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col antialiased selection:bg-indigo-100 selection:text-indigo-700">

  <!-- Top Navigation Header -->
  <header class="sticky top-0 z-40 bg-white/90 backdrop-blur-md border-b border-slate-200">
    <div class="max-w-6xl mx-auto px-4 h-16 flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-indigo-600 to-violet-500 flex items-center justify-center text-white shadow-md shadow-indigo-100">
          <i data-lucide="box" class="w-6 h-6"></i>
        </div>
        <div>
          <h1 class="text-xl font-bold text-slate-900 tracking-tight title-font flex items-center gap-2">
            입체도형 완전 정복
            <span class="text-xs font-semibold px-2 py-0.5 rounded-full bg-indigo-50 text-indigo-600 border border-indigo-200">중1 수학</span>
          </h1>
          <p class="text-xs text-slate-500">다면체 · 각뿔대 · 정다면체의 모든 성질 파헤치기</p>
        </div>
      </div>

      <!-- Quick Summary Stats -->
      <div class="flex items-center space-x-4">
        <div class="flex items-center bg-slate-100 px-3 py-1.5 rounded-lg text-sm font-semibold text-slate-700">
          <i data-lucide="award" class="w-4 h-4 mr-1.5 text-amber-500"></i>
          <span>총 정답: <span id="total-score-badge" class="text-indigo-600">0</span> / <span id="total-questions-count">21</span></span>
        </div>
        <button id="reset-all-btn" class="text-slate-400 hover:text-slate-600 p-2 rounded-lg hover:bg-slate-100 transition" title="처음부터 다시 풀기">
          <i data-lucide="rotate-ccw" class="w-4 h-4"></i>
        </button>
      </div>
    </div>
  </header>

  <main class="flex-1 max-w-6xl w-full mx-auto p-4 md:p-6 grid grid-cols-1 lg:grid-cols-12 gap-6">

    <!-- Left & Center: Quiz Section (7-8 cols) -->
    <div class="lg:col-span-7 flex flex-col space-y-4">
      
      <!-- Part Selection Tabs -->
      <nav class="grid grid-cols-3 gap-2 p-1.5 bg-slate-200/80 rounded-2xl" role="tablist">
        <button id="tab-btn-1" onclick="switchPart(1)" class="part-tab-btn flex flex-col sm:flex-row items-center justify-center gap-1.5 py-2.5 px-3 rounded-xl font-bold text-xs sm:text-sm transition-all duration-200 bg-white shadow-sm text-indigo-600">
          <span class="w-5 h-5 rounded-full bg-indigo-100 text-indigo-700 flex items-center justify-center text-xs font-bold">1</span>
          <span>다면체 기초</span>
        </button>
        <button id="tab-btn-2" onclick="switchPart(2)" class="part-tab-btn flex flex-col sm:flex-row items-center justify-center gap-1.5 py-2.5 px-3 rounded-xl font-medium text-xs sm:text-sm text-slate-600 hover:text-slate-900 transition-all duration-200">
          <span class="w-5 h-5 rounded-full bg-slate-300 text-slate-700 flex items-center justify-center text-xs font-bold">2</span>
          <span>각뿔대의 특징</span>
        </button>
        <button id="tab-btn-3" onclick="switchPart(3)" class="part-tab-btn flex flex-col sm:flex-row items-center justify-center gap-1.5 py-2.5 px-3 rounded-xl font-medium text-xs sm:text-sm text-slate-600 hover:text-slate-900 transition-all duration-200">
          <span class="w-5 h-5 rounded-full bg-slate-300 text-slate-700 flex items-center justify-center text-xs font-bold">3</span>
          <span>5대 정다면체</span>
        </button>
      </nav>

      <!-- Part Concept Card / Banner -->
      <div id="part-banner" class="bg-gradient-to-r from-indigo-500 to-indigo-700 rounded-2xl p-4 sm:p-5 text-white shadow-md relative overflow-hidden">
        <div class="relative z-10">
          <div class="flex items-center space-x-2 text-indigo-200 text-xs font-bold uppercase tracking-wider mb-1">
            <span id="banner-category-tag">파트 1 학습 목표</span>
            <span class="inline-block w-1 h-1 rounded-full bg-indigo-200"></span>
            <span id="banner-progress-text">문제 1 / 7</span>
          </div>
          <h2 id="banner-title" class="text-xl sm:text-2xl font-black title-font">다면체의 이름과 모서리·꼭짓점 개수</h2>
          <p id="banner-desc" class="text-xs sm:text-sm text-indigo-100 mt-1 max-w-md">
            입체도형의 면의 개수, 각기둥·각뿔·각뿔대의 꼭짓점(V)과 모서리(E) 공식을 정확히 이해하고 정답을 맞춰보세요!
          </p>
        </div>
        <!-- Decorative geometric shape in background -->
        <div class="absolute -right-6 -bottom-8 opacity-15 pointer-events-none">
          <i data-lucide="shapes" class="w-40 h-40"></i>
        </div>
      </div>

      <!-- Question Indicators -->
      <div class="flex items-center justify-between bg-white px-4 py-2.5 rounded-xl border border-slate-200 shadow-sm">
        <span class="text-xs font-semibold text-slate-500 flex items-center gap-1">
          <i data-lucide="help-circle" class="w-4 h-4 text-slate-400"></i>
          파트 문제 모음
        </span>
        <div class="flex items-center space-x-1.5 flex-wrap gap-y-1" id="question-indicator-container">
          <!-- Filled by JS -->
        </div>
      </div>

      <!-- Quiz Card Area -->
      <div class="bg-white rounded-2xl border border-slate-200 shadow-sm p-5 sm:p-7 flex-1 flex flex-col justify-between relative min-h-[400px]">
        <div>
          <!-- Question Header Badge -->
          <div class="flex items-center justify-between mb-3">
            <span id="question-type-badge" class="px-3 py-1 rounded-full text-xs font-bold bg-indigo-50 text-indigo-700 border border-indigo-100">
              [다면체 성질]
            </span>
            <span id="question-difficulty-stars" class="text-amber-400 text-sm flex gap-0.5">
              <i data-lucide="star" class="w-4 h-4 fill-amber-400"></i>
              <i data-lucide="star" class="w-4 h-4 fill-amber-400"></i>
              <i data-lucide="star" class="w-4 h-4 text-slate-200"></i>
            </span>
          </div>

          <!-- Main Question Text -->
          <h3 id="question-text" class="text-base sm:text-lg font-bold text-slate-900 leading-snug mb-3">
            질문이 로드되고 있습니다...
          </h3>

          <!-- Embedded Diagram Container (Shown for image/diagram questions) -->
          <div id="question-diagram-container" class="hidden mb-4 p-3 bg-slate-50/90 border border-slate-200 rounded-xl flex flex-col items-center justify-center">
            <div id="question-svg-wrapper" class="w-full flex items-center justify-center max-h-48 overflow-hidden">
              <!-- SVG diagram dynamically injected -->
            </div>
            <span id="question-diagram-caption" class="text-[11px] font-medium text-slate-500 mt-2">
              [도형 참고 그림]
            </span>
          </div>

          <!-- Multiple Choice Options Grid -->
          <div id="options-grid" class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-6">
            <!-- Dynamically injected option buttons -->
          </div>
        </div>

        <!-- Explanation & Feedback Alert Box (Hidden until answered) -->
        <div id="feedback-box" class="hidden rounded-xl p-4 text-sm transition-all duration-300">
          <div class="flex items-start space-x-3">
            <div id="feedback-icon-container" class="shrink-0 mt-0.5"></div>
            <div class="flex-1">
              <div id="feedback-title" class="font-bold text-sm"></div>
              <div id="feedback-desc" class="text-xs sm:text-sm mt-1 leading-relaxed"></div>
            </div>
          </div>
        </div>

        <!-- Card Footer: Navigation buttons -->
        <div class="flex items-center justify-between pt-4 border-t border-slate-100 mt-4">
          <button id="prev-btn" onclick="prevQuestion()" class="px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold text-slate-600 hover:bg-slate-100 border border-slate-200 transition disabled:opacity-30 disabled:pointer-events-none flex items-center gap-1">
            <i data-lucide="chevron-left" class="w-4 h-4"></i> 이전 문제
          </button>
          
          <div class="text-xs text-slate-400 font-medium" id="page-indicator">
            1 / 7
          </div>

          <button id="next-btn" onclick="nextQuestion()" class="px-4 py-2 rounded-xl text-xs sm:text-sm font-bold bg-indigo-600 text-white hover:bg-indigo-700 shadow-sm transition disabled:opacity-40 disabled:pointer-events-none flex items-center gap-1">
            다음 문제 <i data-lucide="chevron-right" class="w-4 h-4"></i>
          </button>
        </div>
      </div>

    </div>

    <!-- Right: 3D Geometry Preview & Quick Cheatsheet (4-5 cols) -->
    <div class="lg:col-span-5 flex flex-col space-y-4">

      <!-- 3D Polyhedron Interactive Canvas Container -->
      <div class="bg-white rounded-2xl border border-slate-200 shadow-sm p-4 flex flex-col overflow-hidden">
        <div class="flex items-center justify-between mb-2">
          <div class="flex items-center space-x-2">
            <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-ping"></span>
            <h4 class="font-bold text-sm text-slate-800">3D 입체도형 뷰어</h4>
          </div>
          <span class="text-[11px] font-semibold text-slate-400 flex items-center gap-1">
            <i data-lucide="mouse-pointer" class="w-3 h-3"></i> 드래그하여 360° 회전
          </span>
        </div>

        <!-- Canvas Container with aspect ratio -->
        <div class="relative w-full h-64 sm:h-72 bg-gradient-to-b from-slate-900 to-slate-800 rounded-xl overflow-hidden cursor-grab active:cursor-grabbing border border-slate-700/50">
          <div id="three-container" class="w-full h-full"></div>
          
          <!-- Shape Name Overlay -->
          <div class="absolute bottom-3 left-3 bg-slate-900/80 backdrop-blur-md px-3 py-1.5 rounded-lg border border-slate-700 text-white text-xs font-semibold flex items-center gap-2">
            <span class="w-2 h-2 rounded-full bg-indigo-400"></span>
            <span id="current-3d-name">정육면체 (Cube)</span>
          </div>

          <!-- Reset Camera Button -->
          <button onclick="reset3DCamera()" class="absolute top-3 right-3 bg-slate-800/80 hover:bg-slate-700 backdrop-blur text-slate-300 hover:text-white p-2 rounded-lg border border-slate-700 text-xs transition" title="각도 초기화">
            <i data-lucide="compass" class="w-4 h-4"></i>
          </button>
        </div>

        <!-- 3D Shape Selection Quick Toolbar -->
        <div class="mt-3 flex items-center gap-1.5 overflow-x-auto pb-1 text-xs">
          <span class="text-slate-400 shrink-0 font-medium text-[11px] mr-1">모델 변경:</span>
          <button onclick="load3DShape('tetrahedron')" class="model-quick-btn px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 font-medium text-slate-700 text-xs shrink-0 transition">정사면체</button>
          <button onclick="load3DShape('cube')" class="model-quick-btn px-2.5 py-1 rounded-lg bg-indigo-50 text-indigo-700 border border-indigo-200 font-bold text-xs shrink-0 transition">정육면체</button>
          <button onclick="load3DShape('octahedron')" class="model-quick-btn px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 font-medium text-slate-700 text-xs shrink-0 transition">정팔면체</button>
          <button onclick="load3DShape('frustum')" class="model-quick-btn px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 font-medium text-slate-700 text-xs shrink-0 transition">사각뿔대</button>
          <button onclick="load3DShape('prism')" class="model-quick-btn px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 font-medium text-slate-700 text-xs shrink-0 transition">삼각기둥</button>
        </div>
      </div>

      <!-- Middle School Polyhedron Cheatsheet Card -->
      <div class="bg-white rounded-2xl border border-slate-200 shadow-sm p-4 flex-1">
        <h4 class="font-bold text-sm text-slate-900 mb-2.5 flex items-center justify-between">
          <span class="flex items-center gap-1.5">
            <i data-lucide="book-open" class="w-4 h-4 text-indigo-600"></i>
            <span id="cheatsheet-title">파트별 핵심 공식 한눈에 보기</span>
          </span>
          <span id="cheatsheet-status-badge" class="text-[11px] font-semibold px-2 py-0.5 rounded-full bg-slate-100 text-slate-500 flex items-center gap-1">
            <i data-lucide="lock" class="w-3 h-3"></i> 잠김
          </span>
        </h4>

        <!-- Locked Overlay Container -->
        <div class="relative min-h-[170px] overflow-hidden rounded-xl">
          <!-- Content that gets blurred when locked -->
          <div id="cheatsheet-protected-area" class="transition-all duration-300 filter blur-sm pointer-events-none select-none">
            <!-- Cheatsheet dynamic body based on current part -->
            <div id="cheatsheet-content" class="text-xs text-slate-600 space-y-2.5">
              <!-- Populated by JavaScript according to currentPart -->
            </div>

            <!-- Euler's Formula Mini Callout -->
            <div class="mt-4 p-3 bg-amber-50/70 border border-amber-200/60 rounded-xl text-amber-900 text-xs flex items-center justify-between">
              <div class="flex items-center space-x-2">
                <span class="font-black text-amber-700 bg-amber-200 px-1.5 py-0.5 rounded text-[10px]">오일러 공식</span>
                <span class="font-semibold">V - E + F = 2</span>
              </div>
              <span class="text-[10px] text-amber-700 font-medium">(꼭짓점 - 모서리 + 면 = 2)</span>
            </div>
          </div>

          <!-- Lock Notice Box -->
          <div id="cheatsheet-lock-overlay" class="absolute inset-0 flex flex-col items-center justify-center p-4 text-center bg-white/70 backdrop-blur-[2px] transition-all duration-300">
            <div class="w-9 h-9 rounded-full bg-indigo-50 text-indigo-600 flex items-center justify-center mb-2 shadow-xs border border-indigo-100">
              <i data-lucide="lock" class="w-4 h-4"></i>
            </div>
            <p class="text-xs font-bold text-slate-800">핵심 공식 요약 잠김</p>
            <p id="lock-remaining-text" class="text-[11px] text-slate-500 mt-1 max-w-[220px]">
              파트의 모든 문제(7문항)를 풀면 정답과 함께 핵심 공식 표가 열립니다!
            </p>
          </div>
        </div>
      </div>

    </div>
  </main>

  <!-- Complete Modal (Hidden by default) -->
  <div id="completion-modal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4 hidden">
    <div class="bg-white rounded-3xl max-w-md w-full p-6 sm:p-8 text-center shadow-2xl transform transition-all scale-95 border border-slate-100">
      <div class="w-16 h-16 rounded-2xl bg-amber-100 text-amber-500 mx-auto flex items-center justify-center mb-4">
        <i data-lucide="trophy" class="w-9 h-9"></i>
      </div>
      <h3 class="text-2xl font-bold title-font text-slate-900 mb-1">학습 완료를 축하합니다!</h3>
      <p id="modal-score-summary" class="text-sm text-slate-500 mb-6">모든 문제를 성실하게 풀어보았습니다.</p>
      
      <div class="bg-slate-50 rounded-2xl p-4 mb-6 text-left space-y-2 text-xs sm:text-sm">
        <div class="flex justify-between items-center py-1 border-b border-slate-200">
          <span class="text-slate-600">파트 1 (다면체 기본)</span>
          <span id="score-p1" class="font-bold text-indigo-600">0 / 7</span>
        </div>
        <div class="flex justify-between items-center py-1 border-b border-slate-200">
          <span class="text-slate-600">파트 2 (각뿔대의 특징)</span>
          <span id="score-p2" class="font-bold text-indigo-600">0 / 7</span>
        </div>
        <div class="flex justify-between items-center py-1">
          <span class="text-slate-600">파트 3 (5대 정다면체)</span>
          <span id="score-p3" class="font-bold text-indigo-600">0 / 7</span>
        </div>
      </div>

      <div class="flex gap-2">
        <button onclick="closeCompletionModal()" class="flex-1 py-3 rounded-xl border border-slate-200 font-bold text-slate-700 text-sm hover:bg-slate-50 transition">
          복습하기
        </button>
        <button onclick="resetAllQuizzes()" class="flex-1 py-3 rounded-xl bg-indigo-600 text-white font-bold text-sm hover:bg-indigo-700 shadow-md transition">
          처음부터 다시
        </button>
      </div>
    </div>
  </div>

  <script>
    // ----------------------------------------------------
    // 1. DATA STRUCTURE: All 3 Parts with 7 Questions Each (Includes Diagrams & Balanced Choices)
    // ----------------------------------------------------
    const quizData = {
      // PART 1: 다면체의 이름, 모서리/꼭짓점 개수
      part1: {
        title: "다면체의 이름과 모서리·꼭짓점 개수",
        desc: "면의 개수에 따른 이름(n면체)과 각기둥·각뿔·각뿔대의 꼭짓점, 모서리 개수를 맞춰보세요.",
        modelKey: "prism",
        questions: [
          {
            id: "p1-q1",
            category: "입체도형의 면수와 이름",
            question: "오각기둥의 밑면은 2개, 옆면은 5개입니다. 이 오각기둥은 면의 총 개수에 따라 몇 면체라고 부를까요?",
            hint: "기둥의 면의 개수 = 밑면 2개 + 옆면 n개 = n + 2",
            options: [
              { text: "칠면체", isCorrect: true },
              { text: "육면체", isCorrect: false },
              { text: "팔면체", isCorrect: false },
              { text: "오면체", isCorrect: false }
            ],
            explanation: "오각기둥의 면은 밑면 2개와 옆면 5개이므로 총 7개의 면으로 이루어진 '칠면체'입니다."
          },
          {
            id: "p1-q2",
            category: "각뿔의 모서리 개수",
            question: "칠각뿔(밑면이 칠각형인 뿔)의 모서리의 개수는 모두 몇 개일까요?",
            hint: "n각뿔의 모서리 수 공식은 밑면 n개 + 옆면 모서리 n개 = 2n 입니다.",
            options: [
              { text: "7개", isCorrect: false },
              { text: "8개", isCorrect: false },
              { text: "21개", isCorrect: false },
              { text: "14개", isCorrect: true }
            ],
            explanation: "n각뿔의 모서리 개수는 2n개입니다. 따라서 칠각뿔의 모서리는 2 × 7 = 14개입니다."
          },
          {
            id: "p1-q3",
            category: "각기둥의 꼭짓점 개수",
            question: "육각기둥(밑면이 육각형인 각기둥)의 꼭짓점의 개수는 모두 몇 개일까요?",
            hint: "n각기둥의 꼭짓점은 위쪽 밑면에 n개, 아래쪽 밑면에 n개 모여 있습니다. (2n개)",
            options: [
              { text: "8개", isCorrect: false },
              { text: "18개", isCorrect: false },
              { text: "12개", isCorrect: true },
              { text: "6개", isCorrect: false }
            ],
            explanation: "n각기둥의 꼭짓점 개수는 2n개입니다. 육각기둥의 꼭짓점은 2 × 6 = 12개입니다."
          },
          {
            id: "p1-q4",
            category: "역으로 입체도형 찾기",
            question: "어떤 각뿔의 꼭짓점의 개수가 10개라고 합니다. 이 각뿔의 모서리의 개수는 몇 개일까요?",
            hint: "n각뿔의 꼭짓점 수 = n + 1 = 10이므로 n을 구한 뒤, 모서리(2n)를 계산하세요.",
            options: [
              { text: "10개", isCorrect: false },
              { text: "18개", isCorrect: true },
              { text: "20개", isCorrect: false },
              { text: "27개", isCorrect: false }
            ],
            explanation: "n각뿔의 꼭짓점은 n + 1 = 10이므로 n = 9(구각뿔)입니다. 구각뿔의 모서리 개수는 2n = 2 × 9 = 18개입니다."
          },
          {
            id: "p1-q5",
            category: "다면체 이름과 꼭짓점·모서리의 합",
            question: "사각뿔은 5개의 면으로 이루어져 있어 '오면체'입니다. 사각뿔의 꼭짓점 개수(V)와 모서리 개수(E)의 합은 얼마일까요?",
            hint: "사각뿔의 꼭짓점 = 4 + 1 = 5개, 모서리 = 4 × 2 = 8개입니다.",
            options: [
              { text: "12", isCorrect: false },
              { text: "14", isCorrect: false },
              { text: "13", isCorrect: true },
              { text: "15", isCorrect: false }
            ],
            explanation: "사각뿔은 꼭짓점 V = 5, 모서리 E = 8이므로 두 값의 합 V + E = 5 + 8 = 13입니다."
          },
          {
            id: "p1-q6",
            category: "[그림 관찰] 삼각기둥 겨냥도 분석",
            question: "아래 겨냥도로 주어진 삼각기둥의 꼭짓점의 개수(V)와 모서리의 개수(E)는 각각 몇 개일까요?",
            hint: "삼각기둥은 n=3일 때이며, 꼭짓점은 위 3개+아래 3개, 모서리는 위 3개+기둥 3개+아래 3개입니다.",
            diagramSvg: `
              <svg viewBox="0 0 240 150" class="w-48 h-32 drop-shadow-sm">
                <!-- Top Triangle -->
                <polygon points="120,20 60,60 180,60" fill="#e0e7ff" stroke="#4f46e5" stroke-width="2.5" />
                <!-- Hidden Back Edge (dashed) -->
                <line x1="120" y1="20" x2="120" y2="90" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                <line x1="60" y1="130" x2="120" y2="90" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                <line x1="180" y1="130" x2="120" y2="90" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                <!-- Side Edges -->
                <line x1="60" y1="60" x2="60" y2="130" stroke="#4f46e5" stroke-width="2.5" />
                <line x1="180" y1="60" x2="180" y2="130" stroke="#4f46e5" stroke-width="2.5" />
                <!-- Bottom Front Edges -->
                <line x1="60" y1="130" x2="180" y2="130" stroke="#4f46e5" stroke-width="2.5" />
                <!-- Front Side Faces fill tint -->
                <polygon points="60,60 180,60 180,130 60,130" fill="#6366f1" fill-opacity="0.15" />
                <!-- Vertices dots -->
                <circle cx="120" cy="20" r="4" fill="#f59e0b" />
                <circle cx="60" cy="60" r="4" fill="#f59e0b" />
                <circle cx="180" cy="60" r="4" fill="#f59e0b" />
                <circle cx="120" cy="90" r="4" fill="#f59e0b" />
                <circle cx="60" cy="130" r="4" fill="#f59e0b" />
                <circle cx="180" cy="130" r="4" fill="#f59e0b" />
              </svg>
            `,
            diagramCaption: "그림: 삼각기둥의 겨냥도 (주황색 점: 꼭짓점)",
            options: [
              { text: "꼭짓점 6개, 모서리 9개", isCorrect: true },
              { text: "꼭짓점 5개, 모서리 8개", isCorrect: false },
              { text: "꼭짓점 6개, 모서리 8개", isCorrect: false },
              { text: "꼭짓점 8개, 모서리 12개", isCorrect: false }
            ],
            explanation: "삼각기둥은 위아래 밑면에 꼭짓점이 각각 3개씩 총 6개(V=6)이고, 모서리는 밑면 3개씩 6개 + 기둥 모서리 3개로 총 9개(E=9)입니다."
          },
          {
            id: "p1-q7",
            category: "[그림 관찰] 사각뿔 겨냥도와 다면체 이름",
            question: "아래 그림의 사각뿔은 밑면 1개와 옆면 4개로 이루어져 있습니다. 이 다면체의 '면의 수에 따른 이름'과 '모서리 개수'를 바르게 짝지은 것은?",
            hint: "면이 총 5개이므로 오면체이며, 사각뿔의 모서리는 밑면 4개 + 옆면 모서리 4개입니다.",
            diagramSvg: `
              <svg viewBox="0 0 240 150" class="w-48 h-32 drop-shadow-sm">
                <!-- Apex to Base Edges -->
                <polygon points="120,20 50,110 140,130" fill="#818cf8" fill-opacity="0.25" stroke="#4f46e5" stroke-width="2.5" />
                <polygon points="120,20 140,130 190,105" fill="#6366f1" fill-opacity="0.35" stroke="#4f46e5" stroke-width="2.5" />
                <!-- Hidden Back Base Edges (dashed) -->
                <line x1="50" y1="110" x2="100" y2="85" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                <line x1="190" y1="105" x2="100" y2="85" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                <line x1="120" y1="20" x2="100" y2="85" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                <!-- Apex dot -->
                <circle cx="120" cy="20" r="4.5" fill="#f59e0b" />
                <!-- Base Vertices dots -->
                <circle cx="50" cy="110" r="3.5" fill="#f59e0b" />
                <circle cx="140" cy="130" r="3.5" fill="#f59e0b" />
                <circle cx="190" cy="105" r="3.5" fill="#f59e0b" />
                <circle cx="100" cy="85" r="3.5" fill="#f59e0b" />
              </svg>
            `,
            diagramCaption: "그림: 사각뿔의 겨냥도",
            options: [
              { text: "사면체, 모서리 6개", isCorrect: false },
              { text: "육면체, 모서리 12개", isCorrect: false },
              { text: "오면체, 모서리 10개", isCorrect: false },
              { text: "오면체, 모서리 8개", isCorrect: true }
            ],
            explanation: "사각뿔은 밑면 1개 + 옆면 4개 = 5개 면으로 둘러싸여 있으므로 '오면체'이고, 모서리는 밑면 4개와 옆면 4개로 총 8개입니다."
          }
        ]
      },

      // PART 2: 각뿔대의 밑면 모양, 이름, 옆면 모양, 면의 개수
      part2: {
        title: "각뿔대의 밑면·옆면 모양과 이름, 면의 개수",
        desc: "각뿔대를 평면으로 잘랐을 때 나오는 밑면과 옆면(사다리꼴)의 성질 및 면의 개수를 완벽히 정리해보세요.",
        modelKey: "frustum",
        questions: [
          {
            id: "p2-q1",
            category: "각뿔대의 옆면 모양",
            question: "모든 각뿔대(삼각뿔대, 사각뿔대 등)의 옆면은 항상 어떤 모양의 다각형일까요?",
            hint: "각뿔의 꼭짓점을 포함한 위쪽 부분을 평행한 평면으로 잘라내면 생기는 사각형입니다.",
            options: [
              { text: "직사각형", isCorrect: false },
              { text: "이등변삼각형", isCorrect: false },
              { text: "사다리꼴", isCorrect: true },
              { text: "평행사변형", isCorrect: false }
            ],
            explanation: "각뿔대의 옆면은 위아래 두 밑면의 변이 서로 평행하므로 항상 '사다리꼴'이 됩니다."
          },
          {
            id: "p2-q2",
            category: "밑면의 모양과 각뿔대의 이름",
            question: "두 밑면이 서로 평행하지만 크기가 다른 두 개의 오각형으로 이루어진 각뿔대의 정확한 이름은 무엇일까요?",
            hint: "각뿔대의 이름은 밑면 다각형의 이름을 따릅니다.",
            options: [
              { text: "오각기둥", isCorrect: false },
              { text: "오각뿔대", isCorrect: true },
              { text: "오각뿔", isCorrect: false },
              { text: "정오면체", isCorrect: false }
            ],
            explanation: "밑면이 오각형이고 두 밑면이 평행하지만 합동이 아니며 옆면이 사다리꼴인 입체도형은 '오각뿔대'입니다."
          },
          {
            id: "p2-q3",
            category: "각뿔대의 면의 개수와 n면체",
            question: "육각뿔대의 전체 면의 개수(F)는 몇 개이며, 따라서 몇 면체라고 부를 수 있을까요?",
            hint: "각뿔대의 면의 수 = 밑면 2개 + 옆면 n개 = n + 2개 입니다.",
            options: [
              { text: "8개 (팔면체)", isCorrect: true },
              { text: "6개 (육면체)", isCorrect: false },
              { text: "7개 (칠면체)", isCorrect: false },
              { text: "12개 (십이면체)", isCorrect: false }
            ],
            explanation: "육각뿔대의 면은 위아래 밑면 2개 + 옆면 6개 = 총 8개이므로 '팔면체'입니다."
          },
          {
            id: "p2-q4",
            category: "각뿔대의 밑면과 옆면의 성질 비교",
            question: "사각뿔대에 대한 설명 중 가장 바르지 않은 것은 무엇일까요?",
            hint: "각뿔대의 두 밑면은 평행하지만 크기(넓이)가 서로 다릅니다(합동이 아님).",
            options: [
              { text: "두 밑면은 서로 평행한 사각형이다.", isCorrect: false },
              { text: "4개의 옆면은 모두 사다리꼴이다.", isCorrect: false },
              { text: "면의 총 개수는 6개(육면체)이다.", isCorrect: false },
              { text: "두 밑면은 서로 크기가 같은 합동이다.", isCorrect: true }
            ],
            explanation: "각뿔대의 두 밑면은 평행하지만 합동이 아닙니다. 두 밑면이 합동인 입체도형은 각기둥입니다."
          },
          {
            id: "p2-q5",
            category: "면의 개수로 각뿔대 역추적",
            question: "어떤 각뿔대의 면의 총 개수가 9개라고 합니다. 이 각뿔대의 밑면의 모양과 각뿔대의 이름은 무엇일까요?",
            hint: "각뿔대의 면의 수 = n + 2 = 9이므로 밑면은 n각형입니다.",
            options: [
              { text: "구각형, 구각뿔대", isCorrect: false },
              { text: "칠각형, 칠각뿔대", isCorrect: true },
              { text: "팔각형, 팔각뿔대", isCorrect: false },
              { text: "육각형, 육각뿔대", isCorrect: false }
            ],
            explanation: "각뿔대의 면의 수는 n + 2입니다. n + 2 = 9에서 n = 7이므로 밑면은 칠각형이고 이름은 '칠각뿔대'입니다."
          },
          {
            id: "p2-q6",
            category: "[그림 관찰] 사각뿔대의 입체 구조 관찰",
            question: "아래 그림은 사각뿔을 밑면에 평행한 평면으로 잘라 만든 '사각뿔대'입니다. 색칠된 옆면(A)의 모양과 전체 면의 개수는 각각 무엇일까요?",
            hint: "각뿔대의 옆면은 윗변과 아랫변이 평행한 사각형입니다. 면은 밑면 2개 + 옆면 4개입니다.",
            diagramSvg: `
              <svg viewBox="0 0 240 160" class="w-52 h-36 drop-shadow-sm">
                <!-- Top base -->
                <polygon points="100,35 150,35 170,55 120,55" fill="#c7d2fe" stroke="#4338ca" stroke-width="2" />
                <text x="135" y="48" font-size="10" font-weight="bold" fill="#3730a3" text-anchor="middle">윗 밑면</text>

                <!-- Hidden rear edges (dashed) -->
                <line x1="100" y1="35" x2="65" y2="105" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />
                <line x1="65" y1="105" x2="145" y2="105" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />
                <line x1="150" y1="35" x2="145" y2="105" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />

                <!-- Front and Right visible side faces -->
                <!-- Front Side Face (Highlighted A) -->
                <polygon points="120,55 170,55 200,135 90,135" fill="#818cf8" fill-opacity="0.3" stroke="#4338ca" stroke-width="2.5" />
                <!-- Right Side Face -->
                <polygon points="170,55 150,35 175,105 200,135" fill="#6366f1" fill-opacity="0.15" stroke="#4338ca" stroke-width="2" />
                <!-- Left Side Face -->
                <line x1="100" y1="35" x2="90" y2="135" stroke="#4338ca" stroke-width="2" />
                <!-- Bottom Base Front Outline -->
                <line x1="90" y1="135" x2="200" y2="135" stroke="#4338ca" stroke-width="2.5" />
                <line x1="200" y1="135" x2="175" y2="105" stroke="#4338ca" stroke-width="2" />
                <line x1="145" y1="105" x2="175" y2="105" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />

                <!-- Label A in the front side face -->
                <circle cx="145" cy="95" r="11" fill="#4f46e5" />
                <text x="145" y="99" font-size="12" font-weight="bold" fill="#ffffff" text-anchor="middle">A</text>
              </svg>
            `,
            diagramCaption: "그림: 사각뿔대와 옆면 A",
            options: [
              { text: "직사각형, 6개", isCorrect: false },
              { text: "사다리꼴, 6개", isCorrect: true },
              { text: "평행사변형, 5개", isCorrect: false },
              { text: "사다리꼴, 5개", isCorrect: false }
            ],
            explanation: "각뿔대의 옆면(A)은 윗변과 아랫변이 평행하므로 항상 '사다리꼴'입니다. 사각뿔대의 면의 총 개수는 위아래 밑면 2개 + 옆면 4개 = 6개입니다."
          },
          {
            id: "p2-q7",
            category: "[그림 관찰] 삼각뿔대의 전개도 분석",
            question: "아래 그림은 어떤 뿔대의 전개도입니다. 두 밑면은 삼각형이고 3개의 옆면은 모두 사다리꼴입니다. 이 입체도형의 올바른 이름은 무엇일까요?",
            hint: "밑면이 삼각형인 뿔대의 전개도입니다.",
            diagramSvg: `
              <svg viewBox="0 0 240 150" class="w-52 h-32 drop-shadow-sm">
                <!-- Top triangle base attached -->
                <polygon points="120,10 95,45 145,45" fill="#a5b4fc" stroke="#4338ca" stroke-width="2" />
                
                <!-- 3 Trapezoids attached in a row -->
                <!-- Trap 1 -->
                <polygon points="45,45 95,45 80,105 20,105" fill="#e0e7ff" stroke="#4338ca" stroke-width="2" />
                <!-- Trap 2 (Center) -->
                <polygon points="95,45 145,45 160,105 80,105" fill="#c7d2fe" stroke="#4338ca" stroke-width="2" />
                <!-- Trap 3 -->
                <polygon points="145,45 195,45 220,105 160,105" fill="#e0e7ff" stroke="#4338ca" stroke-width="2" />
                
                <!-- Bottom larger triangle base attached to center trap -->
                <polygon points="80,105 160,105 120,145" fill="#a5b4fc" stroke="#4338ca" stroke-width="2" />

                <text x="120" y="32" font-size="9" font-weight="bold" fill="#312e81" text-anchor="middle">밑면(작음)</text>
                <text x="120" y="78" font-size="9" font-weight="bold" fill="#312e81" text-anchor="middle">사다리꼴 옆면</text>
                <text x="120" y="128" font-size="9" font-weight="bold" fill="#312e81" text-anchor="middle">밑면(큼)</text>
              </svg>
            `,
            diagramCaption: "그림: 뿔대의 펼쳐진 전개도",
            options: [
              { text: "삼각기둥", isCorrect: false },
              { text: "삼각뿔", isCorrect: false },
              { text: "사각뿔대", isCorrect: false },
              { text: "삼각뿔대", isCorrect: true }
            ],
            explanation: "두 밑면이 서로 닮은 크기가 다른 삼각형이고, 옆면 3개가 모두 사다리꼴이므로 이를 접으면 '삼각뿔대'가 됩니다."
          }
        ]
      },

      // PART 3: 정다면체의 면의 모양, 한 꼭짓점에 모인 면의 개수
      part3: {
        title: "5대 정다면체의 면의 모양과 한 꼭짓점에 모인 면의 수",
        desc: "오직 5개뿐인 정다면체(정사면체, 정육면체, 정팔면체, 정십이면체, 정이십면체)의 고유한 특성을 마스터하세요.",
        modelKey: "cube",
        questions: [
          {
            id: "p3-q1",
            category: "정다면체의 면의 모양",
            question: "정다면체 중 면의 모양이 '정삼각형'이 아닌 것은 어느 것일까요?",
            hint: "정다면체의 면은 정삼각형(3개), 정사각형(1개), 정오각형(1개)으로만 구성됩니다.",
            options: [
              { text: "정사면체", isCorrect: false },
              { text: "정육면체", isCorrect: true },
              { text: "정팔면체", isCorrect: false },
              { text: "정이십면체", isCorrect: false }
            ],
            explanation: "정육면체의 면의 모양은 '정사각형'입니다. 정사면체, 정팔면체, 정이십면체는 모두 면이 정삼각형입니다."
          },
          {
            id: "p3-q2",
            category: "한 꼭짓점에 모인 면의 개수 (정팔면체)",
            question: "정팔면체는 각 꼭짓점마다 몇 개의 정삼각형 면이 모여 있을까요?",
            hint: "정삼각형의 한 내각은 60°입니다. 정팔면체는 60° × 모인 개수가 360°보다 작아야 입체가 됩니다.",
            options: [
              { text: "3개", isCorrect: false },
              { text: "5개", isCorrect: false },
              { text: "4개", isCorrect: true },
              { text: "6개", isCorrect: false }
            ],
            explanation: "정팔면체는 한 꼭짓점에 정삼각형이 4개 모여 있습니다. (참고: 3개 모이면 정사면체, 5개 모이면 정이십면체)"
          },
          {
            id: "p3-q3",
            category: "정십이면체의 특성",
            question: "정십이면체에 대한 설명 중 옳은 것은 무엇일까요?",
            hint: "정십이면체는 5대 정다면체 중 유일하게 밑면/면의 모양이 '정오각형'입니다.",
            options: [
              { text: "면의 모양은 정삼각형이고, 한 꼭짓점에 5개의 면이 모인다.", isCorrect: false },
              { text: "면의 모양은 정사각형이고, 한 꼭짓점에 3개의 면이 모인다.", isCorrect: false },
              { text: "면의 모양은 정오각형이고, 한 꼭짓점에 4개의 면이 모인다.", isCorrect: false },
              { text: "면의 모양은 정오각형이고, 한 꼭짓점에 3개의 면이 모인다.", isCorrect: true }
            ],
            explanation: "정십이면체의 면의 모양은 정오각형이며, 한 꼭짓점에 3개의 면이 모여 입체 각을 이룹니다. (정오각형 한 내각 108° × 3 = 324° < 360°)"
          },
          {
            id: "p3-q4",
            category: "한 꼭짓점에 5개의 면이 모이는 정다면체",
            question: "면의 모양이 정삼각형이고, 한 꼭짓점에 모인 면의 개수가 5개인 정다면체의 이름은 무엇일까요?",
            hint: "정삼각형 면 20개로 둘러싸인 가장 면이 많은 정다면체입니다.",
            options: [
              { text: "정이십면체", isCorrect: true },
              { text: "정사면체", isCorrect: false },
              { text: "정팔면체", isCorrect: false },
              { text: "정십이면체", isCorrect: false }
            ],
            explanation: "정이십면체는 정삼각형 면이 한 꼭짓점에 5개씩 모여 만들어진 정다면체입니다. (60° × 5 = 300° < 360°)"
          },
          {
            id: "p3-q5",
            category: "정육면체의 꼭짓점과 면의 성질",
            question: "정육면체는 면의 모양이 정사각형입니다. 정육면체의 '한 꼭짓점에 모인 면의 개수'는 몇 개일까요?",
            hint: "정사각형의 한 내각은 90°입니다. 한 꼭짓점에서 세 면이 수직으로 만납니다.",
            options: [
              { text: "4개", isCorrect: false },
              { text: "5개", isCorrect: false },
              { text: "3개", isCorrect: true },
              { text: "6개", isCorrect: false }
            ],
            explanation: "정육면체는 한 꼭짓점에 정사각형 3개가 모여 있습니다. (90° × 3 = 270° < 360°, 4개가 모이면 360°로 평면이 되어 입체를 이룰 수 없습니다.)"
          },
          {
            id: "p3-q6",
            category: "[그림 관찰] 정사면체 꼭짓점 분석",
            question: "아래 그림은 정사면체의 겨냥도입니다. 꼭짓점 P에 모여 있는 면의 개수와 각 면의 모양을 바르게 나타낸 것은 무엇일까요?",
            hint: "정사면체는 모든 면이 정삼각형이며, 꼭짓점마다 모이는 면의 개수가 모두 같습니다.",
            diagramSvg: `
              <svg viewBox="0 0 240 150" class="w-48 h-32 drop-shadow-sm">
                <!-- Front-left face -->
                <polygon points="120,18 45,115 125,135" fill="#818cf8" fill-opacity="0.25" stroke="#4338ca" stroke-width="2.5" />
                <!-- Front-right face -->
                <polygon points="120,18 125,135 195,100" fill="#6366f1" fill-opacity="0.35" stroke="#4338ca" stroke-width="2.5" />
                <!-- Bottom hidden edge (dashed) -->
                <line x1="45" y1="115" x2="195" y2="100" stroke="#94a3b8" stroke-width="2" stroke-dasharray="4 3" />
                
                <!-- Highlighted Top Vertex P -->
                <circle cx="120" cy="18" r="6" fill="#ef4444" stroke="#ffffff" stroke-width="2" />
                <text x="135" y="20" font-size="13" font-weight="bold" fill="#dc2626">꼭짓점 P</text>
                
                <!-- Base vertices -->
                <circle cx="45" cy="115" r="3.5" fill="#f59e0b" />
                <circle cx="125" cy="135" r="3.5" fill="#f59e0b" />
                <circle cx="195" cy="100" r="3.5" fill="#f59e0b" />
              </svg>
            `,
            diagramCaption: "그림: 정사면체 겨냥도 (빨간 점: 꼭짓점 P)",
            options: [
              { text: "정삼각형, 3개", isCorrect: true },
              { text: "정삼각형, 4개", isCorrect: false },
              { text: "정사각형, 3개", isCorrect: false },
              { text: "정오각형, 3개", isCorrect: false }
            ],
            explanation: "정사면체는 모든 면이 '정삼각형'이며, 꼭짓점 P를 포함한 모든 꼭짓점에 모인 면의 수는 정확히 3개입니다."
          },
          {
            id: "p3-q7",
            category: "[그림 관찰] 정팔면체 꼭짓점 입체각 관찰",
            question: "아래 그림의 정팔면체에서 가운데 꼭짓점 V에 모여 있는 면의 개수는 몇 개일까요?",
            hint: "정팔면체는 위쪽 피라미드와 아래쪽 피라미드가 붙어 있는 형태이며, 중간 꼭짓점마다 위 2개, 아래 2개의 면이 만납니다.",
            diagramSvg: `
              <svg viewBox="0 0 240 160" class="w-48 h-36 drop-shadow-sm">
                <!-- Upper Pyramid Faces -->
                <polygon points="120,15 50,85 130,105" fill="#818cf8" fill-opacity="0.3" stroke="#4338ca" stroke-width="2" />
                <polygon points="120,15 130,105 190,75" fill="#6366f1" fill-opacity="0.4" stroke="#4338ca" stroke-width="2" />
                <!-- Hidden Back Upper Edges (dashed) -->
                <line x1="120" y1="15" x2="110" y2="65" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />
                <line x1="50" y1="85" x2="110" y2="65" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />
                <line x1="190" y1="75" x2="110" y2="65" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />

                <!-- Lower Pyramid Faces -->
                <polygon points="120,145 50,85 130,105" fill="#4f46e5" fill-opacity="0.35" stroke="#4338ca" stroke-width="2" />
                <polygon points="120,145 130,105 190,75" fill="#3730a3" fill-opacity="0.45" stroke="#4338ca" stroke-width="2" />
                <!-- Hidden Back Lower Edge -->
                <line x1="120" y1="145" x2="110" y2="65" stroke="#94a3b8" stroke-width="1.8" stroke-dasharray="3 3" />

                <!-- Highlighted Center Vertex V (130, 105) -->
                <circle cx="130" cy="105" r="6" fill="#ef4444" stroke="#ffffff" stroke-width="2" />
                <text x="145" y="118" font-size="12" font-weight="bold" fill="#dc2626">꼭짓점 V</text>
              </svg>
            `,
            diagramCaption: "그림: 정팔면체 겨냥도 (빨간 점: 꼭짓점 V)",
            options: [
              { text: "3개", isCorrect: false },
              { text: "5개", isCorrect: false },
              { text: "6개", isCorrect: false },
              { text: "4개", isCorrect: true }
            ],
            explanation: "정팔면체는 모든 꼭짓점마다 정삼각형 면이 4개씩 모여 있습니다. 표시된 꼭짓점 V에서도 위쪽 2개, 아래쪽 2개로 총 4개의 면이 만납니다."
          }
        ]
      }
    };

    // ----------------------------------------------------
    // 2. APP STATE MANAGEMENT
    // ----------------------------------------------------
    let currentPart = 1;
    let currentQuestionIndex = 0;
    // Stores user selections: key = `p${part}_q${index}` -> { selectedOptionIndex, isCorrect }
    const userAnswers = {};

    const cheatsheetData = {
      1: `
        <div class="bg-indigo-50/70 p-3 rounded-xl border border-indigo-100 leading-relaxed">
          <p class="font-bold text-indigo-900 mb-1">■ 각기둥 vs 각뿔 vs 각뿔대 기본 공식 (n기준)</p>
          <ul class="list-disc pl-4 space-y-1 text-slate-700">
            <li><strong>n각기둥:</strong> 면 (n + 2), 꼭짓점 2n, 모서리 3n</li>
            <li><strong>n각뿔:</strong> 면 (n + 1), 꼭짓점 (n + 1), 모서리 2n</li>
            <li><strong>n각뿔대:</strong> 면 (n + 2), 꼭짓점 2n, 모서리 3n</li>
          </ul>
        </div>
        <p class="text-slate-500 text-[11px]">※ 다면체는 오직 다각형인 면으로만 둘러싸인 입체도형이며, 면의 수에 따라 사면체, 오면체 등으로 부릅니다.</p>
      `,
      2: `
        <div class="bg-violet-50/70 p-3 rounded-xl border border-violet-100 leading-relaxed">
          <p class="font-bold text-violet-900 mb-1">■ 각뿔대(Prismatoid / Frustum)의 필수 정의</p>
          <ul class="list-disc pl-4 space-y-1 text-slate-700">
            <li><strong>형성:</strong> 각뿔을 밑면에 평행한 평면으로 잘라 각뿔이 아닌 쪽 입체</li>
            <li><strong>밑면:</strong> 2개, 서로 평행하지만 <span class="text-rose-600 font-bold">합동은 아님</span></li>
            <li><strong>옆면:</strong> 항상 <span class="text-indigo-600 font-bold">사다리꼴</span> 모양</li>
            <li><strong>면의 개수:</strong> 밑면 2 + 옆면 n = <strong>(n + 2)개</strong></li>
          </ul>
        </div>
      `,
      3: `
        <div class="overflow-x-auto rounded-xl border border-slate-200">
          <table class="w-full text-center text-[11px]">
            <thead class="bg-slate-100 text-slate-700 font-bold border-b border-slate-200">
              <tr>
                <th class="p-1.5">이름</th>
                <th class="p-1.5">면 모양</th>
                <th class="p-1.5">한꼭짓점 면수</th>
                <th class="p-1.5">면수(F)</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100 text-slate-600">
              <tr><td class="p-1.5 font-semibold text-indigo-600">정사면체</td><td>정삼각형</td><td>3개</td><td>4</td></tr>
              <tr><td class="p-1.5 font-semibold text-indigo-600">정육면체</td><td>정사각형</td><td>3개</td><td>6</td></tr>
              <tr><td class="p-1.5 font-semibold text-indigo-600">정팔면체</td><td>정삼각형</td><td>4개</td><td>8</td></tr>
              <tr><td class="p-1.5 font-semibold text-indigo-600">정십이면체</td><td>정오각형</td><td>3개</td><td>12</td></tr>
              <tr><td class="p-1.5 font-semibold text-indigo-600">정이십면체</td><td>정삼각형</td><td>5개</td><td>20</td></tr>
            </tbody>
          </table>
        </div>
        <p class="text-[11px] text-slate-500 mt-1">※ 정다면체는 모든 면이 합동인 정다각형이고, 각 꼭짓점에 모인 면의 수가 같아야 합니다.</p>
      `
    };

    function switchPart(partNumber) {
      currentPart = partNumber;
      currentQuestionIndex = 0;

      // Update Tab Styles
      for (let i = 1; i <= 3; i++) {
        const btn = document.getElementById(`tab-btn-${i}`);
        if (i === partNumber) {
          btn.className = "part-tab-btn flex flex-col sm:flex-row items-center justify-center gap-1.5 py-2.5 px-3 rounded-xl font-bold text-xs sm:text-sm transition-all duration-200 bg-white shadow-sm text-indigo-600";
          btn.querySelector("span").className = "w-5 h-5 rounded-full bg-indigo-100 text-indigo-700 flex items-center justify-center text-xs font-bold";
        } else {
          btn.className = "part-tab-btn flex flex-col sm:flex-row items-center justify-center gap-1.5 py-2.5 px-3 rounded-xl font-medium text-xs sm:text-sm text-slate-600 hover:text-slate-900 transition-all duration-200";
          btn.querySelector("span").className = "w-5 h-5 rounded-full bg-slate-300 text-slate-700 flex items-center justify-center text-xs font-bold";
        }
      }

      // Update Part Banner Information
      const pData = quizData[`part${partNumber}`];
      document.getElementById("banner-category-tag").textContent = `파트 ${partNumber} 핵심 퀴즈`;
      document.getElementById("banner-title").textContent = pData.title;
      document.getElementById("banner-desc").textContent = pData.desc;

      // Update Cheatsheet content & update its lock status
      document.getElementById("cheatsheet-content").innerHTML = cheatsheetData[partNumber];
      updateCheatsheetLockStatus();

      // Auto update 3D model corresponding to part default
      load3DShape(pData.modelKey);

      renderQuestion();
      renderIndicators();
      updateScoreBadge();
    }

    // Update lock/unlock state for the core formula cheatsheet
    function updateCheatsheetLockStatus() {
      const questions = quizData[`part${currentPart}`].questions;
      let answeredCount = 0;
      for (let i = 0; i < questions.length; i++) {
        if (userAnswers[`p${currentPart}_q${i}`]) {
          answeredCount++;
        }
      }

      const totalCount = questions.length;
      const isCompleted = (answeredCount === totalCount);

      const statusBadge = document.getElementById("cheatsheet-status-badge");
      const protectedArea = document.getElementById("cheatsheet-protected-area");
      const lockOverlay = document.getElementById("cheatsheet-lock-overlay");
      const lockRemainingText = document.getElementById("lock-remaining-text");

      if (isCompleted) {
        // Unlock
        statusBadge.className = "text-[11px] font-semibold px-2 py-0.5 rounded-full bg-emerald-50 text-emerald-600 border border-emerald-200 flex items-center gap-1";
        statusBadge.innerHTML = `<i data-lucide="unlock" class="w-3 h-3"></i> 열림`;
        protectedArea.className = "transition-all duration-300 filter-none pointer-events-auto select-auto";
        lockOverlay.classList.add("hidden");
      } else {
        // Keep Locked
        const remaining = totalCount - answeredCount;
        statusBadge.className = "text-[11px] font-semibold px-2 py-0.5 rounded-full bg-slate-100 text-slate-500 flex items-center gap-1";
        statusBadge.innerHTML = `<i data-lucide="lock" class="w-3 h-3"></i> 잠김 (${answeredCount}/${totalCount})`;
        protectedArea.className = "transition-all duration-300 filter blur-sm pointer-events-none select-none";
        lockOverlay.classList.remove("hidden");
        lockRemainingText.textContent = `파트 ${currentPart}의 문제를 모두 풀면 열립니다. (남은 문제: ${remaining}개)`;
      }

      lucide.createIcons();
    }

    function renderIndicators() {
      const container = document.getElementById("question-indicator-container");
      container.innerHTML = "";
      const totalQ = quizData[`part${currentPart}`].questions.length;

      for (let i = 0; i < totalQ; i++) {
        const stateKey = `p${currentPart}_q${i}`;
        const answer = userAnswers[stateKey];
        
        const pill = document.createElement("button");
        pill.onclick = () => jumpToQuestion(i);
        pill.className = `w-7 h-7 rounded-lg text-xs font-bold transition flex items-center justify-center ${
          i === currentQuestionIndex 
            ? "ring-2 ring-indigo-500 ring-offset-2 " 
            : ""
        }`;

        if (!answer) {
          pill.className += "bg-slate-100 text-slate-600 hover:bg-slate-200";
          pill.innerText = (i + 1);
        } else if (answer.isCorrect) {
          pill.className += "bg-emerald-500 text-white shadow-sm shadow-emerald-200";
          pill.innerHTML = `<i data-lucide="check" class="w-3.5 h-3.5"></i>`;
        } else {
          pill.className += "bg-rose-500 text-white shadow-sm shadow-rose-200";
          pill.innerHTML = `<i data-lucide="x" class="w-3.5 h-3.5"></i>`;
        }

        container.appendChild(pill);
      }
      lucide.createIcons();
    }

    function renderQuestion() {
      const currentList = quizData[`part${currentPart}`].questions;
      const totalCount = currentList.length;
      const q = currentList[currentQuestionIndex];
      const stateKey = `p${currentPart}_q${currentQuestionIndex}`;
      const record = userAnswers[stateKey];

      // Update Header Text & Page
      document.getElementById("banner-progress-text").textContent = `문제 ${currentQuestionIndex + 1} / ${totalCount}`;
      document.getElementById("question-type-badge").textContent = `[${q.category}]`;
      document.getElementById("question-text").textContent = `Q${currentQuestionIndex + 1}. ${q.question}`;
      document.getElementById("page-indicator").textContent = `${currentQuestionIndex + 1} / ${totalCount}`;

      // Handle SVG diagram rendering if problem contains a figure
      const diagramBox = document.getElementById("question-diagram-container");
      const svgWrapper = document.getElementById("question-svg-wrapper");
      const diagramCaption = document.getElementById("question-diagram-caption");

      if (q.diagramSvg) {
        diagramBox.classList.remove("hidden");
        svgWrapper.innerHTML = q.diagramSvg;
        diagramCaption.textContent = q.diagramCaption || "[도형 그림 참고]";
      } else {
        diagramBox.classList.add("hidden");
        svgWrapper.innerHTML = "";
      }

      // Update Nav Buttons
      document.getElementById("prev-btn").disabled = (currentQuestionIndex === 0);
      document.getElementById("next-btn").disabled = (currentQuestionIndex === totalCount - 1);

      // Render Options
      const optionsGrid = document.getElementById("options-grid");
      optionsGrid.innerHTML = "";

      q.options.forEach((opt, idx) => {
        const btn = document.createElement("button");
        btn.className = "perspective-card w-full text-left p-3.5 sm:p-4 rounded-xl border font-medium text-sm flex items-center justify-between transition group ";

        if (!record) {
          // Unanswered state
          btn.className += "bg-white hover:bg-indigo-50/70 border-slate-200 text-slate-700 hover:border-indigo-300";
          btn.onclick = () => selectOption(idx);
        } else {
          // Already answered
          btn.disabled = true;
          if (opt.isCorrect) {
            btn.className += "bg-emerald-50 border-emerald-500 text-emerald-900 font-semibold";
          } else if (record.selectedIdx === idx && !opt.isCorrect) {
            btn.className += "bg-rose-50 border-rose-400 text-rose-900";
          } else {
            btn.className += "bg-slate-50 border-slate-200 text-slate-400 opacity-60";
          }
        }

        btn.innerHTML = `
          <div class="flex items-center space-x-3">
            <span class="w-6 h-6 rounded-lg ${
              record && opt.isCorrect ? "bg-emerald-500 text-white" :
              record && record.selectedIdx === idx && !opt.isCorrect ? "bg-rose-500 text-white" :
              "bg-slate-100 text-slate-600 group-hover:bg-indigo-100 group-hover:text-indigo-600"
            } flex items-center justify-center text-xs font-bold transition">
              ${idx + 1}
            </span>
            <span>${opt.text}</span>
          </div>
          ${
            record && opt.isCorrect ? '<i data-lucide="check-circle-2" class="w-5 h-5 text-emerald-600 shrink-0"></i>' :
            record && record.selectedIdx === idx && !opt.isCorrect ? '<i data-lucide="x-circle" class="w-5 h-5 text-rose-500 shrink-0"></i>' :
            ''
          }
        `;

        optionsGrid.appendChild(btn);
      });

      // Feedback Box update
      const feedbackBox = document.getElementById("feedback-box");
      if (record) {
        feedbackBox.classList.remove("hidden");
        const fbIcon = document.getElementById("feedback-icon-container");
        const fbTitle = document.getElementById("feedback-title");
        const fbDesc = document.getElementById("feedback-desc");

        if (record.isCorrect) {
          feedbackBox.className = "rounded-xl p-4 text-sm bg-emerald-50 border border-emerald-200 text-emerald-900";
          fbIcon.innerHTML = `<i data-lucide="sparkles" class="w-5 h-5 text-emerald-600"></i>`;
          fbTitle.textContent = "정답입니다! 완벽해요!";
        } else {
          feedbackBox.className = "rounded-xl p-4 text-sm bg-rose-50 border border-rose-200 text-rose-900";
          fbIcon.innerHTML = `<i data-lucide="alert-triangle" class="w-5 h-5 text-rose-600"></i>`;
          fbTitle.textContent = "아쉽네요, 오답입니다.";
        }
        fbDesc.textContent = q.explanation;
      } else {
        feedbackBox.classList.add("hidden");
      }

      lucide.createIcons();
    }

    function selectOption(index) {
      const currentList = quizData[`part${currentPart}`].questions;
      const q = currentList[currentQuestionIndex];
      const isCorrect = q.options[index].isCorrect;
      const stateKey = `p${currentPart}_q${currentQuestionIndex}`;

      userAnswers[stateKey] = {
        selectedIdx: index,
        isCorrect: isCorrect
      };

      if (isCorrect) {
        triggerConfetti(0.4);
      }

      renderQuestion();
      renderIndicators();
      updateScoreBadge();
      updateCheatsheetLockStatus();

      checkOverallCompletion();
    }

    function prevQuestion() {
      if (currentQuestionIndex > 0) {
        currentQuestionIndex--;
        renderQuestion();
        renderIndicators();
      }
    }

    function nextQuestion() {
      const totalCount = quizData[`part${currentPart}`].questions.length;
      if (currentQuestionIndex < totalCount - 1) {
        currentQuestionIndex++;
        renderQuestion();
        renderIndicators();
      }
    }

    function jumpToQuestion(idx) {
      currentQuestionIndex = idx;
      renderQuestion();
      renderIndicators();
    }

    function updateScoreBadge() {
      let correctCount = 0;
      Object.values(userAnswers).forEach(ans => {
        if (ans.isCorrect) correctCount++;
      });
      document.getElementById("total-score-badge").textContent = correctCount;
    }

    function checkOverallCompletion() {
      const totalPossible = 21; // 7 questions per part * 3 parts
      const answeredKeys = Object.keys(userAnswers);
      if (answeredKeys.length === totalPossible) {
        let p1 = 0, p2 = 0, p3 = 0;
        for (let i = 0; i < 7; i++) {
          if (userAnswers[`p1_q${i}`]?.isCorrect) p1++;
          if (userAnswers[`p2_q${i}`]?.isCorrect) p2++;
          if (userAnswers[`p3_q${i}`]?.isCorrect) p3++;
        }

        document.getElementById("score-p1").textContent = `${p1} / 7`;
        document.getElementById("score-p2").textContent = `${p2} / 7`;
        document.getElementById("score-p3").textContent = `${p3} / 7`;

        const total = p1 + p2 + p3;
        document.getElementById("modal-score-summary").textContent = `총 21문제 중 ${total}문제를 맞췄습니다!`;

        setTimeout(() => {
          document.getElementById("completion-modal").classList.remove("hidden");
          triggerConfetti(1);
        }, 600);
      }
    }

    function closeCompletionModal() {
      document.getElementById("completion-modal").classList.add("hidden");
    }

    function resetAllQuizzes() {
      for (const key in userAnswers) {
        delete userAnswers[key];
      }
      closeCompletionModal();
      switchPart(1);
      updateCheatsheetLockStatus();
    }

    document.getElementById("reset-all-btn").addEventListener("click", () => {
      resetAllQuizzes();
    });

    function triggerConfetti(intensity) {
      try {
        confetti({
          particleCount: Math.floor(60 * intensity),
          spread: 70,
          origin: { y: 0.6 }
        });
      } catch (e) {
        // Confetti fallback safely ignored
      }
    }

    let scene, camera, renderer, controls, currentMeshGroup;

    function initThree() {
      const container = document.getElementById("three-container");
      const width = container.clientWidth || 360;
      const height = container.clientHeight || 280;

      scene = new THREE.Scene();
      scene.background = new THREE.Color(0x0f172a); // Slate-900

      camera = new THREE.PerspectiveCamera(45, width / height, 0.1, 100);
      camera.position.set(2.8, 2.2, 3.5);

      renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
      renderer.setSize(width, height);
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
      container.appendChild(renderer.domElement);

      controls = new THREE.OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;
      controls.dampingFactor = 0.05;
      controls.autoRotate = true;
      controls.autoRotateSpeed = 1.2;

      // Lighting
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
      scene.add(ambientLight);

      const dirLight1 = new THREE.DirectionalLight(0x6366f1, 1.2); // Indigo key
      dirLight1.position.set(5, 8, 5);
      scene.add(dirLight1);

      const dirLight2 = new THREE.DirectionalLight(0x38bdf8, 0.8); // Sky fill
      dirLight2.position.set(-5, -4, -5);
      scene.add(dirLight2);

      currentMeshGroup = new THREE.Group();
      scene.add(currentMeshGroup);

      // Load initial shape
      load3DShape("prism");

      // Handle window resize
      window.addEventListener("resize", onWindowResize);
    }

    function onWindowResize() {
      const container = document.getElementById("three-container");
      if (!container || !renderer || !camera) return;
      const width = container.clientWidth;
      const height = container.clientHeight;
      camera.aspect = width / height;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    }

    function reset3DCamera() {
      if (controls && camera) {
        camera.position.set(2.8, 2.2, 3.5);
        controls.target.set(0, 0, 0);
        controls.update();
      }
    }

    // Helper to generate glowing translucent polyhedron mesh with crisp edges & vertices
    function createPolyhedronGroup(geometry, nameKorean) {
      const group = new THREE.Group();

      // 1. Semi-translucent Face Material
      const faceMat = new THREE.MeshPhongMaterial({
        color: 0x6366f1,
        emissive: 0x1e1b4b,
        specular: 0xffffff,
        shininess: 60,
        transparent: true,
        opacity: 0.82,
        side: THREE.DoubleSide,
        flatShading: true
      });
      const mesh = new THREE.Mesh(geometry, faceMat);
      group.add(mesh);

      // 2. Clear Edges Wireframe
      const edgeGeo = new THREE.EdgesGeometry(geometry);
      const edgeMat = new THREE.LineBasicMaterial({ color: 0x38bdf8, linewidth: 2 });
      const wireframe = new THREE.LineSegments(edgeGeo, edgeMat);
      group.add(wireframe);

      // 3. Highlight Vertices with small spheres
      const posAttr = geometry.attributes.position;
      const vertexPoints = [];
      const vGeom = new THREE.SphereGeometry(0.045, 12, 12);
      const vMat = new THREE.MeshBasicMaterial({ color: 0xfacc15 }); // Gold

      // Extract unique vertices
      for (let i = 0; i < posAttr.count; i++) {
        const vx = posAttr.getX(i);
        const vy = posAttr.getY(i);
        const vz = posAttr.getZ(i);

        // Simple duplicate prevention
        const isDuplicate = vertexPoints.some(v => v.distanceTo(new THREE.Vector3(vx, vy, vz)) < 0.05);
        if (!isDuplicate) {
          const vMesh = new THREE.Mesh(vGeom, vMat);
          vMesh.position.set(vx, vy, vz);
          group.add(vMesh);
          vertexPoints.push(new THREE.Vector3(vx, vy, vz));
        }
      }

      document.getElementById("current-3d-name").textContent = nameKorean;
      return group;
    }

    function load3DShape(shapeType) {
      if (!scene || !currentMeshGroup) return;

      // Clear existing children
      while (currentMeshGroup.children.length > 0) {
        const obj = currentMeshGroup.children[0];
        if (obj.geometry) obj.geometry.dispose();
        currentMeshGroup.remove(obj);
      }

      let geom;
      let label = "";

      switch (shapeType) {
        case "tetrahedron":
          geom = new THREE.TetrahedronGeometry(1.3);
          label = "정사면체 (Regular Tetrahedron)";
          break;
        case "cube":
          geom = new THREE.BoxGeometry(1.4, 1.4, 1.4);
          label = "정육면체 (Cube / Regular Hexahedron)";
          break;
        case "octahedron":
          geom = new THREE.OctahedronGeometry(1.3);
          label = "정팔면체 (Regular Octahedron)";
          break;
        case "frustum":
          // Cylinder with different top and bottom radius and 4 radial segments -> Square Frustum
          geom = new THREE.CylinderGeometry(0.7, 1.3, 1.3, 4, 1);
          label = "사각뿔대 (Square Frustum)";
          break;
        case "prism":
          // Triangular prism (Cylinder with 3 segments)
          geom = new THREE.CylinderGeometry(1.1, 1.1, 1.6, 3, 1);
          label = "삼각기둥 (Triangular Prism)";
          break;
        default:
          geom = new THREE.BoxGeometry(1.4, 1.4, 1.4);
          label = "정육면체";
      }

      const newGroup = createPolyhedronGroup(geom, label);
      currentMeshGroup.add(newGroup);

      // Highlight active button in toolbar
      document.querySelectorAll(".model-quick-btn").forEach(btn => {
        if (btn.textContent.includes(label.substring(0, 3))) {
          btn.className = "model-quick-btn px-2.5 py-1 rounded-lg bg-indigo-50 text-indigo-700 border border-indigo-200 font-bold text-xs shrink-0 transition";
        } else {
          btn.className = "model-quick-btn px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 font-medium text-slate-700 text-xs shrink-0 transition";
        }
      });
    }

    // Three.js Render Loop
    function animate() {
      requestAnimationFrame(animate);
      if (controls) controls.update();
      if (renderer && scene && camera) {
        renderer.render(scene, camera);
      }
    }

    window.onload = function () {
      initThree();
      animate();
      switchPart(1);
    };
  </script>
</body>
</html>[Uploading code_artifact (3).html…]()
