# Animix-
The best streaming web for people who struggle to find one with every thing they need
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ANIMIX - Stream Animation & Action</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://code.iconify.design/iconify-icon/1.0.7/iconify-icon.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<script>
tailwind.config = {
  theme: {
    extend: {
      fontFamily: { sans: ['Inter', 'sans-serif'] },
    }
  }
}
</script>
<style>
  body { font-family: 'Inter', sans-serif; background: #050505; }

  /* Noise texture overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    opacity: 0.03;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  }

  /* Glass panel */
  .glass-panel {
    background: rgba(255, 255, 255, 0.03);
    backdrop-filter: blur(16px);
    border: 1px solid rgba(255, 255, 255, 0.08);
  }

  /* Scrollbar */
  .custom-scroll::-webkit-scrollbar { width: 4px; height: 4px; }
  .custom-scroll::-webkit-scrollbar-track { background: transparent; }
  .custom-scroll::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.1); border-radius: 999px; }

  /* Sketch lines animation */
  .sketch-line {
    stroke-dasharray: 1000;
    stroke-dashoffset: 1000;
    animation: draw 3s ease forwards;
  }
  @keyframes draw {
    to { stroke-dashoffset: 0; }
  }
  .sketch-line-delay { animation-delay: 0.5s; }
  .sketch-line-delay2 { animation-delay: 1s; }

  /* Category pill active */
  .cat-pill.active {
    background: #f97316;
    color: #fff;
    border-color: #f97316;
  }

  /* Movie card hover */
  .movie-card {
    transition: transform 500ms cubic-bezier(0, 0, 0.2, 1), box-shadow 500ms;
  }
  .movie-card:hover {
    transform: translateY(-8px) scale(1.02);
    box-shadow: 0 20px 40px -12px rgba(249, 115, 22, 0.2);
  }
  .movie-card:hover .card-overlay {
    opacity: 1;
  }
  .movie-card:hover .card-play {
    transform: scale(1);
    opacity: 1;
  }

  /* Featured play button pulse */
  .play-pulse {
    animation: pulse-ring 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
  }
  @keyframes pulse-ring {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  /* Sidebar link */
  .sidebar-link {
    transition: all 200ms;
  }
  .sidebar-link:hover, .sidebar-link.active {
    background: rgba(249, 115, 22, 0.1);
    color: #f97316;
  }
  .sidebar-link.active {
    border-right: 2px solid #f97316;
  }

  /* Background grid */
  .bg-grid {
    background-size: 40px 40px;
    background-image:
      linear-gradient(to right, rgba(255,255,255,0.02) 1px, transparent 1px),
      linear-gradient(to bottom, rgba(255,255,255,0.02) 1px, transparent 1px);
  }

  /* Toast */
  .toast {
    transform: translateY(20px);
    opacity: 0;
    transition: all 300ms ease;
  }
  .toast.show {
    transform: translateY(0);
    opacity: 1;
  }

  /* Trailer modal */
  .modal-backdrop {
    opacity: 0;
    transition: opacity 300ms;
    pointer-events: none;
  }
  .modal-backdrop.open {
    opacity: 1;
    pointer-events: auto;
  }
  .modal-content {
    transform: scale(0.95) translateY(10px);
    transition: transform 300ms ease;
  }
  .modal-backdrop.open .modal-content {
    transform: scale(1) translateY(0);
  }
</style>
</head>
<body class="text-white min-h-screen overflow-x-hidden">

<!-- Sketch decorative top section -->
<div class="relative w-full h-32 md:h-44 flex-shrink-0 overflow-hidden">
  <svg class="absolute inset-0 w-full h-full" viewBox="0 0 1200 180" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path class="sketch-line" d="M100 90 C200 20, 400 160, 600 80 S900 140, 1100 60" stroke="rgba(249,115,22,0.15)" stroke-width="1.5" stroke-linecap="round"/>
    <path class="sketch-line sketch-line-delay" d="M50 130 C250 60, 450 150, 650 90 S950 120, 1150 100" stroke="rgba(249,115,22,0.1)" stroke-width="1" stroke-linecap="round"/>
    <path class="sketch-line sketch-line-delay2" d="M150 50 C300 100, 500 30, 700 110 S1000 40, 1050 130" stroke="rgba(255,255,255,0.06)" stroke-width="1" stroke-linecap="round"/>
    <circle class="sketch-line" cx="300" cy="70" r="15" stroke="rgba(249,115,22,0.1)" stroke-width="1" fill="none"/>
    <circle class="sketch-line sketch-line-delay" cx="800" cy="120" r="10" stroke="rgba(249,115,22,0.08)" stroke-width="1" fill="none"/>
    <rect class="sketch-line sketch-line-delay2" x="550" y="40" width="20" height="20" rx="3" stroke="rgba(255,255,255,0.05)" stroke-width="1" fill="none" transform="rotate(15 560 50)"/>
  </svg>
  <div class="absolute bottom-0 left-0 right-0 h-16 bg-gradient-to-b from-transparent to-[#050505]"></div>
</div>

<!-- Main Layout -->
<div class="flex min-h-[calc(100vh-11rem)] relative z-10">

  <!-- Sidebar -->
  <aside class="hidden md:flex flex-col w-60 lg:w-64 border-r border-white/5 bg-[#0A0A0A] flex-shrink-0">
    <!-- Logo -->
    <div class="px-6 py-6 border-b border-white/5">
      <div class="flex items-center gap-2.5">
        <div class="w-9 h-9 rounded-xl bg-gradient-to-br from-orange-500 to-red-500 flex items-center justify-center">
          <iconify-icon icon="lucide:play" width="16" class="text-white ml-0.5"></iconify-icon>
        </div>
        <span class="text-xl font-semibold tracking-tighter">ANIMIX</span>
      </div>
    </div>

    <!-- Nav -->
    <nav class="flex-1 py-4 px-3 space-y-1 custom-scroll overflow-y-auto">
      <a href="#" class="sidebar-link active flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium">
        <iconify-icon icon="lucide:home" width="18"></iconify-icon>
        Home
      </a>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:heart" width="18"></iconify-icon>
        Favorites
      </a>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:bookmark" width="18"></iconify-icon>
        Watchlist
      </a>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:clock" width="18"></iconify-icon>
        Continue Watching
      </a>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:download" width="18"></iconify-icon>
        Downloads
      </a>

      <div class="pt-4 pb-2 px-3">
        <span class="text-[10px] uppercase tracking-widest text-neutral-600 font-medium">Browse</span>
      </div>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:film" width="18"></iconify-icon>
        Movies
      </a>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:tv" width="18"></iconify-icon>
        Series
      </a>
      <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
        <iconify-icon icon="lucide:sparkles" width="18"></iconify-icon>
        Originals
      </a>
    </nav>

    <!-- User -->
    <div class="p-4 border-t border-white/5">
      <div class="flex items-center gap-3">
        <div class="w-8 h-8 rounded-full bg-gradient-to-br from-orange-500/30 to-purple-500/30 flex items-center justify-center text-xs font-medium">A</div>
        <div class="flex-1 min-w-0">
          <p class="text-sm font-medium truncate">Alex Chen</p>
          <p class="text-[11px] text-neutral-500">Premium</p>
        </div>
        <iconify-icon icon="lucide:settings" width="16" class="text-neutral-500 cursor-pointer hover:text-neutral-300 transition-colors"></iconify-icon>
      </div>
    </div>
  </aside>

  <!-- Mobile Header -->
  <div class="md:hidden fixed top-0 left-0 right-0 z-50 glass-panel border-b border-white/5 px-4 py-3 flex items-center justify-between">
    <div class="flex items-center gap-2">
      <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-orange-500 to-red-500 flex items-center justify-center">
        <iconify-icon icon="lucide:play" width="14" class="text-white ml-0.5"></iconify-icon>
      </div>
      <span class="text-lg font-semibold tracking-tighter">ANIMIX</span>
    </div>
    <div class="flex items-center gap-3">
      <button class="text-neutral-400 hover:text-white transition-colors">
        <iconify-icon icon="lucide:search" width="20"></iconify-icon>
      </button>
      <button id="mobileMenuBtn" class="text-neutral-400 hover:text-white transition-colors">
        <iconify-icon icon="lucide:menu" width="20"></iconify-icon>
      </button>
    </div>
  </div>

  <!-- Mobile Menu Drawer -->
  <div id="mobileMenu" class="fixed inset-0 z-[60] pointer-events-none">
    <div id="mobileMenuOverlay" class="absolute inset-0 bg-black/60 opacity-0 transition-opacity duration-300"></div>
    <div id="mobileMenuPanel" class="absolute left-0 top-0 bottom-0 w-64 bg-[#0A0A0A] border-r border-white/5 transform -translate-x-full transition-transform duration-300">
      <div class="px-5 py-5 border-b border-white/5 flex items-center justify-between">
        <span class="text-lg font-semibold tracking-tighter">ANIMIX</span>
        <button id="mobileMenuClose" class="text-neutral-400"><iconify-icon icon="lucide:x" width="20"></iconify-icon></button>
      </div>
      <nav class="py-4 px-3 space-y-1">
        <a href="#" class="sidebar-link active flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium">
          <iconify-icon icon="lucide:home" width="18"></iconify-icon> Home
        </a>
        <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
          <iconify-icon icon="lucide:heart" width="18"></iconify-icon> Favorites
        </a>
        <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
          <iconify-icon icon="lucide:bookmark" width="18"></iconify-icon> Watchlist
        </a>
        <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
          <iconify-icon icon="lucide:film" width="18"></iconify-icon> Movies
        </a>
        <a href="#" class="sidebar-link flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium text-neutral-400">
          <iconify-icon icon="lucide:tv" width="18"></iconify-icon> Series
        </a>
      </nav>
    </div>
  </div>

  <!-- Main Content -->
  <main class="flex-1 overflow-y-auto custom-scroll bg-grid" style="padding-top: 0;">

    <!-- Featured Hero -->
    <div class="relative">
      <div class="relative h-[340px] md:h-[420px] lg:h-[460px] overflow-hidden">
        <!-- Background Image -->
        <img src="https://picsum.photos/seed/howtotraindragon/1400/600.jpg" alt="How to Train Your Dragon"
          class="absolute inset-0 w-full h-full object-cover opacity-50">
        <!-- Gradient overlays -->
        <div class="absolute inset-0 bg-gradient-to-t from-[#050505] via-[#050505]/60 to-transparent"></div>
        <div class="absolute inset-0 bg-gradient-to-r from-[#050505]/80 via-transparent to-transparent"></div>

        <!-- Content -->
        <div class="absolute bottom-0 left-0 right-0 p-6 md:p-10 lg:p-12">
          <div class="max-w-2xl">
            <!-- Tags -->
            <div class="flex items-center gap-2 mb-4">
              <span class="px-2.5 py-1 rounded-md bg-orange-500/20 text-orange-400 text-[11px] font-medium uppercase tracking-wide">Animation</span>
              <span class="px-2.5 py-1 rounded-md bg-white/5 text-neutral-400 text-[11px] font-medium uppercase tracking-wide">Adventure</span>
              <span class="px-2.5 py-1 rounded-md bg-white/5 text-neutral-400 text-[11px] font-medium uppercase tracking-wide">2025</span>
            </div>

            <h1 class="text-3xl md:text-5xl font-medium tracking-tight leading-[1.1] mb-3">
              How to Train Your Dragon
            </h1>
            <p class="text-neutral-400 text-sm md:text-base leading-relaxed mb-6 max-w-lg">
              A young Viking befriends a fearsome Night Fury dragon, forging an unbreakable bond that changes their world forever.
            </p>

            <div class="flex items-center gap-3">
              <button id="playTrailerBtn"
                class="flex items-center gap-2.5 bg-white text-black px-6 py-3 rounded-xl text-sm font-medium hover:bg-neutral-200 transition-colors">
                <iconify-icon icon="lucide:play" width="18" class="ml-0"></iconify-icon>
                Play Trailer
              </button>
              <button id="addWatchlistBtn"
                class="flex items-center gap-2 glass-panel px-5 py-3 rounded-xl text-sm font-medium text-neutral-300 hover:text-white hover:border-white/20 transition-all">
                <iconify-icon icon="lucide:plus" width="18"></iconify-icon>
                Watchlist
              </button>
              <button class="w-11 h-11 rounded-xl glass-panel flex items-center justify-center text-neutral-400 hover:text-white hover:border-white/20 transition-all">
                <iconify-icon icon="lucide:volume-2" width="18"></iconify-icon>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Content Area -->
    <div class="px-6 md:px-10 lg:px-12 pb-16">

      <!-- Categories -->
      <div class="flex items-center gap-2 mb-8 overflow-x-auto custom-scroll pb-2 -mx-1 px-1">
        <button class="cat-pill px-4 py-2 rounded-full text-xs font-medium border border-white/10 text-neutral-400 hover:text-white hover:border-white/20 transition-all whitespace-nowrap" data-cat="trending">
          <iconify-icon icon="lucide:trending-up" width="13" class="mr-1"></iconify-icon>Trending
        </button>
        <button class="cat-pill px-4 py-2 rounded-full text-xs font-medium border border-white/10 text-neutral-400 hover:text-white hover:border-white/20 transition-all whitespace-nowrap" data-cat="adventure">
          <iconify-icon icon="lucide:compass" width="13" class="mr-1"></iconify-icon>Adventure
        </button>
        <button class="cat-pill active px-4 py-2 rounded-full text-xs font-medium border border-white/10 whitespace-nowrap" data-cat="action">
          <iconify-icon icon="lucide:flame" width="13" class="mr-1"></iconify-icon>Action
        </button>
        <button class="cat-pill px-4 py-2 rounded-full text-xs font-medium border border-white/10 text-neutral-400 hover:text-white hover:border-white/20 transition-all whitespace-nowrap" data-cat="comedy">
          <iconify-icon icon="lucide:laugh" width="13" class="mr-1"></iconify-icon>Comedy
        </button>
        <button class="cat-pill px-4 py-2 rounded-full text-xs font-medium border border-white/10 text-neutral-400 hover:text-white hover:border-white/20 transition-all whitespace-nowrap" data-cat="fantasy">
          <iconify-icon icon="lucide:wand-2" width="13" class="mr-1"></iconify-icon>Fantasy
        </button>
        <button class="cat-pill px-4 py-2 rounded-full text-xs font-medium border border-white/10 text-neutral-400 hover:text-white hover:border-white/20 transition-all whitespace-nowrap" data-cat="sci-fi">
          <iconify-icon icon="lucide:rocket" width="13" class="mr-1"></iconify-icon>Sci-Fi
        </button>
        <button class="cat-pill px-4 py-2 rounded-full text-xs font-medium border border-white/10 text-neutral-400 hover:text-white hover:border-white/20 transition-all whitespace-nowrap" data-cat="family">
          <iconify-icon icon="lucide:users" width="13" class="mr-1"></iconify-icon>Family
        </button>
      </div>

      <!-- Recommended Section -->
      <section class="mb-12">
        <div class="flex items-center justify-between mb-5">
          <h2 class="text-lg font-medium tracking-tight">Recommended for You</h2>
          <button class="text-xs text-neutral-500 hover:text-orange-400 transition-colors font-medium">See All →</button>
        </div>

        <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4 md:gap-5">

          <!-- Card 1: Lilo & Stitch -->
          <div class="movie-card cursor-pointer group" data-title="Lilo & Stitch">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/lilostitch2025/400/600.jpg" alt="Lilo & Stitch"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <!-- Rating badge -->
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">7.2</span>
              </div>
              <!-- Play button -->
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
              <!-- Bottom info -->
              <div class="absolute bottom-0 left-0 right-0 p-3 opacity-0 card-overlay transition-opacity duration-300">
                <span class="text-[10px] uppercase tracking-wider text-orange-400 font-medium">Animation</span>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Lilo & Stitch</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 1h 52m</p>
          </div>

          <!-- Card 2: Mission: Impossible -->
          <div class="movie-card cursor-pointer group" data-title="Mission: Impossible">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/missionimp2025/400/600.jpg" alt="Mission: Impossible"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">8.2</span>
              </div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
              <div class="absolute bottom-0 left-0 right-0 p-3 opacity-0 card-overlay transition-opacity duration-300">
                <span class="text-[10px] uppercase tracking-wider text-orange-400 font-medium">Action</span>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Mission: Impossible</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 2h 18m</p>
          </div>

          <!-- Card 3: Spider-Verse -->
          <div class="movie-card cursor-pointer group" data-title="Spider-Verse Beyond">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/spiderverse3/400/600.jpg" alt="Spider-Verse Beyond"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">8.8</span>
              </div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
              <div class="absolute bottom-0 left-0 right-0 p-3 opacity-0 card-overlay transition-opacity duration-300">
                <span class="text-[10px] uppercase tracking-wider text-orange-400 font-medium">Animation</span>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Spider-Verse Beyond</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 2h 05m</p>
          </div>

          <!-- Card 4: Gladiator II -->
          <div class="movie-card cursor-pointer group" data-title="Gladiator II">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/gladiator2/400/600.jpg" alt="Gladiator II"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">7.6</span>
              </div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
              <div class="absolute bottom-0 left-0 right-0 p-3 opacity-0 card-overlay transition-opacity duration-300">
                <span class="text-[10px] uppercase tracking-wider text-orange-400 font-medium">Action</span>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Gladiator II</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 2h 30m</p>
          </div>

          <!-- Card 5: Moana 2 -->
          <div class="movie-card cursor-pointer group hidden sm:block" data-title="Moana 2">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/moana2ocean/400/600.jpg" alt="Moana 2"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">7.9</span>
              </div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
              <div class="absolute bottom-0 left-0 right-0 p-3 opacity-0 card-overlay transition-opacity duration-300">
                <span class="text-[10px] uppercase tracking-wider text-orange-400 font-medium">Adventure</span>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Moana 2</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 1h 45m</p>
          </div>

        </div>
      </section>

      <!-- Trending Now -->
      <section class="mb-12">
        <div class="flex items-center justify-between mb-5">
          <div class="flex items-center gap-2">
            <iconify-icon icon="lucide:trending-up" width="18" class="text-orange-400"></iconify-icon>
            <h2 class="text-lg font-medium tracking-tight">Trending Now</h2>
          </div>
          <button class="text-xs text-neutral-500 hover:text-orange-400 transition-colors font-medium">See All →</button>
        </div>

        <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4 md:gap-5">

          <div class="movie-card cursor-pointer group">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/thunderbolts25/400/600.jpg" alt="Thunderbolts"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">7.4</span>
              </div>
              <div class="absolute top-2.5 left-2.5 w-6 h-6 rounded-md bg-orange-500 flex items-center justify-center text-[11px] font-bold">1</div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Thunderbolts</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 2h 10m</p>
          </div>

          <div class="movie-card cursor-pointer group">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/zootopia2city/400/600.jpg" alt="Zootopia 2"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">7.8</span>
              </div>
              <div class="absolute top-2.5 left-2.5 w-6 h-6 rounded-md bg-orange-500/80 flex items-center justify-center text-[11px] font-bold">2</div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Zootopia 2</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 1h 48m</p>
          </div>

          <div class="movie-card cursor-pointer group">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/avengersecret/400/600.jpg" alt="Avengers: Secret Wars"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">9.1</span>
              </div>
              <div class="absolute top-2.5 left-2.5 w-6 h-6 rounded-md bg-orange-500/60 flex items-center justify-center text-[11px] font-bold">3</div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Avengers: Secret Wars</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 2h 45m</p>
          </div>

          <div class="movie-card cursor-pointer group hidden sm:block">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/insideout3/400/600.jpg" alt="Inside Out 3"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">8.0</span>
              </div>
              <div class="absolute top-2.5 left-2.5 w-6 h-6 rounded-md bg-orange-500/40 flex items-center justify-center text-[11px] font-bold">4</div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Inside Out 3</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 1h 55m</p>
          </div>

          <div class="movie-card cursor-pointer group hidden xl:block">
            <div class="relative aspect-[2/3] rounded-xl overflow-hidden bg-neutral-900 mb-3">
              <img src="https://picsum.photos/seed/jurassicworld4/400/600.jpg" alt="Jurassic World 4"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
              <div class="card-overlay absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 transition-opacity duration-300"></div>
              <div class="absolute top-2.5 right-2.5 glass-panel rounded-lg px-2 py-1 flex items-center gap-1">
                <iconify-icon icon="lucide:star" width="11" class="text-orange-400"></iconify-icon>
                <span class="text-[11px] font-medium">6.9</span>
              </div>
              <div class="absolute top-2.5 left-2.5 w-6 h-6 rounded-md bg-orange-500/30 flex items-center justify-center text-[11px] font-bold">5</div>
              <div class="card-play absolute inset-0 flex items-center justify-center opacity-0 transform scale-75 transition-all duration-300">
                <div class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center border border-white/30">
                  <iconify-icon icon="lucide:play" width="20" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <h3 class="text-sm font-medium tracking-tight truncate group-hover:text-orange-400 transition-colors">Jurassic World 4</h3>
            <p class="text-[11px] text-neutral-500 mt-0.5">2025 · 2h 15m</p>
          </div>

        </div>
      </section>

      <!-- Continue Watching -->
      <section>
        <div class="flex items-center justify-between mb-5">
          <div class="flex items-center gap-2">
            <iconify-icon icon="lucide:clock" width="18" class="text-orange-400"></iconify-icon>
            <h2 class="text-lg font-medium tracking-tight">Continue Watching</h2>
          </div>
        </div>

        <div class="space-y-3">
          <!-- Item 1 -->
          <div class="flex items-center gap-4 p-3 rounded-xl hover:bg-white/[0.03] transition-colors cursor-pointer group">
            <div class="relative w-32 md:w-40 aspect-video rounded-lg overflow-hidden bg-neutral-900 flex-shrink-0">
              <img src="https://picsum.photos/seed/dragon3hidden/320/180.jpg" alt=""
                class="w-full h-full object-cover opacity-70 group-hover:opacity-90 transition-opacity">
              <div class="absolute inset-0 flex items-center justify-center">
                <div class="w-9 h-9 rounded-full bg-black/50 backdrop-blur-sm flex items-center justify-center border border-white/20 group-hover:scale-110 transition-transform">
                  <iconify-icon icon="lucide:play" width="14" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <div class="flex-1 min-w-0">
              <h4 class="text-sm font-medium truncate group-hover:text-orange-400 transition-colors">How to Train Your Dragon: The Hidden World</h4>
              <p class="text-[11px] text-neutral-500 mt-1">S1 E3 · 18 min left</p>
              <div class="w-full h-1 bg-white/5 rounded-full mt-2 overflow-hidden">
                <div class="h-full bg-orange-500 rounded-full" style="width: 65%;"></div>
              </div>
            </div>
            <span class="text-[11px] text-neutral-600 flex-shrink-0 hidden md:block">65%</span>
          </div>

          <!-- Item 2 -->
          <div class="flex items-center gap-4 p-3 rounded-xl hover:bg-white/[0.03] transition-colors cursor-pointer group">
            <div class="relative w-32 md:w-40 aspect-video rounded-lg overflow-hidden bg-neutral-900 flex-shrink-0">
              <img src="https://picsum.photos/seed/arcane2ep5/320/180.jpg" alt=""
                class="w-full h-full object-cover opacity-70 group-hover:opacity-90 transition-opacity">
              <div class="absolute inset-0 flex items-center justify-center">
                <div class="w-9 h-9 rounded-full bg-black/50 backdrop-blur-sm flex items-center justify-center border border-white/20 group-hover:scale-110 transition-transform">
                  <iconify-icon icon="lucide:play" width="14" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <div class="flex-1 min-w-0">
              <h4 class="text-sm font-medium truncate group-hover:text-orange-400 transition-colors">Arcane: Season 2</h4>
              <p class="text-[11px] text-neutral-500 mt-1">S2 E5 · 32 min left</p>
              <div class="w-full h-1 bg-white/5 rounded-full mt-2 overflow-hidden">
                <div class="h-full bg-orange-500 rounded-full" style="width: 40%;"></div>
              </div>
            </div>
            <span class="text-[11px] text-neutral-600 flex-shrink-0 hidden md:block">40%</span>
          </div>

          <!-- Item 3 -->
          <div class="flex items-center gap-4 p-3 rounded-xl hover:bg-white/[0.03] transition-colors cursor-pointer group">
            <div class="relative w-32 md:w-40 aspect-video rounded-lg overflow-hidden bg-neutral-900 flex-shrink-0">
              <img src="https://picsum.photos/seed/madmaxfuriosa/320/180.jpg" alt=""
                class="w-full h-full object-cover opacity-70 group-hover:opacity-90 transition-opacity">
              <div class="absolute inset-0 flex items-center justify-center">
                <div class="w-9 h-9 rounded-full bg-black/50 backdrop-blur-sm flex items-center justify-center border border-white/20 group-hover:scale-110 transition-transform">
                  <iconify-icon icon="lucide:play" width="14" class="text-white ml-0.5"></iconify-icon>
                </div>
              </div>
            </div>
            <div class="flex-1 min-w-0">
              <h4 class="text-sm font-medium truncate group-hover:text-orange-400 transition-colors">Furiosa: A Mad Max Saga</h4>
              <p class="text-[11px] text-neutral-500 mt-1">1h 02 min left</p>
              <div class="w-full h-1 bg-white/5 rounded-full mt-2 overflow-hidden">
                <div class="h-full bg-orange-500 rounded-full" style="width: 25%;"></div>
              </div>
            </div>
            <span class="text-[11px] text-neutral-600 flex-shrink-0 hidden md:block">25%</span>
          </div>
        </div>
      </section>

    </div>
  </main>
</div>

<!-- Trailer Modal -->
<div id="trailerModal" class="modal-backdrop fixed inset-0 z-[70] flex items-center justify-center bg-black/80 backdrop-blur-sm">
  <div class="modal-content w-[90%] max-w-3xl">
    <div class="glass-panel rounded-2xl overflow-hidden">
      <!-- Video placeholder -->
      <div class="relative aspect-video bg-neutral-900 flex items-center justify-center">
        <img src="https://picsum.photos/seed/howtotraindragon/900/500.jpg" alt="" class="absolute inset-0 w-full h-full object-cover opacity-30">
        <div class="relative text-center">
          <div class="w-20 h-20 rounded-full bg-orange-500/20 backdrop-blur-sm flex items-center justify-center border border-orange-500/30 mx-auto mb-4 play-pulse">
            <iconify-icon icon="lucide:play" width="32" class="text-orange-400 ml-1"></iconify-icon>
          </div>
          <p class="text-sm text-neutral-400">Trailer would play here</p>
        </div>
        <button id="closeTrailer" class="absolute top-4 right-4 w-9 h-9 rounded-full bg-black/50 backdrop-blur-sm flex items-center justify-center border border-white/10 hover:bg-white/10 transition-colors">
          <iconify-icon icon="lucide:x" width="16"></iconify-icon>
        </button>
      </div>
      <div class="p-5">
        <h3 class="text-base font-medium">How to Train Your Dragon — Official Trailer</h3>
        <p class="text-xs text-neutral-500 mt-1">2:34 · Premiering 2025</p>
      </div>
    </div>
  </div>
</div>

<!-- Toast -->
<div id="toast" class="toast fixed bottom-6 left-1/2 -translate-x-1/2 z-[80] glass-panel rounded-xl px-5 py-3 flex items-center gap-3">
  <iconify-icon icon="lucide:check-circle" width="18" class="text-green-400"></iconify-icon>
  <span id="toastMsg" class="text-sm font-medium">Added to Watchlist</span>
</div>

<script>
  // Category pills
  document.querySelectorAll('.cat-pill').forEach(pill => {
    pill.addEventListener('click', () => {
      document.querySelectorAll('.cat-pill').forEach(p => p.classList.remove('active'));
      pill.classList.add('active');
    });
  });

  // Trailer modal
  const trailerModal = document.getElementById('trailerModal');
  document.getElementById('playTrailerBtn').addEventListener('click', () => {
    trailerModal.classList.add('open');
  });
  document.getElementById('closeTrailer').addEventListener('click', () => {
    trailerModal.classList.remove('open');
  });
  trailerModal.addEventListener('click', (e) => {
    if (e.target === trailerModal) trailerModal.classList.remove('open');
  });

  // Toast helper
  function showToast(msg) {
    const toast = document.getElementById('toast');
    document.getElementById('toastMsg').textContent = msg;
    toast.classList.add('show');
    setTimeout(() => toast.classList.remove('show'), 2500);
  }

  // Watchlist button
  let inWatchlist = false;
  document.getElementById('addWatchlistBtn').addEventListener('click', () => {
    inWatchlist = !inWatchlist;
    const btn = document.getElementById('addWatchlistBtn');
    if (inWatchlist) {
      btn.innerHTML = '<iconify-icon icon="lucide:check" width="18"></iconify-icon> In Watchlist';
      btn.style.borderColor = 'rgba(249,115,22,0.4)';
      btn.style.color = '#f97316';
      showToast('Added to Watchlist');
    } else {
      btn.innerHTML = '<iconify-icon icon="lucide:plus" width="18"></iconify-icon> Watchlist';
      btn.style.borderColor = '';
      btn.style.color = '';
      showToast('Removed from Watchlist');
    }
  });

  // Movie card click
  document.querySelectorAll('.movie-card').forEach(card => {
    card.addEventListener('click', () => {
      showToast(`Opening "${card.dataset.title}"...`);
    });
  });

  // Mobile menu
  const mobileMenu = document.getElementById('mobileMenu');
  const mobileMenuPanel = document.getElementById('mobileMenuPanel');
  const mobileMenuOverlay = document.getElementById('mobileMenuOverlay');

  function openMobileMenu() {
    mobileMenu.style.pointerEvents = 'auto';
    mobileMenuOverlay.style.opacity = '1';
    mobileMenuPanel.style.transform = 'translateX(0)';
  }
  function closeMobileMenu() {
    mobileMenuOverlay.style.opacity = '0';
    mobileMenuPanel.style.transform = 'translateX(-100%)';
    setTimeout(() => { mobileMenu.style.pointerEvents = 'none'; }, 300);
  }

  document.getElementById('mobileMenuBtn').addEventListener('click', openMobileMenu);
  document.getElementById('mobileMenuClose').addEventListener('click', closeMobileMenu);
  mobileMenuOverlay.addEventListener('click', closeMobileMenu);

  // Keyboard: Escape closes modals
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') {
      trailerModal.classList.remove('open');
      closeMobileMenu();
    }
  });
</script>

</body>
</html>
