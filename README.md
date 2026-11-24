<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>每日單字大師 | Daily Vocab & Crossword Studio</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'Noto Sans TC', 'sans-serif'],
            mono: ['ui-monospace', 'SFMono-Regular', 'Menlo', 'Monaco', 'Consolas', 'monospace'],
          }
        }
      }
    }
  </script>
  <style>
    /* 3D Flashcard Flip Animation */
    .perspective-1200 {
      perspective: 1200px;
    }
    .transform-style-3d {
      transform-style: preserve-3d;
    }
    .backface-hidden {
      backface-visibility: hidden;
      -webkit-backface-visibility: hidden;
    }
    .rotate-y-180 {
      transform: rotateY(180deg);
    }

    /* Custom Scrollbars */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #f1f5f9;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #94a3b8;
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased min-h-screen flex flex-col">

  <!-- Top Navigation & Daily Deck Manager Header -->
  <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-xs">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex flex-wrap items-center justify-between py-3 gap-3">
        <!-- Brand Identity -->
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-2xl bg-indigo-600 text-white flex items-center justify-center shadow-md shadow-indigo-200 shrink-0">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
            </svg>
          </div>
          <div>
            <h1 class="text-lg sm:text-xl font-extrabold text-slate-900 tracking-tight leading-tight">每日單字大師</h1>
            <p class="text-xs text-slate-500 font-medium">Daily Vocab & Crossword Studio</p>
          </div>
        </div>

        <!-- Global Daily Deck Switcher & JSON Backup -->
        <div class="flex flex-wrap items-center gap-2">
          <div class="flex items-center bg-slate-100 rounded-xl p-1 border border-slate-200">
            <span class="text-xs font-bold text-slate-500 px-2 hidden md:inline">學習日期庫：</span>
            <select id="globalDeckSelector" class="bg-white text-slate-800 text-xs sm:text-sm font-bold rounded-lg px-2.5 py-1.5 border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 cursor-pointer max-w-[210px] sm:max-w-xs truncate"></select>
            <button id="deleteDeckBtn" title="刪除此日期單字庫" class="ml-1 p-1.5 text-slate-400 hover:text-rose-600 rounded-lg hover:bg-rose-50 transition">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>
          </div>

          <div class="flex items-center gap-1.5">
            <button id="exportJsonBtn" title="匯出 JSON 備份" class="px-3 py-2 rounded-xl bg-white hover:bg-slate-100 text-slate-700 border border-slate-200 text-xs font-bold transition flex items-center gap-1.5">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 text-slate-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
              </svg>
              <span>匯出 JSON</span>
            </button>
            <label title="匯入 JSON 備份" class="px-3 py-2 rounded-xl bg-white hover:bg-slate-100 text-slate-700 border border-slate-200 text-xs font-bold transition cursor-pointer flex items-center gap-1.5">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 text-slate-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
              </svg>
              <span>匯入 JSON</span>
              <input type="file" id="importJsonInput" accept=".json" class="hidden" />
            </label>
          </div>
        </div>
      </div>

      <!-- Mode Navigation Tabs -->
      <nav class="flex gap-2 pb-3 overflow-x-auto">
        <button data-tab="input" class="nav-tab-btn flex items-center gap-2 px-4 py-2 rounded-xl text-xs sm:text-sm font-bold transition whitespace-nowrap bg-white text-slate-600 hover:bg-slate-100 border border-slate-200">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
          </svg>
          單字管理與 AI 生成
        </button>

        <button data-tab="flashcards" class="nav-tab-btn flex items-center gap-2 px-4 py-2 rounded-xl text-xs sm:text-sm font-bold transition whitespace-nowrap bg-indigo-600 text-white shadow-sm">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
          </svg>
          3D 互動學習卡
        </button>

        <button data-tab="crossword" class="nav-tab-btn flex items-center gap-2 px-4 py-2 rounded-xl text-xs sm:text-sm font-bold transition whitespace-nowrap bg-white text-slate-600 hover:bg-slate-100 border border-slate-200">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
          </svg>
          片語語境填字遊戲
        </button>
      </nav>
    </div>
  </header>

  <!-- Toast Notification Container -->
  <div id="toastContainer" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

  <!-- Main Workspace -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

    <!-- =================================================================== -->
    <!-- VIEW 1: 單字管理與 AI 生成 (Daily Word List Manager & AI Generator)   -->
    <!-- =================================================================== -->
    <section id="view-input" class="hidden space-y-6">
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">

        <!-- Left Panel: Date Selector & Bulk Gemini AI Generator -->
        <div class="lg:col-span-5 bg-white rounded-3xl p-6 border border-slate-200 shadow-xs space-y-5">
          <div class="border-b border-slate-100 pb-3.5">
            <h2 class="text-lg sm:text-xl font-extrabold text-slate-900">每日單字輸入與 AI 生成</h2>
            <p class="text-xs sm:text-sm text-slate-500 mt-1">每天貼上 10–20 個英文單字，AI 自動生成中文解釋、2–3 組常用片語搭配與雙語例句。</p>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
            <div>
              <label for="deckDateInput" class="block text-xs font-bold text-slate-700 mb-1">日期設定 (Date)</label>
              <input type="date" id="deckDateInput" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 text-sm font-semibold focus:outline-none focus:ring-2 focus:ring-indigo-500" />
            </div>
            <div>
              <label for="deckTopicInput" class="block text-xs font-bold text-slate-700 mb-1">主題標籤 (選填)</label>
              <input type="text" id="deckTopicInput" placeholder="例如：商務溝通、工程科技" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
            </div>
          </div>

          <div>
            <div class="flex items-center justify-between mb-1">
              <label for="rawWordsInput" class="block text-xs font-bold text-slate-700">貼上 10–20 個英文單字（換行或逗號分隔）</label>
              <span id="wordCountBadge" class="text-xs font-bold px-2.5 py-0.5 rounded-full bg-indigo-50 text-indigo-700">0 個單字</span>
            </div>
            <textarea id="rawWordsInput" rows="6" placeholder="請輸入今日單字，例如：&#10;mitigate, leverage, feasible, bottleneck, consensus, streamline, allocate, scrutinize, benchmark, resilient, stipulate, contingency" class="w-full p-3.5 rounded-2xl border border-slate-300 font-mono text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 leading-relaxed"></textarea>
            <div class="flex items-center justify-between mt-1.5">
              <button type="button" id="insertDemoWordsBtn" class="text-xs font-bold text-indigo-600 hover:text-indigo-800 underline">填入示範 12 個單字</button>
              <span class="text-xs text-slate-400">支援換行、逗號、分號分隔</span>
            </div>
          </div>

          <!-- AI Progress Indicator -->
          <div id="aiProgressBanner" class="hidden bg-indigo-50 border border-indigo-200 rounded-2xl p-4 flex items-center gap-3">
            <div class="w-5 h-5 border-2 border-indigo-600 border-t-transparent rounded-full animate-spin shrink-0"></div>
            <div>
              <p id="aiProgressText" class="text-xs sm:text-sm font-bold text-indigo-900">正在透過 Gemini AI 生成解釋與常用片語...</p>
              <p class="text-xs text-indigo-600">完成後將自動同步至 3D 學習卡與填字遊戲</p>
            </div>
          </div>

          <!-- AI Error Banner -->
          <div id="aiErrorBanner" class="hidden bg-rose-50 border border-rose-200 rounded-2xl p-3.5 text-xs sm:text-sm text-rose-700 font-semibold"></div>

          <button type="button" id="generateAiBtn" class="w-full py-3.5 px-5 rounded-2xl bg-indigo-600 hover:bg-indigo-700 disabled:opacity-50 text-white font-bold text-sm shadow-md shadow-indigo-200 transition flex items-center justify-center gap-2">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M13 10V3L4 14h7v7l9-11h-7z" />
            </svg>
            AI 智能生成學習卡與片語
          </button>
        </div>

        <!-- Right Panel: Editable Word Cards List for Active Date Deck -->
        <div class="lg:col-span-7 bg-white rounded-3xl p-6 border border-slate-200 shadow-xs space-y-5">
          <div class="flex flex-wrap items-center justify-between gap-3 border-b border-slate-100 pb-3.5">
            <div>
              <h2 class="text-lg sm:text-xl font-extrabold text-slate-900" id="activeDeckTableTitle">目前單字庫明細</h2>
              <p class="text-xs text-slate-500 mt-0.5">可隨時檢視、手動新增、編輯或刪除任何單字與多組常用片語</p>
            </div>
            <button type="button" id="addManualWordBtn" class="px-4 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs sm:text-sm font-bold shadow-xs transition flex items-center gap-1.5">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 4v16m8-8H4" />
              </svg>
              新增單字
            </button>
          </div>

          <div id="deckWordsEditorList" class="space-y-3 max-h-[660px] overflow-y-auto pr-1">
            <!-- Populated dynamically -->
          </div>
        </div>
      </div>
    </section>

    <!-- =================================================================== -->
    <!-- VIEW 2: 3D 互動學習卡 (3D Interactive Flashcards)                     -->
    <!-- =================================================================== -->
    <section id="view-flashcards" class="space-y-6">
      <!-- Mastery Progress Bar & Filter Controls -->
      <div class="bg-white rounded-2xl p-4 sm:p-5 border border-slate-200 shadow-xs flex flex-col md:flex-row md:items-center justify-between gap-4">
        <div class="flex-1 space-y-2">
          <div class="flex items-center justify-between text-xs sm:text-sm">
            <span class="font-extrabold text-slate-800" id="fcDeckHeader">2026-11-24 商務・工程・金融核心詞彙 (12 詞)</span>
            <span class="font-bold text-emerald-600" id="fcMasterySummary">已掌握 0 / 12 (0%)</span>
          </div>
          <div class="w-full h-2.5 bg-slate-100 rounded-full overflow-hidden">
            <div id="fcProgressBar" class="h-full bg-emerald-500 rounded-full transition-all duration-500" style="width: 0%"></div>
          </div>
        </div>

        <!-- Filter Toggle ("全部單字" vs "仍需複習") -->
        <div class="flex items-center gap-2 shrink-0">
          <div class="inline-flex rounded-xl bg-slate-100 p-1 border border-slate-200">
            <button type="button" id="filterAllBtn" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-white text-indigo-700 shadow-2xs transition">
              全部單字
            </button>
            <button type="button" id="filterReviewBtn" class="px-3 py-1.5 rounded-lg text-xs font-bold text-slate-600 hover:text-slate-900 transition">
              仍需複習
            </button>
          </div>

          <button type="button" id="shuffleDeckBtn" title="隨機洗牌" class="p-2.5 rounded-xl bg-slate-50 border border-slate-200 hover:bg-slate-100 text-slate-600 transition">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
            </svg>
          </button>
        </div>
      </div>

      <!-- Empty Filter State -->
      <div id="fcEmptyFilterState" class="hidden bg-white rounded-3xl p-12 border border-slate-200 text-center space-y-4 shadow-xs">
        <div class="w-16 h-16 rounded-2xl bg-emerald-100 text-emerald-600 mx-auto flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        </div>
        <h3 class="text-xl font-extrabold text-slate-900">太棒了！本組所有單字皆已標記為「已掌握」！</h3>
        <p class="text-sm text-slate-500 max-w-md mx-auto">您可以切換回「全部單字」再次複習，或前往「片語語境填字遊戲」挑戰拼寫應用。</p>
        <button type="button" id="resetFilterAllBtn" class="px-5 py-2.5 rounded-xl bg-indigo-600 text-white text-sm font-bold hover:bg-indigo-700 transition">顯示全部單字</button>
      </div>

      <!-- 3D Flashcard Stage -->
      <div id="fcMainStage" class="max-w-3xl mx-auto space-y-5">
        <div class="flex items-center justify-between text-xs sm:text-sm text-slate-500 px-1">
          <span class="font-extrabold text-slate-700" id="fcPositionLabel">第 1 / 12 張</span>
          <span class="hidden sm:inline">快捷鍵：<kbd class="px-1.5 py-0.5 bg-white border border-slate-300 rounded text-slate-700 font-mono">Space</kbd> 翻面 · <kbd class="px-1.5 py-0.5 bg-white border border-slate-300 rounded text-slate-700 font-mono">←</kbd> <kbd class="px-1.5 py-0.5 bg-white border border-slate-300 rounded text-slate-700 font-mono">→</kbd> 切換卡片</span>
        </div>

        <!-- 3D Flip Card Container -->
        <div class="perspective-1200 w-full">
          <div id="flashcard3D" class="relative w-full min-h-[480px] sm:min-h-[500px] transition-transform duration-500 transform-style-3d cursor-pointer select-none">

            <!-- FRONT OF CARD -->
            <div class="backface-hidden absolute inset-0 bg-white rounded-3xl border-2 border-slate-200/80 shadow-xl shadow-slate-200/50 p-6 sm:p-10 flex flex-col justify-between">
              <div class="flex items-center justify-between">
                <span id="cardFrontStatus" class="px-3 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-800">仍需複習</span>
                <span class="text-xs font-semibold text-slate-400 flex items-center gap-1">
                  <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
                  </svg>
                  點擊卡片或按空白鍵翻面
                </span>
              </div>

              <div class="text-center my-auto space-y-5">
                <div class="inline-flex items-center gap-2.5 px-3.5 py-1.5 rounded-xl bg-indigo-50 text-indigo-700 font-bold text-sm">
                  <span id="cardFrontPos">v.</span>
                  <span class="text-indigo-300">|</span>
                  <span id="cardFrontIpa" class="font-mono text-slate-600">/ˈmɪt.ɪ.ɡeɪt/</span>
                </div>

                <h2 id="cardFrontWord" class="text-4xl sm:text-6xl font-extrabold text-slate-900 tracking-tight break-words">mitigate</h2>

                <!-- US & UK Pronunciation Buttons -->
                <div class="pt-2 flex flex-wrap items-center justify-center gap-3">
                  <button type="button" id="btnSpeakUS" class="inline-flex items-center gap-2 px-4 py-2.5 rounded-2xl bg-indigo-600 hover:bg-indigo-700 active:scale-95 text-white font-bold text-xs sm:text-sm shadow-md shadow-indigo-200 transition">
                    <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                    </svg>
                    🇺🇸 美式發音 (US)
                  </button>
                  <button type="button" id="btnSpeakUK" class="inline-flex items-center gap-2 px-4 py-2.5 rounded-2xl bg-slate-100 hover:bg-slate-200 active:scale-95 text-slate-800 border border-slate-200 font-bold text-xs sm:text-sm transition">
                    <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 text-indigo-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                    </svg>
                    🇬🇧 英式發音 (UK)
                  </button>
                </div>
              </div>

              <div class="text-center text-xs text-slate-400">
                回想中文解釋與常用搭配片語後，翻面檢視詳細說明
              </div>
            </div>

            <!-- BACK OF CARD -->
            <div class="backface-hidden rotate-y-180 absolute inset-0 bg-white rounded-3xl border-2 border-indigo-200 shadow-xl shadow-indigo-100/60 p-5 sm:p-8 flex flex-col justify-between overflow-y-auto">
              <!-- Header -->
              <div class="flex flex-wrap items-center justify-between gap-2 pb-2.5 border-b border-slate-100">
                <div class="flex items-center gap-2">
                  <h3 id="cardBackWord" class="text-2xl sm:text-3xl font-extrabold text-slate-900">mitigate</h3>
                  <span id="cardBackPos" class="px-2.5 py-0.5 rounded-lg bg-slate-100 text-slate-700 font-bold text-xs">v.</span>
                  <span id="cardBackIpa" class="font-mono text-xs sm:text-sm text-slate-500">/ˈmɪt.ɪ.ɡeɪt/</span>
                </div>
                <span class="text-xs text-indigo-500 font-bold">點擊空白處翻回正面</span>
              </div>

              <!-- Traditional Chinese Definition -->
              <div class="my-2.5">
                <span class="text-xs font-bold uppercase tracking-wider text-slate-400 block mb-0.5">繁體中文解釋</span>
                <p id="cardBackMeaning" class="text-lg sm:text-xl font-extrabold text-indigo-700">減輕、緩和（風險、損害或痛苦）</p>
              </div>

              <!-- Prominently Highlighted Common Phrases List with Audio Playback -->
              <div class="my-2 bg-amber-50 rounded-2xl p-3.5 sm:p-4 border-2 border-amber-200/90 shadow-2xs space-y-2.5">
                <div class="flex items-center gap-1.5 text-xs font-extrabold text-amber-900 uppercase tracking-wide">
                  <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 text-amber-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M13 10V3L4 14h7v7l9-11h-7z" />
                  </svg>
                  <span>常用片語 (Common Phrases)</span>
                </div>
                <div id="cardBackPhrasesList" class="space-y-2">
                  <!-- Populated dynamically with 2-3 collocations & speaker buttons -->
                </div>
              </div>

              <!-- Bilingual Example Sentence -->
              <div class="mt-2 bg-slate-50 rounded-2xl p-3.5 sm:p-4 border border-slate-200/80">
                <div class="flex items-center justify-between mb-1">
                  <span class="text-xs font-bold text-slate-500">雙語例句 (Example)</span>
                  <button type="button" id="btnSpeakExample" class="inline-flex items-center gap-1 text-xs font-bold text-indigo-600 hover:text-indigo-800 px-2 py-0.5 rounded-lg hover:bg-indigo-50 transition">
                    <svg xmlns="http://www.w3.org/2000/svg" class="w-3.5 h-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M15.536 8.464a5 5 0 010 7.072M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                    </svg>
                    朗讀例句
                  </button>
                </div>
                <p id="cardBackExampleEn" class="text-xs sm:text-sm font-semibold text-slate-800 leading-snug"></p>
                <p id="cardBackExampleZh" class="text-xs text-slate-600 mt-1"></p>
              </div>
            </div>

          </div>
        </div>

        <!-- Bottom Navigation & Mastery Controls -->
        <div class="grid grid-cols-2 sm:flex items-center justify-between gap-3 pt-1">
          <button type="button" id="prevCardBtn" class="order-1 px-4 py-3 rounded-2xl bg-white border border-slate-200 hover:bg-slate-100 text-slate-700 font-bold text-sm flex items-center justify-center gap-2 shadow-xs transition">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M15 19l-7-7 7-7" />
            </svg>
            上一張
          </button>

          <div class="col-span-2 order-3 sm:order-2 flex items-center justify-center gap-3">
            <button type="button" id="btnMarkReview" class="flex-1 sm:flex-initial px-5 py-3 rounded-2xl bg-amber-50 hover:bg-amber-100 text-amber-800 border border-amber-200 font-bold text-sm flex items-center justify-center gap-2 transition">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 text-amber-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              仍需複習
            </button>

            <button type="button" id="btnMarkMastered" class="flex-1 sm:flex-initial px-5 py-3 rounded-2xl bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm flex items-center justify-center gap-2 shadow-md shadow-emerald-200 transition">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
              </svg>
              已掌握
            </button>
          </div>

          <button type="button" id="nextCardBtn" class="order-2 sm:order-3 px-4 py-3 rounded-2xl bg-white border border-slate-200 hover:bg-slate-100 text-slate-700 font-bold text-sm flex items-center justify-center gap-2 shadow-xs transition">
            下一張
            <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 5l7 7-7 7" />
            </svg>
          </button>
        </div>

        <!-- Quick Word Jump Bar -->
        <div class="bg-white rounded-2xl p-4 border border-slate-200 shadow-xs">
          <div class="flex items-center justify-between mb-2">
            <span class="text-xs font-bold text-slate-500">單字快速索引（點擊直接跳轉）：</span>
            <button type="button" id="editCurrentFlashcardBtn" class="text-xs font-bold text-indigo-600 hover:text-indigo-800">編輯此單字 ✎</button>
          </div>
          <div id="fcPillsBar" class="flex flex-wrap gap-2"></div>
        </div>
      </div>
    </section>

    <!-- =================================================================== -->
    <!-- VIEW 3: 片語語境填字遊戲 (Phrase-Context Crossword Puzzle)            -->
    <!-- =================================================================== -->
    <section id="view-crossword" class="hidden space-y-5">
      <!-- Crossword Action Toolbar -->
      <div class="bg-white rounded-2xl p-4 border border-slate-200 shadow-xs flex flex-wrap items-center justify-between gap-3">
        <div>
          <h2 class="text-base sm:text-lg font-extrabold text-slate-900">片語語境填字遊戲 (Phrase-Context Crossword)</h2>
          <p class="text-xs text-slate-500">結合中文釋義與常用片語挖空提示（例如 ______ the risk），在真實片語語境中拼寫單字</p>
        </div>

        <div class="flex flex-wrap items-center gap-2">
          <button type="button" id="cwCheckBtn" class="px-3.5 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs sm:text-sm font-bold shadow-xs transition flex items-center gap-1.5">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            檢查答案
          </button>

          <button type="button" id="cwSingleHintBtn" class="px-3.5 py-2 rounded-xl bg-amber-50 hover:bg-amber-100 text-amber-800 border border-amber-200 text-xs sm:text-sm font-bold transition flex items-center gap-1.5">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 text-amber-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
            </svg>
            提示一格
          </button>

          <button type="button" id="cwRevealAllBtn" class="px-3.5 py-2 rounded-xl bg-white hover:bg-rose-50 text-rose-600 border border-rose-200 text-xs sm:text-sm font-bold transition">
            顯示解答
          </button>

          <button type="button" id="cwRegenerateBtn" class="px-3.5 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs sm:text-sm font-bold transition flex items-center gap-1.5">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
            </svg>
            重新生成版面
          </button>
        </div>
      </div>

      <!-- Synchronized Active Clue Banner -->
      <div class="bg-indigo-600 text-white rounded-2xl p-3.5 sm:p-4 shadow-md flex items-center justify-between gap-3">
        <div class="flex items-center gap-3 min-w-0">
          <span id="cwActiveBadge" class="px-2.5 py-1 rounded-lg bg-white/20 font-mono font-bold text-xs sm:text-sm shrink-0">1 橫向</span>
          <p id="cwActiveClueText" class="text-sm sm:text-base font-semibold truncate">請點選任一填字格開始作答</p>
        </div>
        <button type="button" id="cwSwitchDirBtn" class="px-3 py-1.5 rounded-xl bg-white/15 hover:bg-white/25 text-xs font-bold shrink-0 transition">
          切換方向
        </button>
      </div>

      <!-- Completion Celebration Banner -->
      <div id="cwCompleteBanner" class="hidden bg-gradient-to-r from-emerald-500 to-teal-600 text-white rounded-2xl p-5 shadow-lg flex flex-col sm:flex-row items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <div class="w-12 h-12 rounded-2xl bg-white/20 flex items-center justify-center text-2xl">🎉</div>
          <div>
            <h3 class="text-lg sm:text-xl font-extrabold">恭喜完全答對！所有單字拼寫正確！</h3>
            <p class="text-xs sm:text-sm text-emerald-100">您已成功完成本組常用片語語境填字挑戰。</p>
          </div>
        </div>
        <button type="button" id="cwReplayBtn" class="px-5 py-2.5 rounded-xl bg-white text-emerald-700 font-bold text-sm hover:bg-emerald-50 transition shrink-0">
          重新生成再挑戰
        </button>
      </div>

      <!-- Crossword Board & Across/Down Clue Lists -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">
        <!-- Crossword Grid -->
        <div class="lg:col-span-7 bg-white rounded-2xl p-3 sm:p-6 border border-slate-200 shadow-xs flex flex-col items-center justify-center overflow-x-auto">
          <div id="cwGridContainer" class="inline-block bg-slate-900 p-1.5 sm:p-2 rounded-xl shadow-inner"></div>
          <p class="text-xs text-slate-400 mt-3 text-center">支援鍵盤與手機輸入 · 自動前進下一格、Backspace 退格、方向鍵移動與重複點擊切換方向</p>
        </div>

        <!-- Across & Down Clue Lists -->
        <div class="lg:col-span-5 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-1 gap-4">
          <div class="bg-white rounded-2xl p-4 border border-slate-200 shadow-xs">
            <h3 class="text-sm font-extrabold text-indigo-700 uppercase tracking-wider pb-2.5 mb-2.5 border-b border-slate-100 flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-600"></span>
              橫向提示 (Across)
            </h3>
            <ul id="cwAcrossList" class="space-y-1.5 max-h-[300px] overflow-y-auto pr-1"></ul>
          </div>

          <div class="bg-white rounded-2xl p-4 border border-slate-200 shadow-xs">
            <h3 class="text-sm font-extrabold text-teal-700 uppercase tracking-wider pb-2.5 mb-2.5 border-b border-slate-100 flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-teal-600"></span>
              直向提示 (Down)
            </h3>
            <ul id="cwDownList" class="space-y-1.5 max-h-[300px] overflow-y-auto pr-1"></ul>
          </div>
        </div>
      </div>
    </section>

  </main>

  <!-- =================================================================== -->
  <!-- MODAL: ADD / EDIT SINGLE WORD ENTRY                                 -->
  <!-- =================================================================== -->
  <div id="wordModal" class="hidden fixed inset-0 z-50 bg-slate-900/50 backdrop-blur-xs flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl max-w-2xl w-full max-h-[90vh] overflow-y-auto p-6 sm:p-7 shadow-2xl border border-slate-200 space-y-4">
      <div class="flex items-center justify-between border-b border-slate-100 pb-3">
        <h3 id="wordModalHeading" class="text-lg font-extrabold text-slate-900">編輯單字與常用片語</h3>
        <button type="button" id="closeModalBtn" class="p-1.5 rounded-xl text-slate-400 hover:text-slate-700 hover:bg-slate-100">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <form id="wordModalForm" class="space-y-4">
        <input type="hidden" id="modalWordIndex" value="-1" />

        <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">英文單字 *</label>
            <input type="text" id="modalWordInput" required placeholder="mitigate" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">詞性 (POS)</label>
            <input type="text" id="modalPosInput" placeholder="v. / n. / adj." class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">IPA 音標</label>
            <input type="text" id="modalIpaInput" placeholder="/ˈmɪt.ɪ.ɡeɪt/" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-mono text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
          </div>
        </div>

        <div>
          <label class="block text-xs font-bold text-slate-700 mb-1">繁體中文解釋 *</label>
          <input type="text" id="modalMeaningZhInput" required placeholder="緩和、減輕（風險、損害或痛苦）" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-semibold focus:outline-none focus:ring-2 focus:ring-indigo-500" />
        </div>

        <!-- 3 Collocation / Phrase Inputs -->
        <div class="bg-amber-50/70 p-4 rounded-2xl border border-amber-200 space-y-3">
          <span class="block text-xs font-extrabold text-amber-900">常用片語搭配 (Common Phrases - 最多 3 組)</span>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
            <input type="text" id="modalPhraseEn1" required placeholder="片語 1 英文 (例: mitigate the risk)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm font-semibold" />
            <input type="text" id="modalPhraseZh1" required placeholder="片語 1 中文解釋 (例: 降低風險)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm" />
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
            <input type="text" id="modalPhraseEn2" placeholder="片語 2 英文 (選填)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm font-semibold" />
            <input type="text" id="modalPhraseZh2" placeholder="片語 2 中文解釋 (選填)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm" />
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
            <input type="text" id="modalPhraseEn3" placeholder="片語 3 英文 (選填)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm font-semibold" />
            <input type="text" id="modalPhraseZh3" placeholder="片語 3 中文解釋 (選填)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm" />
          </div>
        </div>

        <div class="space-y-2.5">
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">英文例句</label>
            <input type="text" id="modalExampleEnInput" placeholder="Natural English example sentence..." class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">例句繁體中文翻譯</label>
            <input type="text" id="modalExampleZhInput" placeholder="例句中文翻譯..." class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" />
          </div>
        </div>

        <div class="flex items-center justify-end gap-2 pt-2">
          <button type="button" id="cancelModalBtn" class="px-4 py-2.5 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs sm:text-sm font-bold">取消</button>
          <button type="submit" class="px-5 py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs sm:text-sm font-bold shadow-sm">儲存單字</button>
        </div>
      </form>
    </div>
  </div>

  <script>
    // =========================================================================
    // 1. PRE-LOADED DEFAULT SAMPLE LIST OF 12 PRACTICAL ENGLISH WORDS
    // =========================================================================
    const TODAY_STR = new Date().toISOString().split('T')[0];
    const SAMPLE_DECK_ID = 'sample_daily_vocab_set';

    const SAMPLE_DECK = {
      id: SAMPLE_DECK_ID,
      date: TODAY_STR,
      topic: '精選實用高頻詞彙 (12詞)',
      words: [
        {
          word: 'mitigate',
          pos: 'v.',
          ipa: '/ˈmɪt.ɪ.ɡeɪt/',
          zhMeaning: '減輕、緩和（風險、損害或痛苦）',
          phrases: [
            { en: 'mitigate the risk of delay', zh: '降低延誤的風險' },
            { en: 'mitigate environmental impact', zh: '減輕對環境的衝擊' },
            { en: 'mitigate financial losses', zh: '減少財務損失' }
          ],
          example: {
            en: 'Diversifying our supplier network will help mitigate the risk of delay during peak seasons.',
            zh: '使供應商網絡多元化將有助於降低旺季期間延誤的風險。'
          },
          mastered: false
        },
        {
          word: 'leverage',
          pos: 'v.',
          ipa: '/ˈliː.vər.ɪdʒ/',
          zhMeaning: '善用、充分利用（資源、技術或優勢）',
          phrases: [
            { en: 'leverage existing resources', zh: '善用現有資源' },
            { en: 'leverage data analytics', zh: '運用數據分析' },
            { en: 'leverage strategic partnerships', zh: '善用策略合作夥伴關係' }
          ],
          example: {
            en: 'The engineering team plans to leverage data analytics to improve system reliability.',
            zh: '工程團隊計畫運用數據分析來提升系統可靠性。'
          },
          mastered: false
        },
        {
          word: 'feasible',
          pos: 'adj.',
          ipa: '/ˈfiː.zə.bəl/',
          zhMeaning: '可行的、切實可行的',
          phrases: [
            { en: 'economically feasible', zh: '在經濟上可行的' },
            { en: 'technically feasible solution', zh: '技術上可行的解決方案' },
            { en: 'prove to be feasible', zh: '證實是可行的' }
          ],
          example: {
            en: 'After rigorous testing, the prototype proved to be a technically feasible solution.',
            zh: '經過嚴格測試後，該原型被證實是一個技術上可行的解決方案。'
          },
          mastered: false
        },
        {
          word: 'bottleneck',
          pos: 'n.',
          ipa: '/ˈbɒt.əl.nek/',
          zhMeaning: '瓶頸、阻礙進展的環節',
          phrases: [
            { en: 'eliminate a bottleneck', zh: '消除瓶頸' },
            { en: 'cause a severe bottleneck', zh: '造成嚴重瓶頸' },
            { en: 'production bottleneck', zh: '生產瓶頸' }
          ],
          example: {
            en: 'Automating quality assurance helped eliminate a bottleneck in our release pipeline.',
            zh: '自動化品質保證協助消除了我們發布流程中的瓶頸。'
          },
          mastered: false
        },
        {
          word: 'consensus',
          pos: 'n.',
          ipa: '/kənˈsen.səs/',
          zhMeaning: '共識、一致意見',
          phrases: [
            { en: 'reach a consensus', zh: '達成共識' },
            { en: 'build broad consensus', zh: '建立廣泛共識' },
            { en: 'by mutual consensus', zh: '經雙方一致同意' }
          ],
          example: {
            en: 'Stakeholders finally reached a consensus on the annual budget allocation.',
            zh: '利害關係人終於就年度預算分配達成了共識。'
          },
          mastered: false
        },
        {
          word: 'streamline',
          pos: 'v.',
          ipa: '/ˈstriːm.laɪn/',
          zhMeaning: '精簡、使（流程或系統）更有效率',
          phrases: [
            { en: 'streamline operations', zh: '精簡營運流程' },
            { en: 'streamline the approval process', zh: '簡化審批程序' },
            { en: 'streamline workflow', zh: '優化工作流程' }
          ],
          example: {
            en: 'The new cloud platform was designed to streamline operations across regional offices.',
            zh: '新的雲端平台旨在精簡各地區辦事處的營運流程。'
          },
          mastered: false
        },
        {
          word: 'allocate',
          pos: 'v.',
          ipa: '/ˈæl.ə.keɪt/',
          zhMeaning: '分配、撥出（資金、時間或資源）',
          phrases: [
            { en: 'allocate funds to', zh: '撥款給……' },
            { en: 'allocate resources efficiently', zh: '有效分配資源' },
            { en: 'allocate time for review', zh: '撥出時間進行審查' }
          ],
          example: {
            en: 'Management decided to allocate funds to cybersecurity infrastructure upgrades.',
            zh: '管理層決定撥款用於網路安全基礎設施升級。'
          },
          mastered: false
        },
        {
          word: 'scrutinize',
          pos: 'v.',
          ipa: '/ˈskruː.tɪ.naɪz/',
          zhMeaning: '仔細審查、嚴格檢視',
          phrases: [
            { en: 'scrutinize the contract', zh: '仔細審閱合約' },
            { en: 'closely scrutinize data', zh: '嚴密檢視數據' },
            { en: 'scrutinize financial reports', zh: '審查財務報告' }
          ],
          example: {
            en: 'Legal advisors will scrutinize the contract before either party signs it.',
            zh: '法律顧問將在雙方簽署前仔細審閱合約。'
          },
          mastered: false
        },
        {
          word: 'benchmark',
          pos: 'n.',
          ipa: '/ˈbentʃ.mɑːk/',
          zhMeaning: '基準、標竿、衡量標準',
          phrases: [
            { en: 'industry benchmark', zh: '業界標竿／標準' },
            { en: 'set a new benchmark', zh: '樹立新標竿' },
            { en: 'performance benchmark', zh: '效能基準' }
          ],
          example: {
            en: 'Their low-latency processing engine has set a new industry benchmark.',
            zh: '他們的低延遲處理引擎已樹立了新的業界標竿。'
          },
          mastered: false
        },
        {
          word: 'resilient',
          pos: 'adj.',
          ipa: '/rɪˈzɪl.jənt/',
          zhMeaning: '有韌性的、適應力強的、能迅速恢復的',
          phrases: [
            { en: 'resilient infrastructure', zh: '具韌性的基礎設施' },
            { en: 'build a resilient supply chain', zh: '打造具韌性的供應鏈' },
            { en: 'economically resilient', zh: '具備經濟韌性的' }
          ],
          example: {
            en: 'Investing in resilient infrastructure ensures uninterrupted service during peak traffic.',
            zh: '投資具韌性的基礎設施可確保在流量高峰期提供不間斷的服務。'
          },
          mastered: false
        },
        {
          word: 'stipulate',
          pos: 'v.',
          ipa: '/ˈstɪp.jə.leɪt/',
          zhMeaning: '明文規定、約定（條款或條件）',
          phrases: [
            { en: 'stipulate in the agreement', zh: '在協議中明文規定' },
            { en: 'clearly stipulate requirements', zh: '明確規定要求' },
            { en: 'laws stipulate that', zh: '法律規定……' }
          ],
          example: {
            en: 'The service level agreement clearly stipulates in the agreement that uptime must exceed 99.9%.',
            zh: '服務水準協議中明確規定系統正常運行時間必須超過 99.9%。'
          },
          mastered: false
        },
        {
          word: 'contingency',
          pos: 'n.',
          ipa: '/kənˈtɪn.dʒən.si/',
          zhMeaning: '應變方案、突發事件、意外開支',
          phrases: [
            { en: 'contingency plan', zh: '應變計畫／備案' },
            { en: 'contingency fund', zh: '應急準備金' },
            { en: 'prepare for any contingency', zh: '為任何突發狀況做好準備' }
          ],
          example: {
            en: 'Every project manager must maintain an updated disaster recovery contingency plan.',
            zh: '每位專案經理都必須維護最新的災難復原應變計畫。'
          },
          mastered: false
        }
      ]
    };

    // =========================================================================
    // 2. STATE MANAGEMENT & LOCALSTORAGE PERSISTENCE
    // =========================================================================
    const STORAGE_KEY_DECKS = 'daily_vocab_crossword_decks_v2';
    const STORAGE_KEY_ACTIVE = 'daily_vocab_crossword_active_v2';

    let decks = {};
    let activeDeckId = SAMPLE_DECK_ID;

    // Flashcards View State
    let fcIndex = 0;
    let fcFlipped = false;
    let fcFilterMode = 'all'; // 'all' | 'review'
    let fcFilteredIndices = [];

    // Crossword View State
    let cwState = {
      grid: [],
      rows: 0,
      cols: 0,
      placedWords: [],
      selectedCell: null,
      direction: 'across',
      completed: false
    };

    // Normalize legacy or custom entries to the standard schema
    function normalizeWordEntry(item) {
      const phrases = Array.isArray(item.phrases) && item.phrases.length > 0
        ? item.phrases.map(p => ({ en: p.en || '', zh: p.zh || '' })).filter(p => p.en)
        : item.phrase
          ? [{ en: item.phrase, zh: item.phrase_zh || '' }]
          : [{ en: item.word, zh: '' }];

      const exampleObj = item.example && typeof item.example === 'object'
        ? { en: item.example.en || '', zh: item.example.zh || '' }
        : { en: item.example_en || '', zh: item.example_zh || '' };

      return {
        word: (item.word || '').trim().toLowerCase(),
        pos: item.pos || 'word',
        ipa: item.ipa || item.phonetic || '',
        zhMeaning: item.zhMeaning || item.meaning_zh || '',
        phrases,
        example: exampleObj,
        mastered: Boolean(item.mastered)
      };
    }

    function loadAppState() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY_DECKS);
        if (raw) {
          const parsed = JSON.parse(raw);
          Object.keys(parsed).forEach(k => {
            parsed[k].words = (parsed[k].words || []).map(normalizeWordEntry);
          });
          decks = parsed;
        }
      } catch (err) {
        console.error('Failed to load state:', err);
      }

      if (!decks || Object.keys(decks).length === 0) {
        decks = { [SAMPLE_DECK_ID]: JSON.parse(JSON.stringify(SAMPLE_DECK)) };
        saveAppState();
      }

      const savedId = localStorage.getItem(STORAGE_KEY_ACTIVE);
      activeDeckId = (savedId && decks[savedId]) ? savedId : Object.keys(decks)[0];
    }

    function saveAppState() {
      try {
        localStorage.setItem(STORAGE_KEY_DECKS, JSON.stringify(decks));
        localStorage.setItem(STORAGE_KEY_ACTIVE, activeDeckId);
      } catch (err) {
        showToast('儲存失敗：瀏覽器儲存空間不足', 'error');
      }
    }

    function getActiveDeck() {
      return decks[activeDeckId] || Object.values(decks)[0];
    }

    // =========================================================================
    // 3. TOAST & WEB SPEECH API (US / UK PRONUNCIATION)
    // =========================================================================
    function showToast(message, type = 'info') {
      const container = document.getElementById('toastContainer');
      const toast = document.createElement('div');
      const colors = {
        info: 'bg-slate-900 text-white',
        success: 'bg-emerald-600 text-white',
        warning: 'bg-amber-600 text-white',
        error: 'bg-rose-600 text-white'
      };
      toast.className = `${colors[type] || colors.info} px-4 py-3 rounded-2xl shadow-lg text-xs sm:text-sm font-bold transition-all duration-300 transform translate-y-2 opacity-0 pointer-events-auto`;
      toast.textContent = message;
      container.appendChild(toast);

      requestAnimationFrame(() => toast.classList.remove('translate-y-2', 'opacity-0'));
      setTimeout(() => {
        toast.classList.add('opacity-0', 'translate-y-2');
        setTimeout(() => toast.remove(), 300);
      }, 3000);
    }

    function speakText(text, lang = 'en-US') {
      if (!('speechSynthesis' in window)) {
        showToast('您的瀏覽器不支援語音朗讀功能', 'warning');
        return;
      }
      window.speechSynthesis.cancel();
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = lang;
      utterance.rate = 0.95;

      const voices = window.speechSynthesis.getVoices();
      const matchedVoice = voices.find(v => v.lang.replace('_', '-') === lang) ||
                           voices.find(v => v.lang.startsWith('en'));
      if (matchedVoice) utterance.voice = matchedVoice;
      window.speechSynthesis.speak(utterance);
    }

    // =========================================================================
    // 4. DECK SWITCHING & SYNCHRONIZATION
    // =========================================================================
    function renderGlobalDeckSelector() {
      const selector = document.getElementById('globalDeckSelector');
      selector.innerHTML = '';

      const sorted = Object.values(decks).sort((a, b) => b.date.localeCompare(a.date));
      sorted.forEach(deck => {
        const opt = document.createElement('option');
        opt.value = deck.id;
        const topicLabel = deck.topic ? ` · ${deck.topic}` : '';
        opt.textContent = `${deck.date}${topicLabel} (${deck.words.length}詞)`;
        if (deck.id === activeDeckId) opt.selected = true;
        selector.appendChild(opt);
      });
    }

    function switchActiveDeck(newDeckId) {
      if (!decks[newDeckId]) return;
      activeDeckId = newDeckId;
      saveAppState();
      renderGlobalDeckSelector();

      fcIndex = 0;
      fcFlipped = false;
      recomputeFilteredFlashcards();
      renderFlashcardsView();
      renderDeckEditorList();
      initCrosswordPuzzle();
    }

    // =========================================================================
    // 5. TAB 1: DAILY WORD LIST MANAGER & GEMINI AI GENERATOR
    // =========================================================================
    function parseInputWordList(rawText) {
      const tokens = rawText
        .split(/[\s,，;；、\n\r\t]+/)
        .map(w => w.trim().toLowerCase().replace(/[^a-z-]/g, ''))
        .filter(w => w.length >= 2);
      return [...new Set(tokens)];
    }

    async function fetchWordDetailsFromGemini(wordList) {
      const apiKey = ""; // Leave empty; runtime injects the key automatically
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

      const systemPrompt = `你是一位專精於繁體中文英語教學的資深詞彙導師。
請為使用者輸入的英文單字列表生成結構化 JSON 學習資料。
嚴格要求：
1. zhMeaning、phrases 中的 zh 與 example 中的 zh 必須使用精準、自然道地的「繁體中文」。
2. phrases 陣列必須為每個單字提供 2 至 3 組最核心的英文常用片語搭配（Collocations），且每組片語的 en 欄位必須包含該目標英文單字本身。
3. example 提供一句實用自然的英文例句 (en) 與繁體中文翻譯 (zh)。`;

      const userQuery = `請為以下 ${wordList.length} 個英文單字生成完整學習卡與常用片語資料：\n${wordList.join(', ')}`;

      const payload = {
        contents: [{ parts: [{ text: userQuery }] }],
        systemInstruction: { parts: [{ text: systemPrompt }] },
        generationConfig: {
          responseMimeType: "application/json",
          responseSchema: {
            type: "ARRAY",
            items: {
              type: "OBJECT",
              properties: {
                word: { type: "STRING" },
                pos: { type: "STRING" },
                ipa: { type: "STRING" },
                zhMeaning: { type: "STRING" },
                phrases: {
                  type: "ARRAY",
                  items: {
                    type: "OBJECT",
                    properties: {
                      en: { type: "STRING" },
                      zh: { type: "STRING" }
                    },
                    required: ["en", "zh"]
                  }
                },
                example: {
                  type: "OBJECT",
                  properties: {
                    en: { type: "STRING" },
                    zh: { type: "STRING" }
                  },
                  required: ["en", "zh"]
                }
              },
              required: ["word", "pos", "ipa", "zhMeaning", "phrases", "example"]
            }
          }
        }
      };

      const backoffDelays = [1000, 2000, 4000];
      for (let attempt = 0; attempt <= backoffDelays.length; attempt++) {
        try {
          const response = await fetch(apiUrl, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(payload)
          });

          if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
          }

          const result = await response.json();
          const jsonText = result?.candidates?.[0]?.content?.parts?.[0]?.text;
          if (!jsonText) throw new Error('AI 回傳資料為空');

          const parsedArray = JSON.parse(jsonText);
          return parsedArray.map(item => normalizeWordEntry(item));
        } catch (err) {
          if (attempt === backoffDelays.length) throw err;
          await new Promise(resolve => setTimeout(resolve, backoffDelays[attempt]));
        }
      }
    }

    function renderDeckEditorList() {
      const deck = getActiveDeck();
      const titleSuffix = deck.topic ? ` · ${deck.topic}` : '';
      document.getElementById('activeDeckTableTitle').textContent = `單字庫明細：${deck.date}${titleSuffix} (${deck.words.length} 詞)`;

      const container = document.getElementById('deckWordsEditorList');
      container.innerHTML = '';

      deck.words.forEach((item, index) => {
        const card = document.createElement('div');
        card.className = 'p-4 rounded-2xl border border-slate-200 bg-slate-50/70 hover:bg-white transition flex flex-col sm:flex-row sm:items-center justify-between gap-3';

        const phrasesHtml = (item.phrases || []).map(p =>
          `<span class="inline-flex items-center gap-1 bg-amber-100/80 text-amber-950 px-2.5 py-0.5 rounded-lg text-xs font-semibold">
            <span>${p.en}</span>
            <span class="text-amber-700">(${p.zh})</span>
          </span>`
        ).join('');

        card.innerHTML = `
          <div class="space-y-2 min-w-0">
            <div class="flex flex-wrap items-center gap-2">
              <span class="font-extrabold text-base text-slate-900">${item.word}</span>
              <span class="text-xs font-bold px-2 py-0.5 rounded-md bg-indigo-50 text-indigo-700">${item.pos}</span>
              <span class="font-mono text-xs text-slate-500">${item.ipa}</span>
              <span class="text-sm font-bold text-indigo-700">${item.zhMeaning}</span>
              ${item.mastered ? '<span class="text-[11px] font-bold px-2 py-0.5 rounded-full bg-emerald-100 text-emerald-800">已掌握</span>' : ''}
            </div>
            <div class="flex flex-wrap gap-1.5">${phrasesHtml}</div>
          </div>
          <div class="flex items-center gap-2 shrink-0">
            <button type="button" class="edit-entry-btn px-3 py-1.5 rounded-xl bg-white hover:bg-indigo-50 text-indigo-700 border border-slate-200 text-xs font-bold transition">編輯</button>
            <button type="button" class="delete-entry-btn px-3 py-1.5 rounded-xl bg-white hover:bg-rose-50 text-rose-600 border border-slate-200 text-xs font-bold transition">刪除</button>
          </div>
        `;

        card.querySelector('.edit-entry-btn').addEventListener('click', () => openWordModal(index));
        card.querySelector('.delete-entry-btn').addEventListener('click', () => {
          if (deck.words.length <= 2) {
            showToast('單字庫至少需保留 2 個單字以生成填字遊戲', 'warning');
            return;
          }
          deck.words.splice(index, 1);
          saveAppState();
          renderGlobalDeckSelector();
          recomputeFilteredFlashcards();
          renderFlashcardsView();
          renderDeckEditorList();
          initCrosswordPuzzle();
          showToast(`已刪除單字「${item.word}」`, 'info');
        });

        container.appendChild(card);
      });
    }

    function openWordModal(wordIdx = -1) {
      const modal = document.getElementById('wordModal');
      const deck = getActiveDeck();
      document.getElementById('modalWordIndex').value = wordIdx;

      if (wordIdx >= 0 && deck.words[wordIdx]) {
        const entry = deck.words[wordIdx];
        document.getElementById('wordModalHeading').textContent = `編輯單字：${entry.word}`;
        document.getElementById('modalWordInput').value = entry.word;
        document.getElementById('modalPosInput').value = entry.pos || '';
        document.getElementById('modalIpaInput').value = entry.ipa || '';
        document.getElementById('modalMeaningZhInput').value = entry.zhMeaning || '';

        const p = entry.phrases || [];
        document.getElementById('modalPhraseEn1').value = p[0]?.en || '';
        document.getElementById('modalPhraseZh1').value = p[0]?.zh || '';
        document.getElementById('modalPhraseEn2').value = p[1]?.en || '';
        document.getElementById('modalPhraseZh2').value = p[1]?.zh || '';
        document.getElementById('modalPhraseEn3').value = p[2]?.en || '';
        document.getElementById('modalPhraseZh3').value = p[2]?.zh || '';

        document.getElementById('modalExampleEnInput').value = entry.example?.en || '';
        document.getElementById('modalExampleZhInput').value = entry.example?.zh || '';
      } else {
        document.getElementById('wordModalHeading').textContent = '新增單字與常用片語';
        document.getElementById('wordModalForm').reset();
        document.getElementById('modalWordIndex').value = -1;
      }

      modal.classList.remove('hidden');
    }

    function closeWordModal() {
      document.getElementById('wordModal').classList.add('hidden');
    }

    // =========================================================================
    // 6. TAB 2: 3D INTERACTIVE FLASHCARDS
    // =========================================================================
    function recomputeFilteredFlashcards() {
      const deck = getActiveDeck();
      fcFilteredIndices = [];
      deck.words.forEach((w, idx) => {
        if (fcFilterMode === 'all' || !w.mastered) {
          fcFilteredIndices.push(idx);
        }
      });
      if (fcIndex >= fcFilteredIndices.length) {
        fcIndex = Math.max(0, fcFilteredIndices.length - 1);
      }
    }

    function renderFlashcardsView() {
      const deck = getActiveDeck();
      const total = deck.words.length;
      const masteredCount = deck.words.filter(w => w.mastered).length;
      const pct = total > 0 ? Math.round((masteredCount / total) * 100) : 0;

      const titleStr = deck.topic ? `${deck.date} ${deck.topic}` : deck.date;
      document.getElementById('fcDeckHeader').textContent = `${titleStr} (${total} 詞)`;
      document.getElementById('fcMasterySummary').textContent = `已掌握 ${masteredCount} / ${total} (${pct}%)`;
      document.getElementById('fcProgressBar').style.width = `${pct}%`;

      // Filter toggle styles
      const allBtn = document.getElementById('filterAllBtn');
      const revBtn = document.getElementById('filterReviewBtn');
      if (fcFilterMode === 'all') {
        allBtn.className = 'px-3 py-1.5 rounded-lg text-xs font-bold bg-white text-indigo-700 shadow-2xs transition';
        revBtn.className = 'px-3 py-1.5 rounded-lg text-xs font-bold text-slate-600 hover:text-slate-900 transition';
      } else {
        revBtn.className = 'px-3 py-1.5 rounded-lg text-xs font-bold bg-white text-amber-700 shadow-2xs transition';
        allBtn.className = 'px-3 py-1.5 rounded-lg text-xs font-bold text-slate-600 hover:text-slate-900 transition';
      }

      // Quick jump word pills
      const pillsBar = document.getElementById('fcPillsBar');
      pillsBar.innerHTML = '';
      deck.words.forEach((w, rawIdx) => {
        const isCurrent = fcFilteredIndices[fcIndex] === rawIdx;
        const pill = document.createElement('button');
        pill.type = 'button';
        pill.className = `px-3 py-1 rounded-xl text-xs font-bold border transition flex items-center gap-1 ${
          isCurrent
            ? 'bg-indigo-600 text-white border-indigo-600 shadow-xs'
            : w.mastered
              ? 'bg-emerald-50 text-emerald-700 border-emerald-200 hover:bg-emerald-100'
              : 'bg-slate-50 text-slate-700 border-slate-200 hover:bg-slate-100'
        }`;
        pill.innerHTML = `<span>${w.word}</span>${w.mastered ? '<span>✓</span>' : ''}`;
        pill.addEventListener('click', () => {
          if (fcFilterMode === 'review' && w.mastered) {
            fcFilterMode = 'all';
            recomputeFilteredFlashcards();
          }
          const pos = fcFilteredIndices.indexOf(rawIdx);
          if (pos !== -1) {
            fcIndex = pos;
            fcFlipped = false;
            renderFlashcardsView();
          }
        });
        pillsBar.appendChild(pill);
      });

      const emptyState = document.getElementById('fcEmptyFilterState');
      const mainStage = document.getElementById('fcMainStage');

      if (fcFilteredIndices.length === 0) {
        emptyState.classList.remove('hidden');
        mainStage.classList.add('hidden');
        return;
      } else {
        emptyState.classList.add('hidden');
        mainStage.classList.remove('hidden');
      }

      const activeRawIdx = fcFilteredIndices[fcIndex];
      const item = deck.words[activeRawIdx];

      document.getElementById('fcPositionLabel').textContent = `第 ${fcIndex + 1} / ${fcFilteredIndices.length} 張`;

      const card3D = document.getElementById('flashcard3D');
      if (fcFlipped) card3D.classList.add('rotate-y-180');
      else card3D.classList.remove('rotate-y-180');

      // Front of card
      document.getElementById('cardFrontWord').textContent = item.word;
      document.getElementById('cardFrontPos').textContent = item.pos || 'word';
      document.getElementById('cardFrontIpa').textContent = item.ipa || '';

      const statusBadge = document.getElementById('cardFrontStatus');
      if (item.mastered) {
        statusBadge.textContent = '✓ 已掌握';
        statusBadge.className = 'px-3 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800';
      } else {
        statusBadge.textContent = '仍需複習';
        statusBadge.className = 'px-3 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-800';
      }

      // Back of card
      document.getElementById('cardBackWord').textContent = item.word;
      document.getElementById('cardBackPos').textContent = item.pos || '';
      document.getElementById('cardBackIpa').textContent = item.ipa || '';
      document.getElementById('cardBackMeaning').textContent = item.zhMeaning;

      // Render 2-3 Common Phrases with individual audio buttons
      const phrasesListEl = document.getElementById('cardBackPhrasesList');
      phrasesListEl.innerHTML = '';
      (item.phrases || []).forEach(p => {
        const row = document.createElement('div');
        row.className = 'flex items-center justify-between gap-2 bg-white/90 px-3 py-2 rounded-xl border border-amber-200/80';
        row.innerHTML = `
          <div class="min-w-0">
            <p class="text-xs sm:text-sm font-extrabold text-slate-900 leading-snug">${p.en}</p>
            <p class="text-xs font-semibold text-amber-900">${p.zh}</p>
          </div>
          <button type="button" class="speak-phrase-item-btn shrink-0 p-2 rounded-xl bg-amber-500 hover:bg-amber-600 text-white shadow-2xs transition" title="朗讀片語">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-3.5 h-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M15.536 8.464a5 5 0 010 7.072M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
            </svg>
          </button>
        `;
        row.querySelector('.speak-phrase-item-btn').addEventListener('click', (e) => {
          e.stopPropagation();
          speakText(p.en, 'en-US');
        });
        phrasesListEl.appendChild(row);
      });

      document.getElementById('cardBackExampleEn').textContent = item.example?.en || '';
      document.getElementById('cardBackExampleZh').textContent = item.example?.zh || '';
    }

    function updateFlashcardMastery(isMastered) {
      if (fcFilteredIndices.length === 0) return;
      const deck = getActiveDeck();
      const rawIdx = fcFilteredIndices[fcIndex];
      deck.words[rawIdx].mastered = isMastered;
      saveAppState();
      renderDeckEditorList();

      if (fcFilterMode === 'review' && isMastered) {
        recomputeFilteredFlashcards();
        fcFlipped = false;
      } else if (fcIndex < fcFilteredIndices.length - 1) {
        fcIndex++;
        fcFlipped = false;
      }
      renderFlashcardsView();
    }

    // =========================================================================
    // 7. TAB 3: PHRASE-CONTEXT CROSSWORD PUZZLE GENERATOR & INTERACTIVE BOARD
    // =========================================================================
    function formatPhraseContextClue(item) {
      const meaning = item.zhMeaning || '';
      const firstPhrase = (item.phrases && item.phrases[0]) ? item.phrases[0] : null;
      if (firstPhrase && firstPhrase.en) {
        const regex = new RegExp(item.word, 'i');
        const blanked = regex.test(firstPhrase.en)
          ? firstPhrase.en.replace(regex, '______')
          : `______ (${firstPhrase.en})`;
        const zhNote = firstPhrase.zh ? `（${firstPhrase.zh}）` : '';
        return `${meaning} ｜ 片語提示：${blanked}${zhNote}`;
      }
      return `${meaning} [${item.pos || 'word'}]`;
    }

    function buildCrosswordLayout(wordsArray) {
      const entries = wordsArray
        .map((item, idx) => ({
          rawIdx: idx,
          word: item.word.toUpperCase().replace(/[^A-Z]/g, ''),
          clue: formatPhraseContextClue(item)
        }))
        .filter(w => w.word.length >= 2);

      if (entries.length === 0) return null;

      let bestBoard = null;
      let bestScore = -Infinity;

      // Run 30 randomized greedy intersection layouts to produce a compact, high-overlap crossword
      for (let trial = 0; trial < 30; trial++) {
        const sorted = [...entries].sort((a, b) => {
          if (trial === 0) return b.word.length - a.word.length;
          return (b.word.length - a.word.length) + (Math.random() * 4 - 2);
        });

        const SIZE = 40;
        const matrix = Array.from({ length: SIZE }, () => Array(SIZE).fill(null));
        const placed = [];

        function canPlace(word, r, c, dir) {
          const dr = dir === 'down' ? 1 : 0;
          const dc = dir === 'across' ? 1 : 0;

          if (r < 1 || c < 1 || r + dr * word.length >= SIZE - 1 || c + dc * word.length >= SIZE - 1) {
            return { valid: false, intersections: 0 };
          }
          if (matrix[r - dr][c - dc] !== null || matrix[r + dr * word.length][c + dc * word.length] !== null) {
            return { valid: false, intersections: 0 };
          }

          let intersections = 0;
          for (let i = 0; i < word.length; i++) {
            const cr = r + dr * i;
            const cc = c + dc * i;
            const existing = matrix[cr][cc];
            if (existing !== null) {
              if (existing !== word[i]) return { valid: false, intersections: 0 };
              intersections++;
            } else {
              if (dir === 'across') {
                if (matrix[cr - 1][cc] !== null || matrix[cr + 1][cc] !== null) return { valid: false, intersections: 0 };
              } else {
                if (matrix[cr][cc - 1] !== null || matrix[cr][cc + 1] !== null) return { valid: false, intersections: 0 };
              }
            }
          }
          return { valid: true, intersections };
        }

        function placeWord(entry, r, c, dir) {
          const dr = dir === 'down' ? 1 : 0;
          const dc = dir === 'across' ? 1 : 0;
          for (let i = 0; i < entry.word.length; i++) {
            matrix[r + dr * i][c + dc * i] = entry.word[i];
          }
          placed.push({ ...entry, row: r, col: c, dir });
        }

        const first = sorted[0];
        placeWord(first, Math.floor(SIZE / 2), Math.floor((SIZE - first.word.length) / 2), 'across');

        const unplaced = sorted.slice(1);
        let placedAny = true;
        while (placedAny && unplaced.length > 0) {
          placedAny = false;
          for (let u = 0; u < unplaced.length; u++) {
            const candidate = unplaced[u];
            const validSpots = [];

            for (const p of placed) {
              const targetDir = p.dir === 'across' ? 'down' : 'across';
              for (let i = 0; i < p.word.length; i++) {
                const pr = p.row + (p.dir === 'down' ? i : 0);
                const pc = p.col + (p.dir === 'across' ? i : 0);
                for (let j = 0; j < candidate.word.length; j++) {
                  if (candidate.word[j] === p.word[i]) {
                    const cr = pr - (targetDir === 'down' ? j : 0);
                    const cc = pc - (targetDir === 'across' ? j : 0);
                    const check = canPlace(candidate.word, cr, cc, targetDir);
                    if (check.valid && check.intersections > 0) {
                      validSpots.push({ r: cr, c: cc, dir: targetDir, intersections: check.intersections });
                    }
                  }
                }
              }
            }

            if (validSpots.length > 0) {
              validSpots.sort((a, b) => b.intersections - a.intersections || (Math.random() - 0.5));
              placeWord(candidate, validSpots[0].r, validSpots[0].c, validSpots[0].dir);
              unplaced.splice(u, 1);
              placedAny = true;
              break;
            }
          }
        }

        // Fallback placement so every word in the day's deck is included
        for (const rem of unplaced) {
          let inserted = false;
          for (let r = 2; r < SIZE - 2 && !inserted; r += 2) {
            for (let c = 2; c < SIZE - rem.word.length - 2 && !inserted; c++) {
              if (canPlace(rem.word, r, c, 'across').valid) {
                placeWord(rem, r, c, 'across');
                inserted = true;
              }
            }
          }
        }

        let minR = SIZE, maxR = 0, minC = SIZE, maxC = 0;
        placed.forEach(p => {
          const endR = p.row + (p.dir === 'down' ? p.word.length - 1 : 0);
          const endC = p.col + (p.dir === 'across' ? p.word.length - 1 : 0);
          minR = Math.min(minR, p.row);
          maxR = Math.max(maxR, endR);
          minC = Math.min(minC, p.col);
          maxC = Math.max(maxC, endC);
        });

        const area = (maxR - minR + 1) * (maxC - minC + 1);
        const connectedWords = placed.length - unplaced.length;
        const score = connectedWords * 120 - area * 0.25;

        if (score > bestScore) {
          bestScore = score;
          bestBoard = { placed, minR, maxR, minC, maxC };
        }
      }

      const { placed, minR, maxR, minC, maxC } = bestBoard;
      const rows = maxR - minR + 1;
      const cols = maxC - minC + 1;
      const grid = Array.from({ length: rows }, () => Array(cols).fill(null));

      placed.forEach((p, idx) => {
        p.id = idx + 1;
        p.row -= minR;
        p.col -= minC;
        for (let i = 0; i < p.word.length; i++) {
          const r = p.row + (p.dir === 'down' ? i : 0);
          const c = p.col + (p.dir === 'across' ? i : 0);
          if (!grid[r][c]) {
            grid[r][c] = {
              letter: p.word[i],
              userLetter: '',
              number: null,
              acrossWordId: null,
              downWordId: null,
              status: 'neutral' // 'neutral' | 'correct' | 'wrong'
            };
          }
          if (p.dir === 'across') grid[r][c].acrossWordId = p.id;
          if (p.dir === 'down') grid[r][c].downWordId = p.id;
        }
      });

      let clueNum = 1;
      for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
          if (!grid[r][c]) continue;
          const startsAcross = placed.find(p => p.dir === 'across' && p.row === r && p.col === c);
          const startsDown = placed.find(p => p.dir === 'down' && p.row === r && p.col === c);
          if (startsAcross || startsDown) {
            grid[r][c].number = clueNum;
            if (startsAcross) startsAcross.number = clueNum;
            if (startsDown) startsDown.number = clueNum;
            clueNum++;
          }
        }
      }

      return { grid, rows, cols, placedWords: placed };
    }

    function initCrosswordPuzzle() {
      const deck = getActiveDeck();
      const layout = buildCrosswordLayout(deck.words);
      if (!layout) return;

      cwState = {
        grid: layout.grid,
        rows: layout.rows,
        cols: layout.cols,
        placedWords: layout.placedWords,
        selectedCell: null,
        direction: 'across',
        completed: false
      };

      document.getElementById('cwCompleteBanner').classList.add('hidden');
      const firstWord = [...cwState.placedWords].sort((a, b) => a.number - b.number)[0];
      if (firstWord) {
        cwState.selectedCell = { r: firstWord.row, c: firstWord.col };
        cwState.direction = firstWord.dir;
      }
      renderCrosswordView();
    }

    function getActiveCrosswordWord() {
      if (!cwState.selectedCell) return null;
      const { r, c } = cwState.selectedCell;
      const cell = cwState.grid[r]?.[c];
      if (!cell) return null;

      let wordId = cwState.direction === 'across' ? cell.acrossWordId : cell.downWordId;
      if (!wordId) {
        wordId = cell.acrossWordId || cell.downWordId;
        if (wordId) cwState.direction = cell.acrossWordId ? 'across' : 'down';
      }
      return cwState.placedWords.find(w => w.id === wordId) || null;
    }

    function renderCrosswordView() {
      const container = document.getElementById('cwGridContainer');
      container.innerHTML = '';

      const activeWord = getActiveCrosswordWord();
      const activeCoords = new Set();
      if (activeWord) {
        for (let i = 0; i < activeWord.word.length; i++) {
          const wr = activeWord.row + (activeWord.dir === 'down' ? i : 0);
          const wc = activeWord.col + (activeWord.dir === 'across' ? i : 0);
          activeCoords.add(`${wr},${wc}`);
        }
        document.getElementById('cwActiveBadge').textContent = `${activeWord.number} ${activeWord.dir === 'across' ? '橫向' : '直向'}`;
        document.getElementById('cwActiveClueText').textContent = `${activeWord.number}. ${activeWord.clue}`;
      }

      const cellDim = cwState.cols > 15
        ? 'w-7 h-7 sm:w-9 sm:h-9 text-sm sm:text-base'
        : 'w-8 h-8 sm:w-10 sm:h-10 text-base sm:text-lg';

      const gridEl = document.createElement('div');
      gridEl.className = 'grid gap-[2px] bg-slate-800';
      gridEl.style.gridTemplateColumns = `repeat(${cwState.cols}, minmax(0, 1fr))`;

      for (let r = 0; r < cwState.rows; r++) {
        for (let c = 0; c < cwState.cols; c++) {
          const cell = cwState.grid[r][c];
          if (!cell) {
            const blockDiv = document.createElement('div');
            blockDiv.className = `${cellDim} bg-slate-900/95 rounded-[3px]`;
            gridEl.appendChild(blockDiv);
            continue;
          }

          const isSelected = cwState.selectedCell && cwState.selectedCell.r === r && cwState.selectedCell.c === c;
          const isWordHighlighted = activeCoords.has(`${r},${c}`);

          let cellPalette = 'bg-white text-slate-900';
          if (cell.status === 'correct') {
            cellPalette = isSelected ? 'bg-emerald-300 text-emerald-950' : 'bg-emerald-100 text-emerald-900';
          } else if (cell.status === 'wrong') {
            cellPalette = isSelected ? 'bg-rose-300 text-rose-950' : 'bg-rose-100 text-rose-800';
          } else if (isSelected) {
            cellPalette = 'bg-amber-300 text-slate-950 ring-2 ring-indigo-600 z-10';
          } else if (isWordHighlighted) {
            cellPalette = 'bg-indigo-100 text-indigo-950';
          }

          const cellBox = document.createElement('div');
          cellBox.className = `relative ${cellDim} ${cellPalette} rounded-[4px] flex items-center justify-center transition-colors cursor-pointer select-none`;

          if (cell.number) {
            const numSpan = document.createElement('span');
            numSpan.className = 'absolute top-[1px] left-[3px] text-[9px] sm:text-[10px] font-bold leading-none text-slate-500 pointer-events-none';
            numSpan.textContent = cell.number;
            cellBox.appendChild(numSpan);
          }

          const input = document.createElement('input');
          input.type = 'text';
          input.maxLength = 2;
          input.autocomplete = 'off';
          input.autocapitalize = 'characters';
          input.spellcheck = false;
          input.dataset.r = r;
          input.dataset.c = c;
          input.value = cell.userLetter;
          input.className = 'w-full h-full bg-transparent text-center font-mono font-extrabold uppercase focus:outline-none cursor-pointer caret-transparent pt-1';

          input.addEventListener('pointerdown', () => {
            if (cwState.selectedCell && cwState.selectedCell.r === r && cwState.selectedCell.c === c) {
              if (cell.acrossWordId && cell.downWordId) {
                cwState.direction = cwState.direction === 'across' ? 'down' : 'across';
              }
            } else {
              cwState.selectedCell = { r, c };
              if (cwState.direction === 'across' && !cell.acrossWordId && cell.downWordId) {
                cwState.direction = 'down';
              } else if (cwState.direction === 'down' && !cell.downWordId && cell.acrossWordId) {
                cwState.direction = 'across';
              }
            }
            renderCrosswordView();
            setTimeout(() => focusCrosswordCell(r, c), 0);
          });

          input.addEventListener('input', (e) => {
            const chars = e.target.value.toUpperCase().replace(/[^A-Z]/g, '');
            if (chars.length > 0) {
              cell.userLetter = chars[chars.length - 1];
              cell.status = 'neutral';
              advanceCrosswordCursor(r, c, 1);
            } else {
              cell.userLetter = '';
              cell.status = 'neutral';
            }
            checkCrosswordVictory();
            renderCrosswordView();
            if (cwState.selectedCell) focusCrosswordCell(cwState.selectedCell.r, cwState.selectedCell.c);
          });

          input.addEventListener('keydown', (e) => {
            if (e.key === 'Backspace') {
              e.preventDefault();
              if (cell.userLetter !== '') {
                cell.userLetter = '';
                cell.status = 'neutral';
              } else {
                advanceCrosswordCursor(r, c, -1);
                if (cwState.selectedCell) {
                  const prevCell = cwState.grid[cwState.selectedCell.r][cwState.selectedCell.c];
                  if (prevCell) {
                    prevCell.userLetter = '';
                    prevCell.status = 'neutral';
                  }
                }
              }
              renderCrosswordView();
              if (cwState.selectedCell) focusCrosswordCell(cwState.selectedCell.r, cwState.selectedCell.c);
            } else if (e.key === 'ArrowRight') {
              e.preventDefault();
              cwState.direction = 'across';
              moveCrosswordArrow(r, c, 0, 1);
            } else if (e.key === 'ArrowLeft') {
              e.preventDefault();
              cwState.direction = 'across';
              moveCrosswordArrow(r, c, 0, -1);
            } else if (e.key === 'ArrowDown') {
              e.preventDefault();
              cwState.direction = 'down';
              moveCrosswordArrow(r, c, 1, 0);
            } else if (e.key === 'ArrowUp') {
              e.preventDefault();
              cwState.direction = 'down';
              moveCrosswordArrow(r, c, -1, 0);
            }
          });

          cellBox.appendChild(input);
          gridEl.appendChild(cellBox);
        }
      }

      container.appendChild(gridEl);
      renderCrosswordClues(activeWord);
    }

    function focusCrosswordCell(r, c) {
      const el = document.querySelector(`#cwGridContainer input[data-r="${r}"][data-c="${c}"]`);
      if (el) el.focus();
    }

    function advanceCrosswordCursor(r, c, step) {
      const dr = cwState.direction === 'down' ? step : 0;
      const dc = cwState.direction === 'across' ? step : 0;
      const nr = r + dr;
      const nc = c + dc;
      if (cwState.grid[nr]?.[nc]) {
        cwState.selectedCell = { r: nr, c: nc };
      }
    }

    function moveCrosswordArrow(r, c, dr, dc) {
      let nr = r + dr;
      let nc = c + dc;
      while (nr >= 0 && nr < cwState.rows && nc >= 0 && nc < cwState.cols) {
        if (cwState.grid[nr][nc]) {
          cwState.selectedCell = { r: nr, c: nc };
          renderCrosswordView();
          focusCrosswordCell(nr, nc);
          return;
        }
        nr += dr;
        nc += dc;
      }
    }

    function renderCrosswordClues(activeWord) {
      const acrossUl = document.getElementById('cwAcrossList');
      const downUl = document.getElementById('cwDownList');
      acrossUl.innerHTML = '';
      downUl.innerHTML = '';

      const sorted = [...cwState.placedWords].sort((a, b) => a.number - b.number);
      sorted.forEach(w => {
        let isSolved = true;
        for (let i = 0; i < w.word.length; i++) {
          const cr = w.row + (w.dir === 'down' ? i : 0);
          const cc = w.col + (w.dir === 'across' ? i : 0);
          if (cwState.grid[cr][cc].userLetter !== w.word[i]) {
            isSolved = false;
            break;
          }
        }

        const isActive = activeWord && activeWord.id === w.id;
        const li = document.createElement('li');
        li.className = `p-2.5 rounded-xl text-xs sm:text-sm cursor-pointer transition border ${
          isActive
            ? 'bg-indigo-50 border-indigo-300 text-indigo-950 font-semibold shadow-2xs'
            : isSolved
              ? 'bg-emerald-50/70 border-emerald-200 text-slate-400 line-through'
              : 'bg-slate-50/70 border-transparent hover:bg-slate-100 text-slate-700'
        }`;
        li.innerHTML = `
          <div class="flex items-start gap-2">
            <span class="font-mono font-extrabold text-indigo-600 shrink-0">${w.number}.</span>
            <span class="leading-snug">${w.clue} <span class="text-xs text-slate-400 font-mono">(${w.word.length}字母)</span></span>
          </div>
        `;
        li.addEventListener('click', () => {
          cwState.selectedCell = { r: w.row, c: w.col };
          cwState.direction = w.dir;
          renderCrosswordView();
          focusCrosswordCell(w.row, w.col);
        });

        if (w.dir === 'across') acrossUl.appendChild(li);
        else downUl.appendChild(li);
      });
    }

    function checkCrosswordVictory() {
      for (let r = 0; r < cwState.rows; r++) {
        for (let c = 0; c < cwState.cols; c++) {
          const cell = cwState.grid[r][c];
          if (cell && cell.userLetter !== cell.letter) return;
        }
      }
      if (!cwState.completed) {
        cwState.completed = true;
        for (let r = 0; r < cwState.rows; r++) {
          for (let c = 0; c < cwState.cols; c++) {
            if (cwState.grid[r][c]) cwState.grid[r][c].status = 'correct';
          }
        }
        document.getElementById('cwCompleteBanner').classList.remove('hidden');
        showToast('恭喜全部答對！填字遊戲完成！', 'success');
      }
    }

    // =========================================================================
    // 8. NAVIGATION & EVENT BINDINGS
    // =========================================================================
    function switchViewTab(tabName) {
      document.querySelectorAll('.nav-tab-btn').forEach(btn => {
        const isTarget = btn.dataset.tab === tabName;
        btn.className = isTarget
          ? 'nav-tab-btn flex items-center gap-2 px-4 py-2 rounded-xl text-xs sm:text-sm font-bold transition whitespace-nowrap bg-indigo-600 text-white shadow-sm'
          : 'nav-tab-btn flex items-center gap-2 px-4 py-2 rounded-xl text-xs sm:text-sm font-bold transition whitespace-nowrap bg-white text-slate-600 hover:bg-slate-100 border border-slate-200';
      });

      ['input', 'flashcards', 'crossword'].forEach(viewId => {
        const el = document.getElementById(`view-${viewId}`);
        if (viewId === tabName) el.classList.remove('hidden');
        else el.classList.add('hidden');
      });

      if (tabName === 'input') renderDeckEditorList();
      if (tabName === 'flashcards') renderFlashcardsView();
      if (tabName === 'crossword') renderCrosswordView();
    }

    document.addEventListener('DOMContentLoaded', () => {
      loadAppState();
      renderGlobalDeckSelector();

      document.getElementById('deckDateInput').value = TODAY_STR;

      recomputeFilteredFlashcards();
      renderFlashcardsView();
      renderDeckEditorList();
      initCrosswordPuzzle();

      // Mode navigation tabs
      document.querySelectorAll('.nav-tab-btn').forEach(btn => {
        btn.addEventListener('click', () => switchViewTab(btn.dataset.tab));
      });

      // Global daily deck switcher & delete
      document.getElementById('globalDeckSelector').addEventListener('change', (e) => {
        switchActiveDeck(e.target.value);
      });

      document.getElementById('deleteDeckBtn').addEventListener('click', () => {
        if (Object.keys(decks).length <= 1) {
          showToast('至少需保留一組日期單字庫', 'warning');
          return;
        }
        delete decks[activeDeckId];
        switchActiveDeck(Object.keys(decks)[0]);
        showToast('已刪除該日期單字庫', 'info');
      });

      // JSON Export & Import
      document.getElementById('exportJsonBtn').addEventListener('click', () => {
        const blob = new Blob([JSON.stringify(decks, null, 2)], { type: 'application/json' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = `daily-vocab-studio-${TODAY_STR}.json`;
        a.click();
        URL.revokeObjectURL(url);
        showToast('已匯出 JSON 單字庫備份', 'success');
      });

      document.getElementById('importJsonInput').addEventListener('change', (e) => {
        const file = e.target.files?.[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (ev) => {
          try {
            const imported = JSON.parse(ev.target.result);
            if (imported && typeof imported === 'object') {
              Object.keys(imported).forEach(k => {
                imported[k].words = (imported[k].words || []).map(normalizeWordEntry);
              });
              decks = { ...decks, ...imported };
              const firstImportedId = Object.keys(imported)[0] || activeDeckId;
              switchActiveDeck(firstImportedId);
              showToast('成功匯入 JSON 單字庫備份！', 'success');
            }
          } catch (err) {
            showToast('JSON 檔案格式錯誤，無法匯入', 'error');
          }
        };
        reader.readAsText(file);
        e.target.value = '';
      });

      // View 1: Raw word input & Gemini AI Generator
      const rawWordsInput = document.getElementById('rawWordsInput');
      rawWordsInput.addEventListener('input', () => {
        const list = parseInputWordList(rawWordsInput.value);
        document.getElementById('wordCountBadge').textContent = `${list.length} 個單字`;
      });

      document.getElementById('insertDemoWordsBtn').addEventListener('click', () => {
        rawWordsInput.value = 'collaborate, innovative, obsolete, tangible, versatile, ambiguous, catalyst, discrepancy, feasible, hierarchy, momentum, threshold';
        rawWordsInput.dispatchEvent(new Event('input'));
      });

      document.getElementById('generateAiBtn').addEventListener('click', async () => {
        const errBanner = document.getElementById('aiErrorBanner');
        const progBanner = document.getElementById('aiProgressBanner');
        const btn = document.getElementById('generateAiBtn');

        errBanner.classList.add('hidden');
        const wordsList = parseInputWordList(rawWordsInput.value);

        if (wordsList.length < 3) {
          errBanner.textContent = '請至少輸入 3 個英文單字（建議每日 10–20 個單字）。';
          errBanner.classList.remove('hidden');
          return;
        }
        if (wordsList.length > 25) {
          errBanner.textContent = '單次最多支援 25 個單字，請分批輸入。';
          errBanner.classList.remove('hidden');
          return;
        }

        const dateVal = document.getElementById('deckDateInput').value || TODAY_STR;
        const topicVal = document.getElementById('deckTopicInput').value.trim();

        btn.disabled = true;
        progBanner.classList.remove('hidden');
        document.getElementById('aiProgressText').textContent = `AI 正在為 ${wordsList.length} 個單字生成解釋、2–3 組常用片語與例句...`;

        try {
          const generatedWords = await fetchWordDetailsFromGemini(wordsList);
          const newId = `deck_${dateVal}_${Date.now()}`;
          decks[newId] = {
            id: newId,
            date: dateVal,
            topic: topicVal || `每日精選 (${generatedWords.length}詞)`,
            words: generatedWords
          };
          saveAppState();

          rawWordsInput.value = '';
          rawWordsInput.dispatchEvent(new Event('input'));
          document.getElementById('deckTopicInput').value = '';

          switchActiveDeck(newId);
          switchViewTab('flashcards');
          showToast(`成功生成 ${generatedWords.length} 個單字學習卡與填字遊戲！`, 'success');
        } catch (err) {
          errBanner.textContent = `AI 生成失敗（${err.message}），請稍後再試。`;
          errBanner.classList.remove('hidden');
        } finally {
          btn.disabled = false;
          progBanner.classList.add('hidden');
        }
      });

      // Manual Word Add/Edit Modal
      document.getElementById('addManualWordBtn').addEventListener('click', () => openWordModal(-1));
      document.getElementById('closeModalBtn').addEventListener('click', closeWordModal);
      document.getElementById('cancelModalBtn').addEventListener('click', closeWordModal);

      document.getElementById('wordModalForm').addEventListener('submit', (e) => {
        e.preventDefault();
        const idx = parseInt(document.getElementById('modalWordIndex').value, 10);
        const wordText = document.getElementById('modalWordInput').value.trim().toLowerCase().replace(/[^a-z-]/g, '');
        const zhMeaning = document.getElementById('modalMeaningZhInput').value.trim();

        const phrases = [];
        for (let i = 1; i <= 3; i++) {
          const en = document.getElementById(`modalPhraseEn${i}`).value.trim();
          const zh = document.getElementById(`modalPhraseZh${i}`).value.trim();
          if (en) phrases.push({ en, zh });
        }

        if (!wordText || !zhMeaning || phrases.length === 0) return;

        const entry = {
          word: wordText,
          pos: document.getElementById('modalPosInput').value.trim() || 'word',
          ipa: document.getElementById('modalIpaInput').value.trim(),
          zhMeaning,
          phrases,
          example: {
            en: document.getElementById('modalExampleEnInput').value.trim(),
            zh: document.getElementById('modalExampleZhInput').value.trim()
          },
          mastered: false
        };

        const deck = getActiveDeck();
        if (idx >= 0 && deck.words[idx]) {
          entry.mastered = deck.words[idx].mastered;
          deck.words[idx] = entry;
          showToast(`已更新單字「${wordText}」`, 'success');
        } else {
          deck.words.push(entry);
          showToast(`已新增單字「${wordText}」`, 'success');
        }

        saveAppState();
        closeWordModal();
        renderGlobalDeckSelector();
        recomputeFilteredFlashcards();
        renderFlashcardsView();
        renderDeckEditorList();
        initCrosswordPuzzle();
      });

      // View 2: 3D Flashcards Interactions
      document.getElementById('flashcard3D').addEventListener('click', () => {
        fcFlipped = !fcFlipped;
        renderFlashcardsView();
      });

      document.getElementById('btnSpeakUS').addEventListener('click', (e) => {
        e.stopPropagation();
        const w = getActiveDeck().words[fcFilteredIndices[fcIndex]];
        if (w) speakText(w.word, 'en-US');
      });

      document.getElementById('btnSpeakUK').addEventListener('click', (e) => {
        e.stopPropagation();
        const w = getActiveDeck().words[fcFilteredIndices[fcIndex]];
        if (w) speakText(w.word, 'en-GB');
      });

      document.getElementById('btnSpeakExample').addEventListener('click', (e) => {
        e.stopPropagation();
        const w = getActiveDeck().words[fcFilteredIndices[fcIndex]];
        if (w?.example?.en) speakText(w.example.en, 'en-US');
      });

      document.getElementById('prevCardBtn').addEventListener('click', () => {
        if (fcFilteredIndices.length === 0) return;
        fcIndex = (fcIndex - 1 + fcFilteredIndices.length) % fcFilteredIndices.length;
        fcFlipped = false;
        renderFlashcardsView();
      });

      document.getElementById('nextCardBtn').addEventListener('click', () => {
        if (fcFilteredIndices.length === 0) return;
        fcIndex = (fcIndex + 1) % fcFilteredIndices.length;
        fcFlipped = false;
        renderFlashcardsView();
      });

      document.getElementById('btnMarkReview').addEventListener('click', () => updateFlashcardMastery(false));
      document.getElementById('btnMarkMastered').addEventListener('click', () => updateFlashcardMastery(true));

      document.getElementById('filterAllBtn').addEventListener('click', () => {
        fcFilterMode = 'all';
        fcIndex = 0;
        fcFlipped = false;
        recomputeFilteredFlashcards();
        renderFlashcardsView();
      });

      document.getElementById('filterReviewBtn').addEventListener('click', () => {
        fcFilterMode = 'review';
        fcIndex = 0;
        fcFlipped = false;
        recomputeFilteredFlashcards();
        renderFlashcardsView();
      });

      document.getElementById('resetFilterAllBtn').addEventListener('click', () => {
        fcFilterMode = 'all';
        fcIndex = 0;
        fcFlipped = false;
        recomputeFilteredFlashcards();
        renderFlashcardsView();
      });

      document.getElementById('shuffleDeckBtn').addEventListener('click', () => {
        for (let i = fcFilteredIndices.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [fcFilteredIndices[i], fcFilteredIndices[j]] = [fcFilteredIndices[j], fcFilteredIndices[i]];
        }
        fcIndex = 0;
        fcFlipped = false;
        renderFlashcardsView();
        showToast('已隨機洗牌學習卡順序', 'info');
      });

      document.getElementById('editCurrentFlashcardBtn').addEventListener('click', () => {
        if (fcFilteredIndices.length === 0) return;
        openWordModal(fcFilteredIndices[fcIndex]);
      });

      // Keyboard Shortcuts for Flashcards (Spacebar to flip, Left/Right arrows to navigate)
      document.addEventListener('keydown', (e) => {
        const isFlashcardsVisible = !document.getElementById('view-flashcards').classList.contains('hidden');
        const isModalVisible = !document.getElementById('wordModal').classList.contains('hidden');
        if (!isFlashcardsVisible || isModalVisible) return;
        if (['INPUT', 'TEXTAREA', 'SELECT'].includes(document.activeElement.tagName)) return;

        if (e.code === 'Space') {
          e.preventDefault();
          fcFlipped = !fcFlipped;
          renderFlashcardsView();
        } else if (e.key === 'ArrowRight') {
          e.preventDefault();
          document.getElementById('nextCardBtn').click();
        } else if (e.key === 'ArrowLeft') {
          e.preventDefault();
          document.getElementById('prevCardBtn').click();
        }
      });

      // View 3: Crossword Puzzle Controls
      document.getElementById('cwCheckBtn').addEventListener('click', () => {
        let wrongCount = 0;
        let filledCount = 0;
        for (let r = 0; r < cwState.rows; r++) {
          for (let c = 0; c < cwState.cols; c++) {
            const cell = cwState.grid[r][c];
            if (cell && cell.userLetter) {
              filledCount++;
              if (cell.userLetter === cell.letter) {
                cell.status = 'correct';
              } else {
                cell.status = 'wrong';
                wrongCount++;
              }
            }
          }
        }
        renderCrosswordView();
        if (filledCount === 0) showToast('請先輸入字母再檢查答案', 'warning');
        else if (wrongCount > 0) showToast(`發現 ${wrongCount} 個錯誤字母（已標記為紅色）`, 'error');
        else showToast('目前填入的字母全部正確！', 'success');
      });

      document.getElementById('cwSingleHintBtn').addEventListener('click', () => {
        if (!cwState.selectedCell) {
          showToast('請先點選一個填字格', 'warning');
          return;
        }
        const { r, c } = cwState.selectedCell;
        const cell = cwState.grid[r]?.[c];
        if (!cell) return;
        cell.userLetter = cell.letter;
        cell.status = 'correct';
        advanceCrosswordCursor(r, c, 1);
        checkCrosswordVictory();
        renderCrosswordView();
        if (cwState.selectedCell) focusCrosswordCell(cwState.selectedCell.r, cwState.selectedCell.c);
      });

      document.getElementById('cwRevealAllBtn').addEventListener('click', () => {
        for (let r = 0; r < cwState.rows; r++) {
          for (let c = 0; c < cwState.cols; c++) {
            const cell = cwState.grid[r][c];
            if (cell) {
              cell.userLetter = cell.letter;
              cell.status = 'correct';
            }
          }
        }
        cwState.completed = true;
        renderCrosswordView();
        showToast('已顯示全部解答', 'info');
      });

      document.getElementById('cwRegenerateBtn').addEventListener('click', () => {
        initCrosswordPuzzle();
        showToast('已重新生成填字遊戲版面', 'info');
      });

      document.getElementById('cwReplayBtn').addEventListener('click', () => initCrosswordPuzzle());

      document.getElementById('cwSwitchDirBtn').addEventListener('click', () => {
        if (!cwState.selectedCell) return;
        const { r, c } = cwState.selectedCell;
        const cell = cwState.grid[r]?.[c];
        if (cell && cell.acrossWordId && cell.downWordId) {
          cwState.direction = cwState.direction === 'across' ? 'down' : 'across';
          renderCrosswordView();
          focusCrosswordCell(r, c);
        } else {
          showToast('此格子僅有單一方向單字', 'info');
        }
      });
    });
  </script>
</body>
</html>
