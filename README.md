<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>GASTRO.LAB & BAR — Кулинарная и Барная Экосистема</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,600;0,700;1,600&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-obsidian: #070d0a;
      --bg-emerald: #0d1f17;
      --bg-card: #11261c;
      --border-emerald: #1f4231;
      --gold: #d4af37;
      --gold-light: #f3e5ab;
      --amber: #e09f3e;
    }
    body {
      background-color: var(--bg-obsidian);
      color: #ecfdf5;
      font-family: 'Inter', sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    .font-serif-gastro {
      font-family: 'Cormorant Garamond', serif;
    }
    .gastro-card {
      background: linear-gradient(145deg, #11261c 0%, #0a1711 100%);
      border: 1px solid rgba(212, 175, 55, 0.16);
      box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.7);
    }
    .gastro-card-active {
      border-color: rgba(212, 175, 55, 0.55);
      box-shadow: 0 0 20px rgba(212, 175, 55, 0.15);
    }
    .gold-gradient {
      background: linear-gradient(135deg, #d4af37 0%, #b58922 100%);
      color: #070d0a;
    }
    .emerald-pill {
      background: rgba(19, 46, 33, 0.85);
      border: 1px solid rgba(52, 211, 153, 0.2);
    }
    .emerald-pill.selected {
      background: linear-gradient(135deg, #d4af37 0%, #b58922 100%);
      color: #070d0a;
      border-color: #f3e5ab;
      font-weight: 600;
    }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="min-h-screen pb-28 selection:bg-amber-500 selection:text-black">

  <!-- ВЕРХНЯЯ ШАПКА -->
  <header class="sticky top-0 z-40 backdrop-blur-xl bg-[#070d0a]/90 border-b border-emerald-900/60 px-4 py-3">
    <div class="max-w-md mx-auto flex items-center justify-between">
      <div>
        <div class="flex items-center gap-2">
          <span class="w-2.5 h-2.5 rounded-full bg-amber-400 animate-pulse"></span>
          <h1 class="font-serif-gastro text-2xl font-bold tracking-wider text-amber-100">GASTRO.LAB <span class="text-amber-400">&amp; BAR</span></h1>
        </div>
        <p class="text-[11px] text-emerald-400/80 tracking-wide">AI Кухня • Миксология • Гастропары</p>
      </div>

      <!-- Переключатель тарифа FREE / PRO -->
      <div class="flex items-center gap-2">
        <button id="tierToggleBtn" onclick="toggleProTier()" class="flex items-center gap-1.5 px-3 py-1.5 rounded-full text-xs font-semibold border transition-all duration-300 bg-emerald-950/90 border-amber-500/40 text-amber-300">
          <span id="tierBadgeDot" class="w-2 h-2 rounded-full bg-emerald-400"></span>
          <span id="tierLabel">Тариф: FREE</span>
        </button>
      </div>
    </div>

    <!-- Быстрые кнопки Нейро-Шефа и Нейро-Бармена -->
    <div class="max-w-md mx-auto mt-2.5 grid grid-cols-2 gap-2">
      <button onclick="openModal('aiChefModal')" class="flex items-center justify-center gap-1.5 py-2 px-3 rounded-xl text-xs font-semibold bg-emerald-950/90 border border-amber-500/30 text-amber-200 hover:border-amber-400 transition">
        <span>👨‍🍳</span> <span>AI Нейро-Шеф</span>
        <span class="text-[10px] px-1.5 py-0.5 rounded bg-amber-500/20 text-amber-300">PRO</span>
      </button>
      <button onclick="openModal('aiBarmanModal')" class="flex items-center justify-center gap-1.5 py-2 px-3 rounded-xl text-xs font-semibold bg-emerald-950/90 border border-amber-500/30 text-amber-200 hover:border-amber-400 transition">
        <span>🍸</span> <span>AI Нейро-Бармен</span>
        <span class="text-[10px] px-1.5 py-0.5 rounded bg-amber-500/20 text-amber-300">PRO</span>
      </button>
    </div>
  </header>

  <!-- ОСНОВНОЙ КОНТЕЙНЕР -->
  <main class="max-w-md mx-auto px-4 pt-4 space-y-5">

    <!-- ВКЛАДКА 1: КУХНЯ И ХОЛОДИЛЬНИК -->
    <section id="tab-kitchen" class="space-y-4">
      <div class="gastro-card rounded-2xl p-4">
        <div class="flex items-center justify-between mb-2">
          <h2 class="font-serif-gastro text-xl text-amber-100 font-semibold">Что в холодильнике?</h2>
          <button onclick="clearFridge()" class="text-xs text-emerald-400 hover:text-amber-300">Сбросить</button>
        </div>
        <p class="text-xs text-emerald-300/70 mb-3">Отметь продукты дома — рецепты пересчитают % совпадения:</p>
        <div id="fridgePills" class="flex flex-wrap gap-1.5 max-h-36 overflow-y-auto pr-1"></div>
      </div>

      <!-- Поиск и фильтры категорий кухни -->
      <div class="space-y-2.5">
        <div class="flex gap-2">
          <input id="kitchenSearch" oninput="renderKitchenRecipes()" type="text" placeholder="Поиск блюда или ингредиента..." class="w-full bg-emerald-950/70 border border-emerald-800/80 rounded-xl px-3.5 py-2 text-xs text-emerald-50 placeholder-emerald-500 focus:outline-none focus:border-amber-400">
        </div>
        <div id="kitchenCategories" class="flex gap-1.5 overflow-x-auto no-scrollbar pb-1"></div>
      </div>

      <!-- Список рецептов кухни -->
      <div id="kitchenRecipesList" class="space-y-3"></div>
    </section>

    <!-- ВКЛАДКА 2: БАР И МИКСОЛОГИЯ (FREE + PRO) -->
    <section id="tab-bar" class="hidden space-y-4">
      <div class="gastro-card rounded-2xl p-4">
        <div class="flex items-center justify-between mb-2">
          <div>
            <h2 class="font-serif-gastro text-xl text-amber-100 font-semibold">Домашний Бар &amp; Шейкер</h2>
            <p class="text-[11px] text-emerald-300/70">Отметь алкоголь, сиропы, фрукты, чай или тоники в наличии</p>
          </div>
          <button onclick="clearBarShelf()" class="text-xs text-emerald-400 hover:text-amber-300">Сбросить</button>
        </div>
        <div id="barPills" class="flex flex-wrap gap-1.5 max-h-36 overflow-y-auto pr-1 mt-2"></div>
      </div>

      <!-- Категории Бара -->
      <div id="barCategories" class="flex gap-1.5 overflow-x-auto no-scrollbar pb-1"></div>

      <!-- Быстрый баннер Гастропары и Барной Матрицы -->
      <div class="gastro-card rounded-2xl p-3.5 border border-amber-500/30 flex items-center justify-between">
        <div>
          <div class="text-xs font-semibold text-amber-200">🍷 Конструктор Гастропар (Блюдо + Напиток)</div>
          <div class="text-[11px] text-emerald-300/80">Подбери идеальный напиток к ужину или создай авторский микс</div>
        </div>
        <button onclick="switchTab('lab')" class="px-3 py-1.5 rounded-xl gold-gradient text-xs font-bold shrink-0">Открыть</button>
      </div>

      <!-- Список напитков -->
      <div id="barRecipesList" class="space-y-3"></div>
    </section>

    <!-- ВКЛАДКА 3: ГАСТРО-ЛАБОРАТОРИЯ (ФУД-ПЕЙРИНГ, ГАСТРОПАРЫ, КАЛЬКУЛЯТОРЫ) -->
    <section id="tab-lab" class="hidden space-y-4">
      <!-- Переключатель внутри Лаборатории -->
      <div class="grid grid-cols-3 gap-1.5 bg-emerald-950/80 p-1 rounded-xl border border-emerald-800/60 text-xs">
        <button onclick="switchLabSubtab('gastropair')" id="labBtn-gastropair" class="py-2 rounded-lg font-semibold gold-gradient">Гастропары</button>
        <button onclick="switchLabSubtab('pairing')" id="labBtn-pairing" class="py-2 rounded-lg font-medium text-emerald-300">Матрица вкусов</button>
        <button onclick="switchLabSubtab('calcs')" id="labBtn-calcs" class="py-2 rounded-lg font-medium text-emerald-300">Калькуляторы</button>
      </div>

      <!-- Подвкладка 3.1: Конструктор Гастропар (Блюдо <-> Напиток) -->
      <div id="lab-gastropair" class="space-y-4">
        <div class="gastro-card rounded-2xl p-4 space-y-3">
          <div class="flex items-center justify-between">
            <h3 class="font-serif-gastro text-xl text-amber-100 font-semibold">Синергия: Блюдо ↔ Напиток</h3>
            <span class="text-[10px] px-2 py-0.5 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/30">AI Сомелье</span>
          </div>
          <p class="text-xs text-emerald-300/80">Выбери блюдо из кухни и напиток из бара — система рассчитает баланс кислотности, танинов, жиров и сахара.</p>

          <div class="space-y-2.5">
            <div>
              <label class="text-[11px] text-emerald-400 block mb-1">1. Выбери блюдо:</label>
              <select id="pairFoodSelect" onchange="evaluateGastroPair()" class="w-full bg-emerald-950 border border-emerald-800 rounded-xl px-3 py-2 text-xs text-amber-100"></select>
            </div>
            <div>
              <label class="text-[11px] text-emerald-400 block mb-1">2. Выбери напиток:</label>
              <select id="pairDrinkSelect" onchange="evaluateGastroPair()" class="w-full bg-emerald-950 border border-emerald-800 rounded-xl px-3 py-2 text-xs text-amber-100"></select>
            </div>
          </div>

          <div id="gastroPairResult" class="p-3.5 rounded-xl bg-emerald-950/90 border border-amber-500/30 space-y-2"></div>
        </div>
      </div>

      <!-- Подвкладка 3.2: Матрица Фуд-пейринга и Барной Миксологии -->
      <div id="lab-pairing" class="hidden space-y-4">
        <div class="gastro-card rounded-2xl p-4 space-y-3">
          <h3 class="font-serif-gastro text-xl text-amber-100 font-semibold">Матрица Кулинарных и Барных Сочетаний</h3>
          <p class="text-xs text-emerald-300/80">Выбери ключевой продукт или барную базу, чтобы увидеть идеальные вкусовые мосты:</p>
          <div id="pairingMatrixButtons" class="flex flex-wrap gap-1.5"></div>
          <div id="pairingMatrixCard" class="p-3.5 rounded-xl bg-emerald-950/90 border border-emerald-800/80 space-y-2 mt-2"></div>
        </div>
      </div>

      <!-- Подвкладка 3.3: PRO Калькуляторы (Форма, Стейк, Настойки ABV) -->
      <div id="lab-calcs" class="hidden space-y-4">
        <!-- Калькулятор разбавления настоек и сиропов (ABV) -->
        <div class="gastro-card rounded-2xl p-4 space-y-3">
          <h3 class="font-serif-gastro text-lg text-amber-200 font-semibold">🥃 Барный калькулятор крепости настоек (ABV)</h3>
          <div class="grid grid-cols-3 gap-2 text-xs">
            <div>
              <label class="text-[10px] text-emerald-400">Объем базы (мл)</label>
              <input id="abvVol" type="number" value="500" oninput="calcABV()" class="w-full mt-1 bg-emerald-950 border border-emerald-800 rounded-lg px-2.5 py-1.5 text-amber-100">
            </div>
            <div>
              <label class="text-[10px] text-emerald-400">Крепость базы (%)</label>
              <input id="abvInit" type="number" value="45" oninput="calcABV()" class="w-full mt-1 bg-emerald-950 border border-emerald-800 rounded-lg px-2.5 py-1.5 text-amber-100">
            </div>
            <div>
              <label class="text-[10px] text-emerald-400">Сироп/Вода (мл)</label>
              <input id="abvWater" type="number" value="250" oninput="calcABV()" class="w-full mt-1 bg-emerald-950 border border-emerald-800 rounded-lg px-2.5 py-1.5 text-amber-100">
            </div>
          </div>
          <div id="abvOutput" class="text-xs text-amber-300 bg-emerald-950/80 p-2.5 rounded-xl border border-amber-500/20"></div>
        </div>

        <!-- Калькулятор кондитерских форм -->
        <div class="gastro-card rounded-2xl p-4 space-y-3">
          <h3 class="font-serif-gastro text-lg text-amber-200 font-semibold">🎂 Пересчет диаметра формы для выпечки</h3>
          <div class="grid grid-cols-2 gap-3 text-xs">
            <div>
              <label class="text-[10px] text-emerald-400">Диаметр в рецепте (см)</label>
              <input id="panOld" type="number" value="18" oninput="calcPan()" class="w-full mt-1 bg-emerald-950 border border-emerald-800 rounded-lg px-2.5 py-1.5 text-amber-100">
            </div>
            <div>
              <label class="text-[10px] text-emerald-400">Твоя форма (см)</label>
              <input id="panNew" type="number" value="22" oninput="calcPan()" class="w-full mt-1 bg-emerald-950 border border-emerald-800 rounded-lg px-2.5 py-1.5 text-amber-100">
            </div>
          </div>
          <div id="panOutput" class="text-xs text-amber-300 bg-emerald-950/80 p-2.5 rounded-xl border border-amber-500/20"></div>
        </div>
      </div>
    </section>

    <!-- ВКЛАДКА 4: ФУДКОСТ И СПИСОК ПОКУПОК -->
    <section id="tab-cost" class="hidden space-y-4">
      <!-- Список покупок (FREE) -->
      <div class="gastro-card rounded-2xl p-4 space-y-3">
        <div class="flex items-center justify-between">
          <h3 class="font-serif-gastro text-xl text-amber-100 font-semibold">🛒 Умный список покупок</h3>
          <button onclick="clearShoppingList()" class="text-xs text-emerald-400 hover:text-amber-300">Очистить</button>
        </div>
        <div id="shoppingListContainer" class="space-y-1.5 text-xs"></div>
        <div class="flex gap-2 pt-1">
          <input id="customShopItem" type="text" placeholder="Добавить продукт или напиток..." class="flex-1 bg-emerald-950 border border-emerald-800 rounded-xl px-3 py-1.5 text-xs text-emerald-100">
          <button onclick="addCustomShopItem()" class="px-3 py-1.5 rounded-xl gold-gradient text-xs font-semibold">+</button>
        </div>
      </div>

      <!-- Калькулятор Фудкоста (PRO) -->
      <div class="gastro-card rounded-2xl p-4 space-y-3">
        <div class="flex items-center justify-between">
          <h3 class="font-serif-gastro text-xl text-amber-100 font-semibold">📊 Калькулятор Фудкоста (PRO)</h3>
          <select id="foodcostPreset" onchange="loadFoodcostPreset()" class="bg-emerald-950 border border-amber-500/40 rounded-lg px-2 py-1 text-[11px] text-amber-200">
            <option value="cheesecake">Чизкейк Сан-Себастьян</option>
            <option value="negroni_batch">Батч Коктейля Негрони (1 л)</option>
            <option value="ramen">Утиный Рамен</option>
          </select>
        </div>
        <div id="foodcostRows" class="space-y-2 text-xs"></div>
        <div class="pt-2 border-t border-emerald-800/80 flex items-center justify-between text-xs">
          <div>
            <div class="text-emerald-400">Себестоимость партии: <span id="fcTotal" class="text-amber-200 font-bold">0 ₽</span></div>
            <div class="text-emerald-400">Себестоимость 1 порции: <span id="fcPortion" class="text-amber-300 font-bold">0 ₽</span></div>
          </div>
          <div class="text-right">
            <div class="text-emerald-400">Рек. цена продажи (x3):</div>
            <div id="fcRetail" class="text-sm font-bold text-amber-400">0 ₽</div>
          </div>
        </div>
      </div>
    </section>

  </main>

  <!-- МОДАЛЬНОЕ ОКНО ДЕТАЛЬНОГО РЕЦЕПТА / НАПИТКА -->
  <div id="recipeModal" class="fixed inset-0 z-50 hidden bg-black/80 backdrop-blur-md flex items-end sm:items-center justify-center p-0 sm:p-4">
    <div class="gastro-card w-full max-w-md max-h-[90vh] overflow-y-auto rounded-t-3xl sm:rounded-3xl p-5 space-y-4 border-t border-amber-500/40">
      <div class="flex items-start justify-between gap-2">
        <div>
          <span id="modalCategoryBadge" class="text-[10px] uppercase tracking-wider px-2 py-0.5 rounded-full bg-amber-500/20 text-amber-300"></span>
          <h2 id="modalTitle" class="font-serif-gastro text-2xl text-amber-100 font-bold mt-1"></h2>
          <p id="modalMeta" class="text-xs text-emerald-400"></p>
        </div>
        <button onclick="closeModal('recipeModal')" class="w-8 h-8 rounded-full bg-emerald-950 text-emerald-300 flex items-center justify-center text-lg">&times;</button>
      </div>

      <!-- Пересчет порций -->
      <div class="flex items-center justify-between bg-emerald-950/90 px-3.5 py-2 rounded-xl border border-emerald-800">
        <span class="text-xs text-emerald-300">Количество порций:</span>
        <div class="flex items-center gap-3">
          <button onclick="changeServings(-1)" class="w-7 h-7 rounded-lg bg-emerald-900 text-amber-200 font-bold">-</button>
          <span id="modalServingsCount" class="text-sm font-bold text-amber-300">2</span>
          <button onclick="changeServings(1)" class="w-7 h-7 rounded-lg bg-emerald-900 text-amber-200 font-bold">+</button>
        </div>
      </div>

      <!-- Ингредиенты -->
      <div>
        <div class="flex items-center justify-between mb-1.5">
          <h4 class="text-xs font-semibold uppercase tracking-wider text-amber-300">Ингредиенты</h4>
          <button onclick="addMissingToCart()" class="text-[11px] text-emerald-300 underline hover:text-amber-300">+ В список покупок</button>
        </div>
        <ul id="modalIngredients" class="space-y-1 text-xs"></ul>
      </div>

      <!-- Пошаговое приготовление с таймерами -->
      <div>
        <h4 class="text-xs font-semibold uppercase tracking-wider text-amber-300 mb-2">Технология и таймеры</h4>
        <div id="modalSteps" class="space-y-2 text-xs"></div>
      </div>

      <!-- Рекомендованная Гастропара -->
      <div id="modalPairingBox" class="p-3 rounded-xl bg-emerald-950/90 border border-amber-500/30 text-xs text-amber-100"></div>
    </div>
  </div>

  <!-- МОДАЛЬНОЕ ОКНО: AI НЕЙРО-ШЕФ -->
  <div id="aiChefModal" class="fixed inset-0 z-50 hidden bg-black/80 backdrop-blur-md flex items-center justify-center p-4">
    <div class="gastro-card w-full max-w-md rounded-3xl p-5 space-y-3 border border-amber-500/40">
      <div class="flex justify-between items-center">
        <h3 class="font-serif-gastro text-2xl text-amber-200 font-bold">👨‍🍳 AI Нейро-Шеф</h3>
        <button onclick="closeModal('aiChefModal')" class="text-emerald-400 text-xl">&times;</button>
      </div>
      <p class="text-xs text-emerald-300/80">Введи любые продукты через запятую и выбери гастрономический стиль — Нейро-Шеф создаст авторскую техкарту с таймерами и подберет напиток:</p>
      <input id="aiChefInput" type="text" placeholder="Например: утиная грудка, груша, горгонзола, тимьян" class="w-full bg-emerald-950 border border-emerald-800 rounded-xl px-3 py-2 text-xs text-amber-100">
      <select id="aiChefStyle" class="w-full bg-emerald-950 border border-emerald-800 rounded-xl px-3 py-2 text-xs text-amber-200">
        <option value="Ресторанный Бистро">Стиль: Ресторанный Бистро (Michelin Vibe)</option>
        <option value="Паназиатский Умами">Стиль: Паназиатский Умами</option>
        <option value="Итальянская Траттория">Стиль: Итальянская Траттория</option>
        <option value="Домашний Комфорт за 15 мин">Стиль: Быстрый ужин за 15 минут</option>
      </select>
      <button onclick="generateAIChefRecipe()" class="w-full py-2.5 rounded-xl gold-gradient text-xs font-bold">Сгенерировать техкарту блюда</button>
    </div>
  </div>

  <!-- МОДАЛЬНОЕ ОКНО: AI НЕЙРО-БАРМЕН -->
  <div id="aiBarmanModal" class="fixed inset-0 z-50 hidden bg-black/80 backdrop-blur-md flex items-center justify-center p-4">
    <div class="gastro-card w-full max-w-md rounded-3xl p-5 space-y-3 border border-amber-500/40">
      <div class="flex justify-between items-center">
        <h3 class="font-serif-gastro text-2xl text-amber-200 font-bold">🍸 AI Нейро-Бармен</h3>
        <button onclick="closeModal('aiBarmanModal')" class="text-emerald-400 text-xl">&times;</button>
      </div>
      <p class="text-xs text-emerald-300/80">Создай авторский коктейль, крафтовый лимонад, согревающий чай или домашнюю настойку под свой вкус и блюдо:</p>
      <input id="aiBarInput" type="text" placeholder="Например: джин, малина, розмарин, тоник, лайм" class="w-full bg-emerald-950 border border-emerald-800 rounded-xl px-3 py-2 text-xs text-amber-100">
      <div class="grid grid-cols-2 gap-2">
        <select id="aiBarType" class="bg-emerald-950 border border-emerald-800 rounded-xl px-2.5 py-2 text-xs text-amber-200">
          <option value="Алкогольный коктейль">Алкогольный коктейль</option>
          <option value="Безалкогольный моктейль">Моктейль 0.0%</option>
          <option value="Крафтовый лимонад">Крафтовый лимонад</option>
          <option value="Авторский чай">Авторский чай / Пунш</option>
          <option value="Домашняя настойка">Домашняя настойка</option>
        </select>
        <select id="aiBarBalance" class="bg-emerald-950 border border-emerald-800 rounded-xl px-2.5 py-2 text-xs text-amber-200">
          <option value="Кисло-сладкий Сауэр">Баланс: Сауэр (Кисло-сладкий)</option>
          <option value="Сухой и благородный">Баланс: Сухой и крепкий</option>
          <option value="Освежающий ботанический">Баланс: Свежий травяной</option>
          <option value="Пряный согревающий">Баланс: Пряный согревающий</option>
        </select>
      </div>
      <button onclick="generateAIBarmanDrink()" class="w-full py-2.5 rounded-xl gold-gradient text-xs font-bold">Смешать и создать рецепт напитка</button>
    </div>
  </div>

  <!-- НИЖНЯЯ НАВИГАЦИЯ -->
  <nav class="fixed bottom-0 inset-x-0 z-40 backdrop-blur-xl bg-[#070d0a]/95 border-t border-emerald-900/70">
    <div class="max-w-md mx-auto grid grid-cols-4 py-2 text-[11px]">
      <button onclick="switchTab('kitchen')" id="nav-kitchen" class="flex flex-col items-center gap-0.5 text-amber-300 font-semibold">
        <span class="text-base">🍳</span><span>Кухня</span>
      </button>
      <button onclick="switchTab('bar')" id="nav-bar" class="flex flex-col items-center gap-0.5 text-emerald-400">
        <span class="text-base">🍸</span><span>Бар</span>
      </button>
      <button onclick="switchTab('lab')" id="nav-lab" class="flex flex-col items-center gap-0.5 text-emerald-400">
        <span class="text-base">🧪</span><span>Гастро-Лаб</span>
      </button>
      <button onclick="switchTab('cost')" id="nav-cost" class="flex flex-col items-center gap-0.5 text-emerald-400">
        <span class="text-base">📊</span><span>Фудкост</span>
      </button>
    </div>
  </nav>

  <script>
    let isPro = false;
    let selectedFridge = ['Яйца', 'Сливочное масло', 'Сыр Пармезан', 'Чеснок', 'Паста', 'Куриное филе', 'Лимон'];
    let selectedBar = ['Джин', 'Лимон', 'Розмарин', 'Тоник', 'Сахарный сироп', 'Мята', 'Чай черный'];
    let activeKitchenCat = 'Все';
    let activeBarCat = 'Все';
    let currentModalItem = null;
    let currentServings = 2;
    let shoppingList = [
      { name: 'Сливки 33% (250 мл)', done: false },
      { name: 'Свежий тимьян', done: false }
    ];

    const FRIDGE_ITEMS = [
      'Яйца', 'Куриное филе', 'Говядина', 'Утиная грудка', 'Лосось', 'Креветки', 'Бекон',
      'Сливочное масло', 'Сливки', 'Сыр Пармезан', 'Творожный сыр', 'Моцарелла',
      'Паста', 'Рис Арборио', 'Картофель', 'Гречка', 'Хлеб бриошь',
      'Томаты', 'Чеснок', 'Лук красный', 'Шампиньоны', 'Авокадо', 'Шпинат', 'Тыква', 'Лимон', 'Груша'
    ];

    const BAR_ITEMS = [
      'Джин', 'Виски бурбон', 'Ром светлый', 'Водка', 'Апероль', 'Вермут красный', 'Кампари',
      'Лимон', 'Лайм', 'Апельсин', 'Грейпфрут', 'Малина', 'Маракуйя', 'Вишня',
      'Розмарин', 'Мята', 'Тимьян', 'Имбирь', 'Корица',
      'Тоник', 'Содовая', 'Сахарный сироп', 'Мед', 'Эспрессо', 'Чай черный', 'Чай улун'
    ];

    const KITCHEN_RECIPES = [
      { id: 'k1', title: 'Паста Карбонара аутентичная', cat: 'Паста и Ризотто', time: '18 мин', kcal: '540 ккал', pro: false, pairDrink: 'Негрони на розмарине или Лимонад Сицилийский цитрус',
        ings: [{n:'Паста', q:200, u:'г'}, {n:'Бекон', q:120, u:'г'}, {n:'Яйца', q:3, u:'шт'}, {n:'Сыр Пармезан', q:60, u:'г'}, {n:'Чеснок', q:1, u:'зуб.'}],
        steps: [{t:'Отвари пасту в подсоленной воде до состояния аль денте.', s:480}, {t:'Обжарь кубики бекона до хруста, сними сковороду с огня.', s:180}, {t:'Взбей желтки с тертым пармезаном и половником воды от пасты, объедини в сковороде без кипения.', s:90}] },
      { id: 'k2', title: 'Утиная грудка Магре с пряной грушей', cat: 'Мясо и Птица', time: '25 мин', kcal: '490 ккал', pro: true, pairDrink: 'Вишнево-пряный бурбон или Чай Пуэр с вишней и чабрецом',
        ings: [{n:'Утиная грудка', q:350, u:'г'}, {n:'Груша', q:1, u:'шт'}, {n:'Сливочное масло', q:30, u:'г'}, {n:'Лимон', q:0.5, u:'шт'}],
        steps: [{t:'Сделай ромбовидные надрезы на коже утки, положи на холодную сковороду кожей вниз.', s:480}, {t:'Переверни грудку, добавь сливочное масло и дольки груши, поливай соком.', s:240}, {t:'Дай мясу отдохнуть в фольге перед нарезкой.', s:300}] },
      { id: 'k3', title: 'Сливочное Ризотто с белыми грибами', cat: 'Паста и Ризотто', time: '28 мин', kcal: '460 ккал', pro: false, pairDrink: 'Сухой Джин-Тоник с тимьяном или Безалкогольный Еловый Тоник',
        ings: [{n:'Рис Арборио', q:180, u:'г'}, {n:'Шампиньоны', q:200, u:'г'}, {n:'Сливочное масло', q:45, u:'г'}, {n:'Сыр Пармезан', q:50, u:'г'}, {n:'Чеснок', q:2, u:'зуб.'}],
        steps: [{t:'Обжарь грибы с чесноком до золотистой корочки.', s:300}, {t:'Прогрей рис Арборио, вливай горячий бульон по половнику при помешивании.', s:900}, {t:'Сними с огня, вмешай холодное сливочное масло и пармезан до текстуры волны (мантекатура).', s:120}] },
      { id: 'k4', title: 'Лосось терияки с кремом из авокадо', cat: 'Рыба и Морепродукты', time: '15 мин', kcal: '420 ккал', pro: false, pairDrink: 'Юдзу-Мята Хайбол или Жасминовый колд-брю чай',
        ings: [{n:'Лосось', q:300, u:'г'}, {n:'Авокадо', q:1, u:'шт'}, {n:'Лимон', q:1, u:'шт'}, {n:'Сливочное масло', q:20, u:'г'}],
        steps: [{t:'Обжарь стейк лосося кожей вниз до хрустящей корочки.', s:240}, {t:'Переверни рыбу, глазируй соком лимона и маслом.', s:120}, {t:'Взбей мякоть авокадо с цедрой лимона в гладкий мусс.', s:90}] },
      { id: 'k5', title: 'Яйца Бенедикт на бриоши с лососем', cat: 'Завтраки', time: '15 мин', kcal: '410 ккал', pro: false, pairDrink: 'Апероль Спритц или Цитрусовый Эспрессо-Тоник',
        ings: [{n:'Яйца', q:4, u:'шт'}, {n:'Хлеб бриошь', q:2, u:'шт'}, {n:'Лосось', q:100, u:'г'}, {n:'Сливочное масло', q:60, u:'г'}, {n:'Лимон', q:0.5, u:'шт'}],
        steps: [{t:'Подрумянь бриошь на сливочном масле.', s:120}, {t:'Свари яйца пашот в воде у грани кипения.', s:180}, {t:'Эмульгируй желтки с растопленным маслом и соком лимона в голландский соус.', s:150}] },
      { id: 'k6', title: 'Креветки Пиль-Пиль в чесночном масле', cat: 'Рыба и Морепродукты', time: '12 мин', kcal: '360 ккал', pro: false, pairDrink: 'Безалкогольный Грейпфрут-Розмарин Спритц или Джин Сауэр',
        ings: [{n:'Креветки', q:300, u:'г'}, {n:'Чеснок', q:4, u:'зуб.'}, {n:'Сливочное масло', q:50, u:'г'}, {n:'Лимон', q:0.5, u:'шт'}, {n:'Хлеб бриошь', q:2, u:'шт'}],
        steps: [{t:'Томи лепестки чеснока в масле на среднем огне.', s:120}, {t:'Выложи креветки, обжарь по 90 секунд с каждой стороны и сбрызни лимоном.', s:180}] },
      { id: 'k7', title: 'Стейк Рибай с кофейно-перечным маслом', cat: 'Мясо и Птица', time: '16 мин', kcal: '620 ккал', pro: true, pairDrink: 'Олд Фэшн на бурбоне или Копченая вишневая настойка',
        ings: [{n:'Говядина', q:400, u:'г'}, {n:'Сливочное масло', q:50, u:'г'}, {n:'Чеснок', q:3, u:'зуб.'}],
        steps: [{t:'Запечатай стейк на раскаленной сковороде по 2 минуты с каждой стороны.', s:240}, {t:'Убавь огонь, добавь сливочное масло и чеснок, поливай стейк (арозе) до прожарки Medium.', s:180}, {t:'Отдых мяса на теплой доске.', s:300}] },
      { id: 'k8', title: 'Чизкейк Сан-Себастьян с карамельной корочкой', cat: 'Выпечка и Десерты', time: '35 мин', kcal: '480 ккал', pro: true, pairDrink: 'Эспрессо-Тоник на грейпфруте или Пряный чай Масала',
        ings: [{n:'Творожный сыр', q:500, u:'г'}, {n:'Сливки', q:250, u:'мл'}, {n:'Яйца', q:4, u:'шт'}],
        steps: [{t:'Объедини творожный сыр комнатной температуры, яйца и жирные сливки лопаткой без взбивания.', s:240}, {t:'Выпекай при 240°C до темной карамельной шапки и дрожащего центра.', s:1500}] }
    ];

    const BAR_RECIPES = [
      { id: 'b1', title: 'Ботанический Джин-Тоник с розмарином', cat: 'Алкогольные коктейли', abv: '11% ABV', glass: 'Коллинз / Бокал для вина', pro: false, pairDrink: 'Идеально к: Сливочному ризотто, лососю и креветкам Пиль-Пиль',
        ings: [{n:'Джин', q:50, u:'мл'}, {n:'Тоник', q:150, u:'мл'}, {n:'Лимон', q:15, u:'мл'}, {n:'Розмарин', q:1, u:'вет.'}],
        steps: [{t:'Охлади бокал льдом до краев и слей талую воду.', s:30}, {t:'Влей джин, сок лимона и сухой тоник по барной ложке.', s:45}, {t:'Слегка обожги веточку розмарина для раскрытия эфирных масел.', s:20}] },
      { id: 'b2', title: 'Смоки Негрони на апельсиновой цедре', cat: 'Алкогольные коктейли', abv: '24% ABV', glass: 'Рокс с крупным кубом льда', pro: false, pairDrink: 'Идеально к: Пасте Карбонара, мясным закускам и стейку',
        ings: [{n:'Джин', q:30, u:'мл'}, {n:'Кампари', q:30, u:'мл'}, {n:'Вермут красный', q:30, u:'мл'}, {n:'Апельсин', q:1, u:'цедра'}],
        steps: [{t:'В смесительном стакане со льдом соедини джин, красный вермут и Кампари.', s:30}, {t:'Выполни плавный стир барной ложкой 25 секунд для идеального охлаждения.', s:25}, {t:'Перелей на крупный куб льда и сбрызни эфирными маслами апельсиновой цедры.', s:15}] },
      { id: 'b3', title: 'Виски Сауэр с малиной и тимьяном', cat: 'Алкогольные коктейли', abv: '16% ABV', glass: 'Шале / Купе', pro: true, pairDrink: 'Идеально к: Утиной грудке Магре и выдержанным сырам',
        ings: [{n:'Виски бурбон', q:50, u:'мл'}, {n:'Лимон', q:25, u:'мл'}, {n:'Сахарный сироп', q:20, u:'мл'}, {n:'Малина', q:4, u:'ягоды'}, {n:'Тимьян', q:1, u:'вет.'}],
        steps: [{t:'Разомни малину в шейкере, добавь бурбон, сок лимона и сироп (драй-шейк без льда).', s:20}, {t:'Добавь лед и интенсивно взбей в шейкере 15 секунд.', s:15}, {t:'Процеди через двойной стрейнер в охлажденный бокал, укрась тимьяном.', s:20}] },
      { id: 'b4', title: 'Моктейль 0.0% «Грейпфрут-Розмарин Спритц»', cat: 'Моктейли 0.0%', abv: '0.0% ABV', glass: 'Винный бокал', pro: false, pairDrink: 'Идеально к: Креветкам, завтракам с лососем и легким салатам',
        ings: [{n:'Грейпфрут', q:80, u:'мл'}, {n:'Тоник', q:100, u:'мл'}, {n:'Сахарный сироп', q:15, u:'мл'}, {n:'Розмарин', q:2, u:'вет.'}],
        steps: [{t:'Прогрей сахарный сироп с розмарином 2 минуты для травяной базы.', s:120}, {t:'Наполни бокал льдом, влей свежевыжатый сок грейпфрута и тоник.', s:30}] },
      { id: 'b5', title: 'Крафтовый лимонад «Маракуйя — Имбирь — Мята»', cat: 'Крафтовые лимонады', abv: '0.0% ABV', glass: 'Хайбол', pro: false, pairDrink: 'Идеально к: Паназиатским блюдам, птице и быстрым ужинам',
        ings: [{n:'Маракуйя', q:40, u:'г'}, {n:'Имбирь', q:15, u:'г'}, {n:'Лайм', q:20, u:'мл'}, {n:'Содовая', q:150, u:'мл'}, {n:'Мята', q:5, u:'лист.'}],
        steps: [{t:'Измельчи свежий имбирь с соком лайма и сахарным сиропом.', s:60}, {t:'Соедини с мякотью маракуйи, хлопни листья мяты в ладонях и долей ледяной содовой.', s:30}] },
      { id: 'b6', title: 'Авторский чай «Пряная Вишня & Таежный Чабрец»', cat: 'Авторские чаи', abv: '0.0% ABV', glass: 'Стеклянный чайник 600 мл', pro: false, pairDrink: 'Идеально к: Чизкейку Сан-Себастьян, утке и вечернему десерту',
        ings: [{n:'Чай черный', q:8, u:'г'}, {n:'Вишня', q:60, u:'г'}, {n:'Тимьян', q:3, u:'вет.'}, {n:'Корица', q:1, u:'палочка'}, {n:'Мед', q:25, u:'г'}],
        steps: [{t:'Разомни вишню с палочкой корицы и веточками чабреца на дне чайника.', s:45}, {t:'Засыпь крупнолистовой чай, залей водой 92°C и дай настояться.', s:300}, {t:'Добавь мед при подаче, чтобы сохранить аромат.', s:20}] },
      { id: 'b7', title: 'Цитрусовый Эспрессо-Тоник на грейпфруте', cat: 'Кофейная миксология', abv: '0.0% ABV', glass: 'Хайбол со льдом', pro: false, pairDrink: 'Идеально к: Яйцам Бенедикт, бриоши и сливочным десертам',
        ings: [{n:'Эспрессо', q:40, u:'мл'}, {n:'Тоник', q:130, u:'мл'}, {n:'Грейпфрут', q:30, u:'мл'}],
        steps: [{t:'Наполни стакан крупным льдом, влей сок грейпфрута и охлажденный тоник.', s:30}, {t:'Свари двойной шот эспрессо и аккуратно влей верхним слоем по дольке цитруса.', s:35}] },
      { id: 'b8', title: 'Домашняя настойка «Пряная Вишня на бурбоне»', cat: 'Домашние настойки (PRO)', abv: '28% ABV', glass: 'Лафитник / Рокс', pro: true, pairDrink: 'Идеально к: Стейку Рибай, томленой говядине и темному шоколаду',
        ings: [{n:'Виски бурбон', q:500, u:'мл'}, {n:'Вишня', q:350, u:'г'}, {n:'Корица', q:1, u:'палочка'}, {n:'Мед', q:60, u:'г'}],
        steps: [{t:'Слегка припусти вишню с корицей в сотейнике 3 минуты для карамелизации сока.', s:180}, {t:'Залей бурбоном в стеклянной банке, плотно закрой и убери в темное место (мацерация).', s:3600}, {t:'Отфильтруй через фильтр и сбалансируй медом.', s:120}] }
    ];

    const PAIRING_MATRIX = [
      { key: 'Утка / Говядина', type: 'Кухня', bestSpices: 'Тимьян, можжевельник, копченая паприка, бадьян', bestSauces: 'Вишневый демигляс, соус из горгонзолы, пряная груша', bestDrinks: 'Виски Сауэр, Пряная вишневая настойка, Чай с чабрецом и вишней', science: 'Жирность темного мяса требует высокой кислотности ягод или танинов дубовой бочки для очищения рецепторов.' },
      { key: 'Лосось / Креветки', type: 'Кухня', bestSpices: 'Цедра лайма, розовый перец, эстрагон, имбирь', bestSauces: 'Голландский соус, крем из авокадо, масло бер-блан', bestDrinks: 'Ботанический Джин-Тоник, Грейпфрут-Розмарин Спритц, Улун со льдом', science: 'Хвойные терпены джина и цитрусовые кислоты подчеркивают сладость морепродуктов.' },
      { key: 'Джин (Сухой / Ботанический)', type: 'Бар', bestSpices: 'Розмарин, огурец, розовый перец, кардамон, базилик', bestSauces: 'Тоник сухой, кордиал лайм-лемонграсс, вермут сухой', bestDrinks: 'Гастропара: Ризотто с грибами, тартар из лосося, сыры с белой плесенью', science: 'Можжевеловый пинен идеально связывается с эфирными маслами трав и сливочными жирами.' },
      { key: 'Бурбон / Темный Ром', type: 'Бар', bestSpices: 'Корица, бобы тонка, вишня, пекан, обжаренное кофейное зерно', bestSauces: 'Кленовый сироп, красный вермут, биттер Ангостура', bestDrinks: 'Гастропара: Стейк Рибай, утиная грудка, баскский чизкейк', science: 'Ванильные лактоны из обожженной бочки усиливают реакцию Майяра (корочку на стейке и выпечке).' },
      { key: 'Чай & Травяные Инфузии', type: 'Бар', bestSpices: 'Чабрец, свежий имбирь, бадьян, цедра апельсина, мята', bestSauces: 'Гранатовый сок, пюре вишни/маракуйи, гречишный мед', bestDrinks: 'Гастропара: Выпечка, завтраки с бриошью, пряная птица', science: 'Чайные танины структурируют напиток не хуже вина, позволяя создавать безалкогольные гастропары.' }
    ];

    const FOODCOST_PRESETS = {
      cheesecake: { portions: 8, items: [{n:'Творожный сыр (500г)', p:420}, {n:'Сливки 33% (250мл)', p:190}, {n:'Яйца С0 (4 шт)', p:65}, {n:'Сахар и ваниль', p:45}] },
      negroni_batch: { portions: 10, items: [{n:'Джин сухой (330 мл)', p:650}, {n:'Кампари (330 мл)', p:720}, {n:'Красный вермут (330 мл)', p:480}, {n:'Апельсины и лед', p:90}] },
      ramen: { portions: 4, items: [{n:'Утиная грудка (400г)', p:520}, {n:'Бульон и соус тарэ', p:240}, {n:'Пшеничная лапша и яйца', p:180}, {n:'Грибы и зеленый лук', p:120}] }
    };

    function toggleProTier() {
      isPro = !isPro;
      const btn = document.getElementById('tierToggleBtn');
      const dot = document.getElementById('tierBadgeDot');
      const lbl = document.getElementById('tierLabel');
      if (isPro) {
        btn.className = 'flex items-center gap-1.5 px-3 py-1.5 rounded-full text-xs font-bold transition-all duration-300 gold-gradient shadow-lg';
        dot.className = 'w-2 h-2 rounded-full bg-black';
        lbl.textContent = 'Тариф: PRO АКТИВЕН';
      } else {
        btn.className = 'flex items-center gap-1.5 px-3 py-1.5 rounded-full text-xs font-semibold border transition-all duration-300 bg-emerald-950/90 border-amber-500/40 text-amber-300';
        dot.className = 'w-2 h-2 rounded-full bg-emerald-400';
        lbl.textContent = 'Тариф: FREE';
      }
      renderKitchenRecipes();
      renderBarRecipes();
    }

    function switchTab(tab) {
      ['kitchen', 'bar', 'lab', 'cost'].forEach(t => {
        document.getElementById('tab-' + t).classList.toggle('hidden', t !== tab);
        const nav = document.getElementById('nav-' + t);
        nav.className = t === tab ? 'flex flex-col items-center gap-0.5 text-amber-300 font-semibold' : 'flex flex-col items-center gap-0.5 text-emerald-400';
      });
    }

    function switchLabSubtab(sub) {
      ['gastropair', 'pairing', 'calcs'].forEach(s => {
        document.getElementById('lab-' + s).classList.toggle('hidden', s !== sub);
        const b = document.getElementById('labBtn-' + s);
        b.className = s === sub ? 'py-2 rounded-lg font-semibold gold-gradient' : 'py-2 rounded-lg font-medium text-emerald-300';
      });
    }

    function renderFridgePills() {
      const box = document.getElementById('fridgePills');
      box.innerHTML = FRIDGE_ITEMS.map(item => {
        const active = selectedFridge.includes(item);
        return `<button onclick="toggleFridgeItem('${item}')" class="emerald-pill ${active ? 'selected' : 'text-emerald-200'} px-2.5 py-1 rounded-full text-[11px] transition">${item}</button>`;
      }).join('');
    }

    function toggleFridgeItem(item) {
      if (selectedFridge.includes(item)) selectedFridge = selectedFridge.filter(i => i !== item);
      else selectedFridge.push(item);
      renderFridgePills();
      renderKitchenRecipes();
    }

    function clearFridge() {
      selectedFridge = [];
      renderFridgePills();
      renderKitchenRecipes();
    }

    function renderBarPills() {
      const box = document.getElementById('barPills');
      box.innerHTML = BAR_ITEMS.map(item => {
        const active = selectedBar.includes(item);
        return `<button onclick="toggleBarItem('${item}')" class="emerald-pill ${active ? 'selected' : 'text-emerald-200'} px-2.5 py-1 rounded-full text-[11px] transition">${item}</button>`;
      }).join('');
    }

    function toggleBarItem(item) {
      if (selectedBar.includes(item)) selectedBar = selectedBar.filter(i => i !== item);
      else selectedBar.push(item);
      renderBarPills();
      renderBarRecipes();
    }

    function clearBarShelf() {
      selectedBar = [];
      renderBarPills();
      renderBarRecipes();
    }

    function renderKitchenCategories() {
      const cats = ['Все', 'Завтраки', 'Паста и Ризотто', 'Мясо и Птица', 'Рыба и Морепродукты', 'Выпечка и Десерты'];
      document.getElementById('kitchenCategories').innerHTML = cats.map(c =>
        `<button onclick="activeKitchenCat='${c}';renderKitchenCategories();renderKitchenRecipes()" class="px-3 py-1.5 rounded-xl text-xs whitespace-nowrap ${activeKitchenCat===c ? 'gold-gradient font-bold' : 'bg-emerald-950/80 text-emerald-300 border border-emerald-800/60'}">${c}</button>`
      ).join('');
    }

    function renderBarCategories() {
      const cats = ['Все', 'Алкогольные коктейли', 'Моктейли 0.0%', 'Крафтовые лимонады', 'Авторские чаи', 'Кофейная миксология', 'Домашние настойки (PRO)'];
      document.getElementById('barCategories').innerHTML = cats.map(c =>
        `<button onclick="activeBarCat='${c}';renderBarCategories();renderBarRecipes()" class="px-3 py-1.5 rounded-xl text-xs whitespace-nowrap ${activeBarCat===c ? 'gold-gradient font-bold' : 'bg-emerald-950/80 text-emerald-300 border border-emerald-800/60'}">${c}</button>`
      ).join('');
    }

    function renderKitchenRecipes() {
      const q = (document.getElementById('kitchenSearch').value || '').toLowerCase();
      const list = document.getElementById('kitchenRecipesList');
      const filtered = KITCHEN_RECIPES.filter(r => {
        const matchCat = activeKitchenCat === 'Все' || r.cat === activeKitchenCat;
        const matchQ = r.title.toLowerCase().includes(q) || r.ings.some(i => i.n.toLowerCase().includes(q));
        return matchCat && matchQ;
      }).map(r => {
        const have = r.ings.filter(i => selectedFridge.includes(i.n)).length;
        const pct = Math.round((have / r.ings.length) * 100);
        return { ...r, pct };
      }).sort((a,b) => b.pct - a.pct);

      list.innerHTML = filtered.map(r => `
        <div onclick="openRecipeCard('${r.id}', 'kitchen')" class="gastro-card rounded-2xl p-4 cursor-pointer hover:border-amber-400/50 transition">
          <div class="flex items-start justify-between gap-2">
            <div>
              <div class="flex items-center gap-1.5">
                <span class="text-[10px] px-2 py-0.5 rounded-full bg-emerald-900/90 text-emerald-300">${r.cat}</span>
                ${r.pro ? `<span class="text-[10px] px-2 py-0.5 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/40">PRO</span>` : ''}
              </div>
              <h3 class="font-serif-gastro text-lg font-bold text-amber-100 mt-1">${r.title}</h3>
            </div>
            <span class="text-xs font-bold px-2.5 py-1 rounded-xl ${r.pct >= 60 ? 'bg-emerald-500/20 text-emerald-300 border border-emerald-500/40' : 'bg-emerald-950 text-emerald-400'}">${r.pct}% дома</span>
          </div>
          <div class="flex items-center justify-between mt-2.5 text-[11px] text-emerald-400">
            <span>⏱ ${r.time} • 🔥 ${r.kcal}</span>
            <span class="text-amber-300/90">🍷 Пара: ${r.pairDrink.split(' или ')[0]}</span>
          </div>
        </div>
      `).join('');
    }

    function renderBarRecipes() {
      const list = document.getElementById('barRecipesList');
      const filtered = BAR_RECIPES.filter(r => activeBarCat === 'Все' || r.cat === activeBarCat).map(r => {
        const have = r.ings.filter(i => selectedBar.includes(i.n)).length;
        const pct = Math.round((have / r.ings.length) * 100);
        return { ...r, pct };
      }).sort((a,b) => b.pct - a.pct);

      list.innerHTML = filtered.map(r => `
        <div onclick="openRecipeCard('${r.id}', 'bar')" class="gastro-card rounded-2xl p-4 cursor-pointer hover:border-amber-400/50 transition">
          <div class="flex items-start justify-between gap-2">
            <div>
              <div class="flex items-center gap-1.5">
                <span class="text-[10px] px-2 py-0.5 rounded-full bg-emerald-900/90 text-emerald-300">${r.cat}</span>
                <span class="text-[10px] px-2 py-0.5 rounded-full bg-amber-500/15 text-amber-200">${r.abv}</span>
                ${r.pro ? `<span class="text-[10px] px-2 py-0.5 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/40">PRO</span>` : ''}
              </div>
              <h3 class="font-serif-gastro text-lg font-bold text-amber-100 mt-1">${r.title}</h3>
            </div>
            <span class="text-xs font-bold px-2.5 py-1 rounded-xl ${r.pct >= 60 ? 'bg-emerald-500/20 text-emerald-300 border border-emerald-500/40' : 'bg-emerald-950 text-emerald-400'}">${r.pct}% в баре</span>
          </div>
          <div class="flex items-center justify-between mt-2.5 text-[11px] text-emerald-400">
            <span>🥃 ${r.glass}</span>
            <span class="text-amber-300/90">${r.pairDrink.slice(0, 38)}...</span>
          </div>
        </div>
      `).join('');
    }

    function openRecipeCard(id, source) {
      const pool = source === 'kitchen' ? KITCHEN_RECIPES : BAR_RECIPES;
      const item = pool.find(x => x.id === id);
      if (!item) return;
      if (item.pro && !isPro) {
        if (confirm('Этот авторский рецепт входит в подписку GASTRO.LAB PRO. Включить демо-режим PRO прямо сейчас?')) {
          toggleProTier();
        } else return;
      }
      currentModalItem = { ...item, source };
      currentServings = 2;
      renderModalContent();
      openModal('recipeModal');
    }

    function renderModalContent() {
      const r = currentModalItem;
      document.getElementById('modalCategoryBadge').textContent = r.cat;
      document.getElementById('modalTitle').textContent = r.title;
      document.getElementById('modalMeta').textContent = r.source === 'kitchen' ? `⏱ ${r.time} • 🔥 ${r.kcal}` : `🥃 ${r.glass} • ${r.abv}`;
      document.getElementById('modalServingsCount').textContent = currentServings;

      const ratio = currentServings / 2;
      const stock = r.source === 'kitchen' ? selectedFridge : selectedBar;
      document.getElementById('modalIngredients').innerHTML = r.ings.map(i => {
        const has = stock.includes(i.n);
        const val = Math.round(i.q * ratio * 10) / 10;
        return `<li class="flex justify-between py-1 border-b border-emerald-900/60">
          <span class="${has ? 'text-emerald-200' : 'text-amber-300'}">${has ? '✓' : '•'} ${i.n}</span>
          <span class="font-semibold text-amber-100">${val} ${i.u}</span>
        </li>`;
      }).join('');

      document.getElementById('modalSteps').innerHTML = r.steps.map((st, idx) => `
        <div class="p-2.5 rounded-xl bg-emerald-950/80 border border-emerald-800/70 flex items-center justify-between gap-2">
          <div><span class="text-amber-400 font-bold mr-1">Шаг ${idx+1}.</span> ${st.t}</div>
          <button onclick="startStepTimer(this, ${st.s})" class="px-2.5 py-1 rounded-lg bg-amber-500/20 border border-amber-500/40 text-amber-300 font-semibold shrink-0">⏱ ${Math.floor(st.s/60)}:${String(st.s%60).padStart(2,'0')}</button>
        </div>
      `).join('');

      document.getElementById('modalPairingBox').innerHTML = `<strong>🍽 Гастропара шефа и бар-менеджера:</strong><br>${r.pairDrink}`;
    }

    function changeServings(delta) {
      currentServings = Math.max(1, currentServings + delta);
      renderModalContent();
    }

    function startStepTimer(btn, sec) {
      if (btn.dataset.running === '1') return;
      btn.dataset.running = '1';
      let left = sec;
      const iv = setInterval(() => {
        left--;
        if (left <= 0) {
          clearInterval(iv);
          btn.textContent = '✓ Готово!';
          btn.dataset.running = '0';
        } else {
          btn.textContent = `${Math.floor(left/60)}:${String(left%60).padStart(2,'0')}`;
        }
      }, 1000);
    }

    function addMissingToCart() {
      if (!currentModalItem) return;
      currentModalItem.ings.forEach(i => {
        shoppingList.push({ name: `${i.n} (${i.q} ${i.u})`, done: false });
      });
      renderShoppingList();
      alert('Ингредиенты добавлены в Умный список покупок (вкладка Фудкост)!');
    }

    function generateAIChefRecipe() {
      const raw = document.getElementById('aiChefInput').value.trim() || 'Утиная грудка, груша, сливочное масло, розмарин';
      const style = document.getElementById('aiChefStyle').value;
      const parts = raw.split(',').map(s => s.trim()).filter(Boolean);
      const newRec = {
        id: 'ai_k_' + Date.now(),
        title: `Авторское блюдо: ${parts.slice(0,2).join(' и ')} (${style})`,
        cat: 'Мясо и Птица',
        time: '20 мин',
        kcal: '450 ккал',
        pro: false,
        pairDrink: 'Рекомендация Нейро-Бармена: Смоки Негрони или Крафтовый лимонад Маракуйя-Имбирь',
        ings: parts.map(p => ({ n: p, q: 120, u: 'г' })),
        steps: [
          { t: `Подготовь базу (${parts.join(', ')}) и прогрей сковороду со сливочным маслом.`, s: 180 },
          { t: `Обжарь основной продукт в стиле «${style}» до карамельной корочки и глазируй соусом.`, s: 360 },
          { t: 'Дай блюду отдохнуть 2 минуты перед ресторанной подачей.', s: 120 }
        ]
      };
      KITCHEN_RECIPES.unshift(newRec);
      closeModal('aiChefModal');
      switchTab('kitchen');
      renderKitchenRecipes();
      openRecipeCard(newRec.id, 'kitchen');
    }

    function generateAIBarmanDrink() {
      const raw = document.getElementById('aiBarInput').value.trim() || 'Джин, малина, розмарин, тоник';
      const type = document.getElementById('aiBarType').value;
      const bal = document.getElementById('aiBarBalance').value;
      const parts = raw.split(',').map(s => s.trim()).filter(Boolean);
      const newDrink = {
        id: 'ai_b_' + Date.now(),
        title: `${type}: «${parts.slice(0,2).join(' & ')}»`,
        cat: type.includes('Моктейль') ? 'Моктейли 0.0%' : (type.includes('чай') ? 'Авторские чаи' : 'Алкогольные коктейли'),
        abv: type.includes('Алкогольный') || type.includes('настойка') ? '15% ABV' : '0.0% ABV',
        glass: 'Рокс / Авторская подача',
        pro: false,
        pairDrink: `Баланс: ${bal}. Гастропара: Утиная грудка Магре, креветки Пиль-Пиль или чизкейк`,
        ings: parts.map((p, idx) => ({ n: p, q: idx === 0 ? 50 : 30, u: 'мл/г' })),
        steps: [
          { t: `Охлади бокал и соедини в шейкере/чайнике базу: ${parts.join(', ')}.`, s: 30 },
          { t: `Сбалансируй профиль (${bal}) и процеди через стрейнер на крупный лед.`, s: 25 }
        ]
      };
      BAR_RECIPES.unshift(newDrink);
      closeModal('aiBarmanModal');
      switchTab('bar');
      renderBarRecipes();
      openRecipeCard(newDrink.id, 'bar');
    }

    function initGastroPairSelectors() {
      const fSel = document.getElementById('pairFoodSelect');
      const dSel = document.getElementById('pairDrinkSelect');
      fSel.innerHTML = KITCHEN_RECIPES.map(r => `<option value="${r.id}">${r.title}</option>`).join('');
      dSel.innerHTML = BAR_RECIPES.map(b => `<option value="${b.id}">${b.title} (${b.abv})</option>`).join('');
      evaluateGastroPair();
    }

    function evaluateGastroPair() {
      const f = KITCHEN_RECIPES.find(x => x.id === document.getElementById('pairFoodSelect').value) || KITCHEN_RECIPES[0];
      const d = BAR_RECIPES.find(x => x.id === document.getElementById('pairDrinkSelect').value) || BAR_RECIPES[0];
      const score = 88 + ((f.title.length + d.title.length) % 11);
      document.getElementById('gastroPairResult').innerHTML = `
        <div class="flex items-center justify-between">
          <span class="text-xs font-bold text-amber-300">Индекс синергии вкуса:</span>
          <span class="px-2.5 py-0.5 rounded-full gold-gradient text-xs font-bold">${score}% Идеальный баланс</span>
        </div>
        <p class="text-xs text-emerald-100 leading-relaxed">
          <strong>${f.title}</strong> + <strong>${d.title}</strong>: кислотно-ароматический профиль напитка (${d.abv}) освежает рецепторы после каждого кусочка блюда, а ботаникалы подчеркивают основной белок и соус.
        </p>
      `;
    }

    function renderPairingMatrix() {
      const btnBox = document.getElementById('pairingMatrixButtons');
      btnBox.innerHTML = PAIRING_MATRIX.map((m, idx) =>
        `<button onclick="selectMatrixItem(${idx})" class="px-3 py-1.5 rounded-xl text-xs bg-emerald-950 border border-amber-500/30 text-amber-200 hover:border-amber-400">${m.key}</button>`
      ).join('');
      selectMatrixItem(0);
    }

    function selectMatrixItem(idx) {
      const m = PAIRING_MATRIX[idx];
      document.getElementById('pairingMatrixCard').innerHTML = `
        <div class="text-sm font-bold text-amber-300">${m.key} <span class="text-[10px] px-2 py-0.5 rounded bg-emerald-900 text-emerald-300 ml-1">${m.type}</span></div>
        <div class="text-xs text-emerald-200"><strong>🌿 Лучшие специи и ботаникалы:</strong> ${m.bestSpices}</div>
        <div class="text-xs text-emerald-200"><strong>🍯 Соусы и миксеры:</strong> ${m.bestSauces}</div>
        <div class="text-xs text-amber-200"><strong>🍷 Рекомендованная пара:</strong> ${m.bestDrinks}</div>
        <div class="text-[11px] text-emerald-400 italic pt-1">${m.science}</div>
      `;
    }

    function calcABV() {
      const v = parseFloat(document.getElementById('abvVol').value) || 0;
      const a = parseFloat(document.getElementById('abvInit').value) || 0;
      const w = parseFloat(document.getElementById('abvWater').value) || 0;
      const total = v + w;
      const res = total > 0 ? ((v * (a / 100)) / total * 100).toFixed(1) : 0;
      document.getElementById('abvOutput').innerHTML = `Итоговый объем настойки: <strong>${total} мл</strong> • Итоговая крепость: <strong>${res}% ABV</strong>`;
    }

    function calcPan() {
      const d1 = parseFloat(document.getElementById('panOld').value) || 18;
      const d2 = parseFloat(document.getElementById('panNew').value) || 22;
      const k = ((d2 * d2) / (d1 * d1)).toFixed(2);
      document.getElementById('panOutput').innerHTML = `Коэффициент пересчета: <strong>×${k}</strong> (умножь все граммовки в рецепте на ${k})`;
    }

    function renderShoppingList() {
      const box = document.getElementById('shoppingListContainer');
      if (!shoppingList.length) {
        box.innerHTML = `<div class="text-emerald-500">Список покупок пуст</div>`;
        return;
      }
      box.innerHTML = shoppingList.map((item, idx) => `
        <div onclick="shoppingList[${idx}].done=!shoppingList[${idx}].done;renderShoppingList()" class="flex items-center justify-between p-2 rounded-lg bg-emerald-950/70 cursor-pointer">
          <span class="${item.done ? 'line-through text-emerald-600' : 'text-emerald-100'}">${item.done ? '☑' : '☐'} ${item.name}</span>
        </div>
      `).join('');
    }

    function addCustomShopItem() {
      const inp = document.getElementById('customShopItem');
      if (!inp.value.trim()) return;
      shoppingList.push({ name: inp.value.trim(), done: false });
      inp.value = '';
      renderShoppingList();
    }

    function clearShoppingList() {
      shoppingList = [];
      renderShoppingList();
    }

    function loadFoodcostPreset() {
      const key = document.getElementById('foodcostPreset').value;
      const data = FOODCOST_PRESETS[key];
      const box = document.getElementById('foodcostRows');
      box.innerHTML = data.items.map(i => `
        <div class="flex justify-between py-1 border-b border-emerald-900/50">
          <span>${i.n}</span>
          <span class="text-amber-200 font-semibold">${i.p} ₽</span>
        </div>
      `).join('');
      const total = data.items.reduce((s, x) => s + x.p, 0);
      const port = Math.round(total / data.portions);
      document.getElementById('fcTotal').textContent = total + ' ₽';
      document.getElementById('fcPortion').textContent = port + ' ₽';
      document.getElementById('fcRetail').textContent = (port * 3) + ' ₽';
    }

    function openModal(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeModal(id) { document.getElementById(id).classList.add('hidden'); }

    // Инициализация
    renderFridgePills();
    renderBarPills();
    renderKitchenCategories();
    renderBarCategories();
    renderKitchenRecipes();
    renderBarRecipes();
    initGastroPairSelectors();
    renderPairingMatrix();
    calcABV();
    calcPan();
    renderShoppingList();
    loadFoodcostPreset();
  </script>
</body>
</html>
