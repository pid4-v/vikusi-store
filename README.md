<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>VIKUSI by K&K — Косметика и ароматы с характером</title>
  
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- React & ReactDOM -->
  <script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
  <!-- Babel for JSX -->
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@600;700&family=Cormorant+Garamond:ital,wght@0,500;0,600;0,700;1,400&family=Montserrat:wght@300;400;500;600;700&display=swap" rel="stylesheet">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brandMilk: '#FAF6F0',
            brandPink: '#FCE7F3',
            brandPinkBright: '#EC4899',
            brandBurgundy: '#4C0519',
            brandDark: '#1C1917',
            brandAccent: '#E11D48',
          },
          fontFamily: {
            serif: ['Cormorant Garamond', 'serif'],
            sans: ['Montserrat', 'sans-serif'],
            handwriting: ['Caveat', 'cursive'],
          }
        }
      }
    }
  </script>
</head>
<body class="bg-brandMilk text-brandDark font-sans antialiased selection:bg-brandPink selection:text-brandBurgundy min-h-screen">

  <div id="root"></div>

  <script type="text/babel">
    const { useState, useEffect } = React;

    // База данных товаров с вашими описаниями и именами файлов для изображений
    const PRODUCTS_DATA = [
      {
        id: 'shampoo',
        slug: 'shampoo',
        name: 'Шампунь',
        fullName: 'Шампунь с запахом VIKUSI',
        price: 29.99,
        image: 'image_899d4b.jpg', // Фото шампуня
        shortDesc: 'Для тех, кто любит покушать и быть красивым! ♡',
        fullDesc: 'Шампунь VIKUSI создан для повседневного ухода за волосами. Он помогает эффективно очищать волосы и кожу головы от загрязнений, оставляя неповторимое ощущение свежести и уникальный фирменный запах Викуси. Простой и приятный шаг в уходе, который хочется повторять каждый день.',
        funQuote: 'Для тех, кто любит покушать и быть красивым! ♡',
        badge: 'Хит продаж',
        specs: { volume: '250 мл', type: 'Для всех типов волос', scent: 'Запах Викуси' },
        usage: 'Нанесите небольшое количество шампуня на влажные волосы, вспеньте массирующими движениями и тщательно смойте водой.'
      },
      {
        id: 'shower-gel',
        slug: 'shower-gel',
        name: 'Гель для душа',
        fullName: 'Гель для душа с запахом картошки VIKUSI',
        price: 24.99,
        image: 'image_899d68.jpg', // Фото геля для душа
        shortDesc: 'Чистота, которая вкусно пахнет! ♡',
        fullDesc: 'Гель для душа VIKUSI превращает привычный душ в настоящий ритуал удовольствия. Его фирменная концепция — оригинальный и невероятно запоминающийся аромат картошки Викуси. Это продукт для тех, кто ценит оригинальность, хорошую шутку и хочет добавить немного характера в свой день.',
        funQuote: 'Чистота, которая вкусно пахнет! ♡',
        badge: 'Эксклюзив',
        specs: { volume: '250 мл', type: 'Для тела', scent: 'Картошка Викуси' },
        usage: 'Нанесите гель на влажную кожу или мочалку, вспеньте мягкими движениями и смойте теплой водой.'
      },
      {
        id: 'hair-mask',
        slug: 'hair-mask',
        name: 'Маска для волос',
        fullName: 'Маска для волос с запахом картошки VIKUSI',
        price: 34.99,
        image: 'image_899d83.jpg', // Фото маски для волос
        shortDesc: 'Волосы как у Вики: густые, вкусные, красивые! ♡',
        fullDesc: 'Маска для волос VIKUSI — ваш особый ритуал глубокого ухода. Сочетает в себе питательную формулу и фирменный ароматический аккорд картошки. Создана для моментов, когда хочется уделить волосам чуть больше внимания и сделать их по-настоящему классными.',
        funQuote: 'Волосы как у Вики: густые, вкусные, красивые! ♡',
        badge: 'Любимый ритуал',
        specs: { volume: '200 мл', type: 'Питательный уход', scent: 'Картошка Викуси' },
        usage: 'Нанесите на чистые влажные волосы, распределите по всей длине, оставьте на 5-10 минут, затем смойте водой.'
      },
      {
        id: 'face-cream',
        slug: 'face-cream',
        name: 'Крем для лица',
        fullName: 'Крем для лица VIKUSI',
        price: 39.99,
        image: 'image_899d88.jpg', // Фото крема для лица
        shortDesc: 'Чтобы стать такой же красивой как Вика ♡',
        fullDesc: 'Крем для лица VIKUSI — базовый этап вашего ежедневного ритуала. Увлажняет, делает классной и работает даже тогда, когда у вас плохое настроение! Сочетает комфортную текстуру и приятное ощущение заботы о себе.',
        funQuote: 'Увлажняет. Делает классной. Работает даже в плохом настроении! ♡',
        badge: 'MUST HAVE',
        specs: { volume: '50 мл', type: 'Дневной / Ночной уход', scent: 'Нежная свежесть' },
        usage: 'Легкими массирующими движениями нанесите небольшое количество крема на очищенную кожу лица.'
      },
      {
        id: 'perfume',
        slug: 'perfume',
        name: 'Духи',
        fullName: 'Духи VIKUSI — Eau de Vika (Гнилая Картошка)',
        price: 59.99,
        image: 'image_899da4.jpg', // Фото парфюма
        shortDesc: 'Эксклюзивный аромат Eau de Vika (50 ml). Выбор ALEKSANDRA GIELNIAK!',
        fullDesc: 'Духи VIKUSI «Гнилая Картошка / Eau de Vika» — это культовый, дерзкий и незабываемый парфюмерный шедевр в роскошном стеклянном флаконе. Финальный штрих вашего неповторимого образа. Особый статус аромата подтверждён: этими духами пользовалась сама ALEKSANDRA GIELNIAK!',
        funQuote: 'Eau de Vika — концептуальная парфюмерия с настоящим характером!',
        badge: 'Выбор ALEKSANDRA GIELNIAK',
        specs: { volume: '50 мл', type: 'Eau de Parfum', scent: 'Гнилая Картошка / Eau de Vika' },
        usage: 'Распылите духи на пульсирующие точки: запястья, шею и за ушами.'
      }
    ];

    function App() {
      const [currentRoute, setCurrentRoute] = useState('home'); // 'home', 'catalog', 'product', 'about', 'contacts'
      const [selectedProductId, setSelectedProductId] = useState('shampoo');
      const [cart, setCart] = useState([]);
      const [isCartOpen, setIsCartOpen] = useState(false);
      const [isCheckoutOpen, setIsCheckoutOpen] = useState(false);
      const [toastMessage, setToastMessage] = useState('');

      // Уведомления
      const showToast = (msg) => {
        setToastMessage(msg);
        setTimeout(() => setToastMessage(''), 3000);
      };

      // Переход на страницу товара
      const navigateToProduct = (id) => {
        setSelectedProductId(id);
        setCurrentRoute('product');
        window.scrollTo({ top: 0, behavior: 'smooth' });
      };

      // Переход по меню
      const navigateTo = (route) => {
        setCurrentRoute(route);
        window.scrollTo({ top: 0, behavior: 'smooth' });
      };

      // Добавление в корзину
      const addToCart = (product, quantity = 1) => {
        setCart(prev => {
          const existing = prev.find(item => item.id === product.id);
          if (existing) {
            return prev.map(item => 
              item.id === product.id ? { ...item, qty: item.qty + quantity } : item
            );
          }
          return [...prev, { ...product, qty: quantity }];
        });
        showToast(`«${product.name}» добавлен в корзину!`);
      };

      const updateQty = (id, delta) => {
        setCart(prev => prev.map(item => {
          if (item.id === id) {
            const newQty = item.qty + delta;
            return newQty > 0 ? { ...item, qty: newQty } : null;
          }
          return item;
        }).filter(Boolean));
      };

      const totalCartPrice = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
      const totalCartCount = cart.reduce((sum, item) => sum + item.qty, 0);

      const currentProduct = PRODUCTS_DATA.find(p => p.id === selectedProductId) || PRODUCTS_DATA[0];

      return (
        <div class="min-h-screen flex flex-col justify-between">
          
          {/* ВСПЛЫВАЮЩЕЕ УВЕДОМЛЕНИЕ */}
          {toastMessage && (
            <div class="fixed bottom-6 right-6 z-50 bg-brandBurgundy text-white px-6 py-3 rounded-full shadow-2xl flex items-center gap-3 animate-bounce border border-rose-300/30">
              <span class="text-xl">✨</span>
              <span class="text-sm font-medium">{toastMessage}</span>
            </div>
          )}

          {/* ШАПКА САЙТА */}
          <header class="sticky top-0 z-40 bg-brandMilk/90 backdrop-blur-md border-b border-rose-100 transition-all">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
              
              {/* ЛОГОТИП */}
              <button onClick={() => navigateTo('home')} class="text-left group">
                <div class="flex items-center gap-1">
                  <span class="font-serif text-2xl sm:text-3xl font-bold tracking-wider text-brandBurgundy group-hover:text-brandAccent transition">
                    VIKUSI
                  </span>
                  <span class="text-xs text-brandPinkBright font-extrabold tracking-widest relative -top-2">👑</span>
                </div>
                <div class="text-[10px] tracking-[0.25em] text-rose-800 font-semibold uppercase -mt-1">
                  by K&K
                </div>
              </button>

              {/* МЕНЮ НАВИГАЦИИ */}
              <nav class="hidden md:flex items-center gap-8 text-xs font-semibold uppercase tracking-widest text-stone-700">
                <button onClick={() => navigateTo('home')} class={`hover:text-brandBurgundy transition ${currentRoute === 'home' ? 'text-brandBurgundy border-b-2 border-brandBurgundy pb-1' : ''}`}>
                  Главная
                </button>
                <button onClick={() => navigateTo('catalog')} class={`hover:text-brandBurgundy transition ${currentRoute === 'catalog' ? 'text-brandBurgundy border-b-2 border-brandBurgundy pb-1' : ''}`}>
                  Каталог
                </button>
                <button onClick={() => navigateTo('about')} class={`hover:text-brandBurgundy transition ${currentRoute === 'about' ? 'text-brandBurgundy border-b-2 border-brandBurgundy pb-1' : ''}`}>
                  О бренде
                </button>
                <button onClick={() => navigateTo('contacts')} class={`hover:text-brandBurgundy transition ${currentRoute === 'contacts' ? 'text-brandBurgundy border-b-2 border-brandBurgundy pb-1' : ''}`}>
                  Контакты
                </button>
              </nav>

              {/* КОРЗИНА И ИКОНКА */}
              <button 
                onClick={() => setIsCartOpen(true)}
                class="relative bg-rose-100/70 hover:bg-rose-200/80 text-brandBurgundy px-4 py-2 rounded-full flex items-center gap-2 transition font-medium text-xs sm:text-sm border border-rose-200"
              >
                <span>🛒</span>
                <span class="hidden sm:inline">Корзина</span>
                {totalCartCount > 0 && (
                  <span class="bg-brandBurgundy text-white text-[11px] font-bold w-5 h-5 rounded-full flex items-center justify-center">
                    {totalCartCount}
                  </span>
                )}
              </button>
            </div>

            {/* Мобильная навигация */}
            <div class="md:hidden flex justify-around py-2 border-t border-rose-100 text-[11px] font-medium text-stone-600 bg-white/50">
              <button onClick={() => navigateTo('home')} class={currentRoute === 'home' ? 'text-brandBurgundy font-bold' : ''}>Главная</button>
              <button onClick={() => navigateTo('catalog')} class={currentRoute === 'catalog' ? 'text-brandBurgundy font-bold' : ''}>Каталог</button>
              <button onClick={() => navigateTo('about')} class={currentRoute === 'about' ? 'text-brandBurgundy font-bold' : ''}>О бренде</button>
              <button onClick={() => navigateTo('contacts')} class={currentRoute === 'contacts' ? 'text-brandBurgundy font-bold' : ''}>Контакты</button>
            </div>
          </header>

          {/* ОСНОВНОЙ КОНТЕНТ */}
          <main class="flex-grow">
            
            {/* 1. ГЛАВНАЯ СТРАНИЦА */}
            {currentRoute === 'home' && (
              <div>
                {/* HERO SECTION */}
                <section class="relative py-16 lg:py-24 overflow-hidden bg-gradient-to-b from-rose-50/50 to-transparent">
                  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid md:grid-cols-2 gap-12 items-center">
                    <div>
                      <span class="inline-block px-3 py-1 bg-rose-200/60 text-brandBurgundy rounded-full text-xs font-semibold tracking-wider uppercase mb-4">
                        Beauty made personal
                      </span>
                      <h1 class="font-serif text-5xl sm:text-6xl lg:text-7xl font-normal leading-tight text-brandBurgundy mb-6">
                        Уход с характером.
                      </h1>
                      <p class="text-stone-600 text-lg mb-8 leading-relaxed font-light max-w-lg">
                        Открой мир <strong class="font-semibold text-brandBurgundy">VIKUSI by K&K</strong> — косметики и ароматов, которые превращают обычные моменты в маленький эстетичный ритуал удовольствия.
                      </p>
                      <div class="flex flex-wrap gap-4">
                        <button 
                          onClick={() => navigateTo('catalog')}
                          class="bg-brandBurgundy hover:bg-rose-950 text-white px-8 py-4 rounded-full font-medium text-sm tracking-widest uppercase transition shadow-lg shadow-rose-900/10"
                        >
                          Открыть коллекцию
                        </button>
                      </div>
                      <div class="mt-8 text-xs text-stone-500 font-handwriting text-xl">
                        «Твой ритуал. Твой аромат. Твой VIKUSI.»
                      </div>
                    </div>

                    {/* Галерея-коллаж продуктов */}
                    <div class="relative grid grid-cols-2 gap-4">
                      <div class="space-y-4">
                        <div class="bg-white p-3 rounded-2xl shadow-md border border-rose-100 hover:scale-105 transition duration-300">
                          <img 
                            src="image_899d4b.jpg" 
                            alt="Шампунь VIKUSI" 
                            class="w-full h-48 sm:h-64 object-cover rounded-xl"
                            onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1535585209827-a15fcdbc4c2d?auto=format&fit=crop&w=600&q=80'; }}
                          />
                          <p class="mt-2 text-xs font-semibold text-center text-brandBurgundy">Шампунь с запахом VIKUSI</p>
                        </div>
                        <div class="bg-white p-3 rounded-2xl shadow-md border border-rose-100 hover:scale-105 transition duration-300">
                          <img 
                            src="image_899d68.jpg" 
                            alt="Гель для душа" 
                            class="w-full h-36 sm:h-48 object-cover rounded-xl"
                            onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1556228720-195a672e8a03?auto=format&fit=crop&w=600&q=80'; }}
                          />
                          <p class="mt-2 text-xs font-semibold text-center text-brandBurgundy">Гель с запахом картошки</p>
                        </div>
                      </div>

                      <div class="space-y-4 pt-8">
                        <div class="bg-white p-3 rounded-2xl shadow-md border border-rose-100 hover:scale-105 transition duration-300">
                          <img 
                            src="image_899da4.jpg" 
                            alt="Духи Eau de Vika" 
                            class="w-full h-56 sm:h-72 object-cover rounded-xl"
                            onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1541643600914-78b084683601?auto=format&fit=crop&w=600&q=80'; }}
                          />
                          <p class="mt-2 text-xs font-semibold text-center text-brandBurgundy">Eau de Vika (50 ml)</p>
                        </div>
                      </div>
                    </div>
                  </div>
                </section>

                {/* БЛОК КАТАЛОГА */}
                <section class="py-20 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                  <div class="text-center max-w-2xl mx-auto mb-16">
                    <h2 class="font-serif text-4xl sm:text-5xl text-brandBurgundy mb-4">Твоя коллекция ухода</h2>
                    <p class="text-stone-600 text-sm tracking-wide">Пять продуктов. Пять настроений. Один VIKUSI.</p>
                  </div>

                  <div class="grid md:grid-cols-3 gap-8">
                    {PRODUCTS_DATA.map((product, idx) => (
                      <div 
                        key={product.id}
                        onClick={() => navigateToProduct(product.id)}
                        class={`group cursor-pointer bg-white rounded-3xl p-5 border border-rose-100/80 shadow-sm hover:shadow-xl hover:-translate-y-1 transition duration-300 flex flex-col justify-between ${idx === 4 ? 'md:col-span-2 lg:col-span-1' : ''}`}
                      >
                        <div>
                          <div class="relative overflow-hidden rounded-2xl mb-4 bg-rose-50/50 aspect-square flex items-center justify-center">
                            <img 
                              src={product.image} 
                              alt={product.fullName}
                              class="w-full h-full object-cover group-hover:scale-105 transition duration-500"
                              onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1556228720-195a672e8a03?auto=format&fit=crop&w=600&q=80'; }}
                            />
                            <span class="absolute top-3 left-3 bg-white/90 backdrop-blur text-brandBurgundy text-[10px] font-bold px-3 py-1 rounded-full uppercase tracking-wider border border-rose-100 shadow-sm">
                              {product.badge}
                            </span>
                          </div>

                          <h3 class="font-serif text-2xl text-stone-900 group-hover:text-brandBurgundy transition mb-1">
                            {product.name}
                          </h3>
                          <p class="text-xs text-rose-800 font-medium mb-3 italic">
                            «{product.fullName}»
                          </p>
                          <p class="text-stone-600 text-xs line-clamp-2 mb-4 leading-relaxed">
                            {product.funQuote}
                          </p>
                        </div>

                        <div class="pt-4 border-t border-rose-50 flex items-center justify-between">
                          <span class="font-serif text-xl font-bold text-brandBurgundy">
                            {product.price.toFixed(2)} zł
                          </span>
                          <span class="text-xs font-semibold text-rose-700 group-hover:underline flex items-center gap-1">
                            Подробнее →
                          </span>
                        </div>
                      </div>
                    ))}
                  </div>
                </section>

                {/* ПОЧЕМУ VIKUSI */}
                <section class="py-16 bg-rose-100/40 border-y border-rose-200/50">
                  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                    <h2 class="font-serif text-3xl text-center text-brandBurgundy mb-12">Почему VIKUSI?</h2>
                    <div class="grid md:grid-cols-3 gap-8">
                      <div class="bg-white/80 p-8 rounded-2xl border border-rose-100 text-center">
                        <div class="text-3xl mb-4">👑</div>
                        <h3 class="font-serif text-xl text-brandBurgundy mb-2">Уход с индивидуальностью</h3>
                        <p class="text-xs text-stone-600 leading-relaxed">Каждый продукт имеет свой уникальный характер, настроение и концептуальный аромат.</p>
                      </div>
                      <div class="bg-white/80 p-8 rounded-2xl border border-rose-100 text-center">
                        <div class="text-3xl mb-4">✨</div>
                        <h3 class="font-serif text-xl text-brandBurgundy mb-2">Красота в деталях</h3>
                        <p class="text-xs text-stone-600 leading-relaxed">Мы уделяем внимание эстетике упаковки, приятной текстуре и самому ощущению заботы о себе.</p>
                      </div>
                      <div class="bg-white/80 p-8 rounded-2xl border border-rose-100 text-center">
                        <div class="text-3xl mb-4">🥔</div>
                        <h3 class="font-serif text-xl text-brandBurgundy mb-2">Маленькие ритуалы</h3>
                        <p class="text-xs text-stone-600 leading-relaxed">Даже обычный душ или уход за волосами превращается в эстетичную и весёлую часть дня.</p>
                      </div>
                    </div>
                  </div>
                </section>

                {/* БАННЕР ФИРМЕННОГО ПОСЛАНИЯ */}
                <section class="py-20 text-center max-w-4xl mx-auto px-4">
                  <h2 class="font-serif text-4xl sm:text-6xl text-brandBurgundy leading-tight mb-6">
                    Не просто уход. Твоё настроение во флаконе.
                  </h2>
                  <p class="text-stone-600 text-base mb-8">Найди свой идеальный аромат и свой VIKUSI прямо сейчас.</p>
                  <button 
                    onClick={() => navigateTo('catalog')}
                    class="bg-brandBurgundy text-white px-10 py-4 rounded-full font-medium text-sm tracking-widest uppercase hover:bg-rose-900 transition shadow-lg"
                  >
                    Выбрать свой VIKUSI
                  </button>
                </section>
              </div>
            )}

            {/* 2. СТРАНИЦА КАТАЛОГА */}
            {currentRoute === 'catalog' && (
              <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
                <div class="mb-12 text-center">
                  <h1 class="font-serif text-4xl sm:text-5xl text-brandBurgundy mb-3">Каталог VIKUSI</h1>
                  <p class="text-stone-600 text-sm">Полная коллекция косметики и парфюмерии</p>
                </div>

                <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
                  {PRODUCTS_DATA.map((product) => (
                    <div 
                      key={product.id}
                      class="bg-white rounded-3xl p-6 border border-rose-100 shadow-sm flex flex-col justify-between"
                    >
                      <div>
                        <div 
                          onClick={() => navigateToProduct(product.id)}
                          class="cursor-pointer overflow-hidden rounded-2xl mb-4 bg-rose-50 aspect-square"
                        >
                          <img 
                            src={product.image} 
                            alt={product.fullName}
                            class="w-full h-full object-cover hover:scale-105 transition duration-500"
                            onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1556228720-195a672e8a03?auto=format&fit=crop&w=600&q=80'; }}
                          />
                        </div>
                        <span class="text-[10px] font-bold text-rose-800 bg-rose-100 px-3 py-1 rounded-full uppercase tracking-wider">
                          {product.badge}
                        </span>
                        <h3 
                          onClick={() => navigateToProduct(product.id)}
                          class="font-serif text-2xl text-stone-900 hover:text-brandBurgundy cursor-pointer mt-3 mb-1"
                        >
                          {product.fullName}
                        </h3>
                        <p class="text-xs text-stone-500 mb-4">{product.shortDesc}</p>
                      </div>

                      <div class="space-y-3 pt-4 border-t border-rose-50">
                        <div class="flex justify-between items-center">
                          <span class="font-serif text-2xl font-bold text-brandBurgundy">{product.price.toFixed(2)} zł</span>
                          <span class="text-xs text-stone-400">{product.specs.volume}</span>
                        </div>
                        <div class="flex gap-2">
                          <button 
                            onClick={() => navigateToProduct(product.id)}
                            class="flex-1 border border-brandBurgundy text-brandBurgundy py-2 rounded-xl text-xs font-semibold hover:bg-rose-50 transition"
                          >
                            Подробнее
                          </button>
                          <button 
                            onClick={() => addToCart(product)}
                            class="bg-brandBurgundy text-white px-4 py-2 rounded-xl text-xs font-semibold hover:bg-rose-950 transition"
                          >
                            В корзину
                          </button>
                        </div>
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            )}

            {/* 3. СТРАНИЦА ОТДЕЛЬНОГО ТОВАРА */}
            {currentRoute === 'product' && (
              <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
                <button 
                  onClick={() => navigateTo('catalog')}
                  class="text-xs font-semibold text-rose-800 hover:text-brandBurgundy mb-8 flex items-center gap-1"
                >
                  ← Назад в каталог
                </button>

                <div class="grid md:grid-cols-2 gap-12 items-start">
                  {/* ИЗОБРАЖЕНИЕ ТОВАРА */}
                  <div class="bg-white p-4 rounded-3xl border border-rose-100 shadow-md">
                    <img 
                      src={currentProduct.image} 
                      alt={currentProduct.fullName}
                      class="w-full h-auto max-h-[550px] object-cover rounded-2xl"
                      onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1556228720-195a672e8a03?auto=format&fit=crop&w=600&q=80'; }}
                    />
                  </div>

                  {/* ИНФОРМАЦИЯ О ТОВАРЕ */}
                  <div class="space-y-6">
                    <div>
                      <span class="bg-rose-100 text-brandBurgundy text-[11px] font-bold px-3 py-1 rounded-full uppercase tracking-wider">
                        {currentProduct.badge}
                      </span>
                      <h1 class="font-serif text-4xl sm:text-5xl text-brandBurgundy mt-3 mb-2">
                        {currentProduct.fullName}
                      </h1>
                      <div class="font-serif text-3xl font-bold text-stone-900">
                        {currentProduct.price.toFixed(2)} zł
                      </div>
                    </div>

                    <div class="p-4 bg-rose-50/70 border border-rose-200/60 rounded-2xl font-handwriting text-2xl text-brandBurgundy">
                      «{currentProduct.funQuote}»
                    </div>

                    <p class="text-stone-600 text-sm leading-relaxed">
                      {currentProduct.fullDesc}
                    </p>

                    {/* Выбор количества и кнопка */}
                    <div class="pt-4 border-t border-rose-100 space-y-4">
                      <div class="flex items-center gap-4">
                        <button 
                          onClick={() => addToCart(currentProduct)}
                          class="flex-1 bg-brandBurgundy hover:bg-rose-950 text-white py-4 rounded-2xl font-medium text-sm tracking-wider uppercase transition shadow-lg flex items-center justify-center gap-2"
                        >
                          <span>🛒</span> Добавить в корзину
                        </button>
                      </div>
                    </div>

                    {/* Особый блок для духов — ALEKSANDRA GIELNIAK */}
                    {currentProduct.id === 'perfume' && (
                      <div class="p-5 bg-gradient-to-r from-amber-50 to-rose-50 border border-amber-200 rounded-2xl flex items-center gap-4">
                        <div class="w-16 h-16 rounded-full overflow-hidden flex-shrink-0 border-2 border-amber-300">
                          <img 
                            src="aleksandra.jpg" 
                            alt="Aleksandra Gielniak" 
                            class="w-full h-full object-cover"
                            onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=300&q=80'; }}
                          />
                        </div>
                        <div>
                          <div class="text-xs font-bold text-amber-900 uppercase tracking-wider">Особый выбор</div>
                          <div class="font-serif text-lg font-bold text-stone-900">Этими духами пользовалась сама ALEKSANDRA GIELNIAK!</div>
                          <p class="text-xs text-stone-600 mt-1">Официальный амбассадор настроения Eau de Vika.</p>
                        </div>
                      </div>
                    )}

                    {/* Характеристики */}
                    <div class="space-y-3 pt-4 border-t border-rose-100 text-xs">
                      <div class="flex justify-between py-1 border-b border-stone-100">
                        <span class="text-stone-500">Объем:</span>
                        <span class="font-semibold text-stone-800">{currentProduct.specs.volume}</span>
                      </div>
                      <div class="flex justify-between py-1 border-b border-stone-100">
                        <span class="text-stone-500">Аромат:</span>
                        <span class="font-semibold text-stone-800">{currentProduct.specs.scent}</span>
                      </div>
                      <div class="flex justify-between py-1 border-b border-stone-100">
                        <span class="text-stone-500">Тип:</span>
                        <span class="font-semibold text-stone-800">{currentProduct.specs.type}</span>
                      </div>
                    </div>

                    {/* Как использовать */}
                    <div class="bg-white p-5 rounded-2xl border border-rose-100 text-xs leading-relaxed space-y-1">
                      <div class="font-bold text-brandBurgundy uppercase tracking-wider mb-1">Способ применения:</div>
                      <p class="text-stone-600">{currentProduct.usage}</p>
                    </div>

                  </div>
                </div>
              </div>
            )}

            {/* 4. О БРЕНДЕ */}
            {currentRoute === 'about' && (
              <div class="max-w-4xl mx-auto px-4 py-16 text-center">
                <h1 class="font-serif text-5xl text-brandBurgundy mb-6">О бренде VIKUSI by K&K</h1>
                <p class="text-lg text-stone-600 leading-relaxed mb-8">
                  VIKUSI by K&K — это косметика и ароматы с характером. Мы превращаем повседневный уход в маленький ритуал удовольствия, где каждая деталь имеет значение: текстура, аромат, настроение и впечатление.
                </p>
                <div class="p-8 bg-white rounded-3xl border border-rose-100 shadow-sm text-left space-y-4 mb-8">
                  <p class="text-sm text-stone-600 leading-relaxed">
                    От шампуня до парфюма — наши продукты созданы для того, чтобы обычные моменты становились особенными. Мы любим индивидуальность, необычные идеи и красоту без скучных правил.
                  </p>
                  <p class="text-sm font-semibold text-brandBurgundy">
                    VIKUSI by K&K — уход, который невозможно перепутать с другим.
                  </p>
                </div>
              </div>
            )}

            {/* 5. КОНТАКТЫ */}
            {currentRoute === 'contacts' && (
              <div class="max-w-2xl mx-auto px-4 py-16 text-center">
                <h1 class="font-serif text-5xl text-brandBurgundy mb-6">Контакты</h1>
                <div class="bg-white p-8 rounded-3xl border border-rose-100 shadow-sm space-y-6 text-left">
                  <div>
                    <div class="text-xs uppercase text-stone-400 tracking-wider">Email</div>
                    <div class="text-stone-800 font-medium">hello@vikusi-beauty.com</div>
                  </div>
                  <div>
                    <div class="text-xs uppercase text-stone-400 tracking-wider">Инстаграм</div>
                    <div class="text-stone-800 font-medium">@vikusi.by.kk</div>
                  </div>
                  <div>
                    <div class="text-xs uppercase text-stone-400 tracking-wider">Локация</div>
                    <div class="text-stone-800 font-medium">Польша, Варшава</div>
                  </div>
                </div>
              </div>
            )}

          </main>

          {/* ВЫЕЗЖАЮЩАЯ КОРЗИНА */}
          {isCartOpen && (
            <div class="fixed inset-0 z-50 overflow-hidden">
              <div class="absolute inset-0 bg-stone-900/40 backdrop-blur-sm" onClick={() => setIsCartOpen(false)}></div>
              <div class="absolute inset-y-0 right-0 max-w-full flex pl-10">
                <div class="w-screen max-w-md bg-brandMilk shadow-2xl flex flex-col justify-between">
                  
                  {/* Шапка корзины */}
                  <div class="p-6 border-b border-rose-100 flex items-center justify-between bg-white">
                    <h2 class="font-serif text-2xl text-brandBurgundy font-bold">Твоя корзина</h2>
                    <button onClick={() => setIsCartOpen(false)} class="text-stone-400 hover:text-stone-700 text-xl font-bold">✕</button>
                  </div>

                  {/* Список товаров */}
                  <div class="flex-grow overflow-y-auto p-6 space-y-4">
                    {cart.length === 0 ? (
                      <div class="text-center py-12 text-stone-400 text-sm">
                        Ваша корзина пока пуста 🧺
                      </div>
                    ) : (
                      cart.map(item => (
                        <div key={item.id} class="bg-white p-4 rounded-2xl border border-rose-100 flex gap-4 items-center">
                          <img src={item.image} alt={item.name} class="w-16 h-16 object-cover rounded-xl" onError={(e) => { e.target.src = 'https://images.unsplash.com/photo-1556228720-195a672e8a03?auto=format&fit=crop&w=600&q=80'; }} />
                          <div class="flex-grow">
                            <h4 class="font-serif font-bold text-stone-800 text-sm">{item.name}</h4>
                            <div class="text-xs text-brandBurgundy font-semibold">{item.price.toFixed(2)} zł</div>
                            <div class="flex items-center gap-2 mt-2">
                              <button onClick={() => updateQty(item.id, -1)} class="w-6 h-6 rounded-md bg-stone-100 text-xs font-bold">-</button>
                              <span class="text-xs font-semibold">{item.qty}</span>
                              <button onClick={() => updateQty(item.id, 1)} class="w-6 h-6 rounded-md bg-stone-100 text-xs font-bold">+</button>
                            </div>
                          </div>
                        </div>
                      ))
                    )}
                  </div>

                  {/* Подвал корзины */}
                  {cart.length > 0 && (
                    <div class="p-6 bg-white border-t border-rose-100 space-y-4">
                      <div class="flex justify-between items-center text-lg font-serif">
                        <span>Итого:</span>
                        <span class="font-bold text-brandBurgundy">{totalCartPrice.toFixed(2)} zł</span>
                      </div>
                      <button 
                        onClick={() => { setIsCartOpen(false); setIsCheckoutOpen(true); }}
                        class="w-full bg-brandBurgundy text-white py-4 rounded-2xl font-semibold text-xs uppercase tracking-widest hover:bg-rose-950 transition"
                      >
                        Оформить заказ
                      </button>
                    </div>
                  )}

                </div>
              </div>
            </div>
          )}

          {/* ОФОРМЛЕНИЕ ЗАКАЗА (ДЕМО) */}
          {isCheckoutOpen && (
            <div class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-stone-900/50 backdrop-blur-sm">
              <div class="bg-white rounded-3xl p-8 max-w-md w-full text-center space-y-4 border border-rose-100 shadow-2xl">
                <div class="text-4xl">🛍️</div>
                <h3 class="font-serif text-3xl text-brandBurgundy">Оформление заказа</h3>
                <p class="text-xs text-stone-600 leading-relaxed">
                  Демонстрационный режим. Магазин подготовлен к работе и подключению платежной системы!
                </p>
                <div class="p-4 bg-rose-50 rounded-2xl text-left text-xs space-y-2 text-stone-700">
                  <div class="flex justify-between"><span>Товаров в заказе:</span> <strong>{totalCartCount} шт.</strong></div>
                  <div class="flex justify-between"><span>Сумма к оплате:</span> <strong class="text-brandBurgundy">{totalCartPrice.toFixed(2)} zł</strong></div>
                </div>
                <button 
                  onClick={() => setIsCheckoutOpen(false)}
                  class="w-full bg-brandBurgundy text-white py-3 rounded-xl font-semibold text-xs uppercase tracking-wider"
                >
                  Закрыть
                </button>
              </div>
            </div>
          )}

          {/* ПОДВАЛ (FOOTER) */}
          <footer class="bg-white border-t border-rose-100 py-12 text-stone-600 text-xs">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid md:grid-cols-4 gap-8">
              <div>
                <div class="font-serif text-2xl font-bold text-brandBurgundy mb-2">VIKUSI by K&K</div>
                <p class="text-stone-500 leading-relaxed">Твой ритуал. Твой аромат. Твой VIKUSI.</p>
              </div>
              <div>
                <h4 class="font-bold uppercase text-stone-800 mb-3 tracking-wider">Навигация</h4>
                <ul class="space-y-2">
                  <li><button onClick={() => navigateTo('home')} class="hover:text-brandBurgundy">Главная</button></li>
                  <li><button onClick={() => navigateTo('catalog')} class="hover:text-brandBurgundy">Каталог</button></li>
                  <li><button onClick={() => navigateTo('about')} class="hover:text-brandBurgundy">О бренде</button></li>
                </ul>
              </div>
              <div>
                <h4 class="font-bold uppercase text-stone-800 mb-3 tracking-wider">Контакты</h4>
                <p class="text-stone-500">Варшава, Польша</p>
                <p class="text-stone-500">hello@vikusi-beauty.com</p>
              </div>
              <div>
                <h4 class="font-bold uppercase text-stone-800 mb-3 tracking-wider">Соцсети</h4>
                <p class="text-stone-500">Instagram: @vikusi.by.kk</p>
              </div>
            </div>
            <div class="mt-8 pt-8 border-t border-rose-50 text-center text-stone-400">
              © VIKUSI by K&K · Beauty made personal
            </div>
          </footer>

        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById('root')).render(<App />);
  </script>
</body>
</html>
