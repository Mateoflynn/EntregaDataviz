<script>
  import { onMount } from 'svelte';
  
  // Datos de ejemplo basados en tu archivo - CON ICONOS
  const genreData = [
    { genre: "Acción", count: 34, color: "#ef4444", icon: "⚔️" },
    { genre: "Aventura", count: 21, color: "#22c55e", icon: "🗺️" },
    { genre: "RPG", count: 15, color: "#8b5cf6", icon: "🐉" },
    { genre: "Estrategia", count: 7, color: "#06b6d4", icon: "♟️" },
    { genre: "Puzzle", count: 6, color: "#ec4899", icon: "🧩" },
    { genre: "Simulación", count: 5, color: "#10b981", icon: "🏗️" },
    { genre: "Lucha", count: 5, color: "#dc2626", icon: "👊" },
    { genre: "Carreras", count: 4, color: "#3b82f6", icon: "🏎️" },
    { genre: "Deportes", count: 3, color: "#f59e0b", icon: "⚽" },
  ];

  const regionalData = [
    { region: "North America", count: 44, color: "#3b82f6", shortName: "NA" },
    { region: "Asia", count: 40, color: "#ef4444", shortName: "Asia" },
    { region: "Europe", count: 15, color: "#22c55e", shortName: "EU" },
    { region: "Oceania", count: 1, color: "#f59e0b", shortName: "OCE" },
  ];

  let isVisible = $state(false);
  let hoveredGenre = $state(null);
  let hoveredRegion = $state(null);
  let currentPage = $state(0);

  onMount(() => {
    setTimeout(() => isVisible = true, 300);
  });

  const total = genreData.reduce((sum, item) => sum + item.count, 0);
  const genresPerPage = 6;
  const totalPages = Math.ceil(genreData.length / genresPerPage);
  
  let currentGenres = $derived(genreData
    .sort((a, b) => b.count - a.count)
    .slice(currentPage * genresPerPage, (currentPage + 1) * genresPerPage));

  function nextPage() {
    currentPage = (currentPage + 1) % totalPages;
  }

  function prevPage() {
    currentPage = (currentPage - 1 + totalPages) % totalPages;
  }

  function setCurrentPage(index) {
    currentPage = index;
  }
</script>

<div class="min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900 p-6">
  <div class="max-w-7xl mx-auto">
    <!-- Header -->
    <div class="text-center mb-12">
      <h1 class="text-5xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-4">
        Análisis Visual de Datos
      </h1>
      <p class="text-xl text-gray-300 max-w-3xl mx-auto">
        Visualizaciones modernas e interactivas de nuestra colección de 100 juegos
      </p>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-12">
      <!-- Gráfico de Géneros -->
      <div class="bg-gray-900/50 border border-gray-700 backdrop-blur-sm rounded-lg">
        <div class="flex flex-row items-center justify-between p-6 pb-0">
          <h2 class="text-2xl font-bold text-white flex items-center gap-3">
            <div class="w-3 h-3 bg-gradient-to-r from-purple-500 to-pink-500 rounded-full"></div>
            Distribución por Género
          </h2>
          <div class="flex items-center gap-2">
            <button
              onclick={prevPage}
              disabled={totalPages <= 1}
              class="text-gray-400 hover:text-white hover:bg-gray-800 p-2 rounded disabled:opacity-50"
              aria-label="Previous Page"
            >
              ←
            </button>
            <span class="text-gray-400 text-sm px-2">
              {currentPage + 1} / {totalPages}
            </span>
            <button
              onclick={nextPage}
              disabled={totalPages <= 1}
              class="text-gray-400 hover:text-white hover:bg-gray-800 p-2 rounded disabled:opacity-50"
              aria-label="Next Page"
            >
              →
            </button>
          </div>
        </div>
        
        <div class="p-6">
          <!-- Grid 2x3 de géneros -->
          <div class="grid grid-cols-3 grid-rows-2 gap-4 mb-6 h-80">
            {#each currentGenres as genre, index}
              {@const percentage = (genre.count / total) * 100}
              {@const isHovered = hoveredGenre === genre.genre}
              
              <div
                class="relative group cursor-pointer"
                onmouseenter={() => hoveredGenre = genre.genre}
                onmouseleave={() => hoveredGenre = null}
                role="button"
                tabindex="0"
                aria-label={`Hover to see details about ${genre.genre}`}
              >
                <div
                  class="relative p-3 rounded-xl border-2 transition-all duration-500 overflow-hidden h-full flex flex-col justify-center"
                  style="
                    background-color: {genre.color}10;
                    border-color: {isHovered ? genre.color : genre.color + '30'};
                    transform: {isHovered ? 'scale(1.05) translateY(-3px)' : 'scale(1)'};
                    box-shadow: {isHovered 
                      ? `0 15px 30px ${genre.color}30, 0 0 20px ${genre.color}20`
                      : `0 3px 10px ${genre.color}20`};
                  "
                >
                  <!-- Contenido CENTRADO -->
                  <div class="relative z-10 text-center flex flex-col justify-center h-full">
                    <!-- ICONO DEL GÉNERO -->
                    <div class="text-2xl mb-2 transform transition-transform duration-300 group-hover:scale-110">
                      {genre.icon}
                    </div>

                    <!-- Número principal -->
                    <div
                      class="text-xl font-bold mb-2 transition-colors duration-300"
                      style="color: {isHovered ? genre.color : '#ffffff'}"
                    >
                      {genre.count}
                    </div>

                    <!-- Nombre del género -->
                    <div class="text-gray-200 font-medium text-xs mb-2">{genre.genre}</div>

                    <!-- Barra de progreso -->
                    <div class="w-full h-1.5 bg-gray-800 rounded-full overflow-hidden mb-1">
                      <div
                        class="h-full rounded-full transition-all duration-1000 ease-out"
                        style="
                          width: {isVisible ? percentage : 0}%;
                          background-color: {genre.color};
                          box-shadow: 0 0 8px {genre.color}60;
                        "
                      ></div>
                    </div>

                    <!-- Porcentaje -->
                    <div class="text-gray-400 text-xs">{percentage.toFixed(1)}%</div>
                  </div>
                </div>

                <!-- Tooltip -->
                {#if isHovered}
                  <div class="absolute -top-12 left-1/2 transform -translate-x-1/2 bg-gray-900 text-white px-3 py-1.5 rounded-lg text-xs whitespace-nowrap border border-gray-600 shadow-xl z-20">
                    <div class="font-semibold flex items-center gap-2">
                      <span>{genre.icon}</span>
                      {genre.genre}
                    </div>
                    <div class="text-gray-300">
                      {genre.count} juegos ({percentage.toFixed(1)}%)
                    </div>
                  </div>
                {/if}
              </div>
            {/each}
          </div>

          <!-- Indicadores de página -->
          <div class="flex justify-center gap-2 mb-4">
            {#each Array(totalPages) as _, index}
              <button
                onclick={() => setCurrentPage(index)}
                class="w-2 h-2 rounded-full transition-all duration-300 {index === currentPage ? 'bg-purple-500 w-6' : 'bg-gray-600 hover:bg-gray-500'}"
                aria-label={`Go to page ${index + 1}`}
              ></button>
            {/each}
          </div>
        </div>
      </div>

      <!-- Gráfico Regional - Radar -->
      <div class="bg-gray-900/50 border border-gray-700 backdrop-blur-sm rounded-lg">
        <div class="p-6 pb-0">
          <h2 class="text-2xl font-bold text-white flex items-center gap-3">
            <div class="w-3 h-3 bg-gradient-to-r from-cyan-500 to-blue-500 rounded-full"></div>
            Distribución Regional
          </h2>
        </div>
        
        <div class="p-6">
          <div class="relative w-full h-80 flex items-center justify-center">
            <!-- Círculos concéntricos del radar -->
            <div class="absolute inset-0 flex items-center justify-center">
              {#each [1, 2, 3, 4] as ring}
                <div
                  class="absolute border border-gray-600 rounded-full animate-pulse"
                  style="
                    width: {ring * 60}px;
                    height: {ring * 60}px;
                    animation-delay: {ring * 0.5}s;
                    opacity: 0.3;
                  "
                ></div>
              {/each}
            </div>

            <!-- Líneas del radar -->
            <div class="absolute inset-0 flex items-center justify-center">
              {#each [0, 45, 90, 135, 180, 225, 270, 315] as angle}
                <div
                  class="absolute w-px h-32 bg-gray-600 opacity-30"
                  style="
                    transform: rotate({angle}deg);
                    transform-origin: bottom center;
                  "
                ></div>
              {/each}
            </div>

            <!-- Puntos de datos en el radar -->
            {#each regionalData.sort((a, b) => b.count - a.count) as region, index}
              {@const angle = index * 90 - 45}
              {@const distance = 40 + (region.count / regionalData[0].count) * 80}
              {@const x = Math.cos((angle * Math.PI) / 180) * distance}
              {@const y = Math.sin((angle * Math.PI) / 180) * distance}
              {@const isHovered = hoveredRegion === region.region}
              
              <div
                class="absolute cursor-pointer group"
                style="transform: translate({x}px, {y}px)"
                onmouseenter={() => hoveredRegion = region.region}
                onmouseleave={() => hoveredRegion = null}
                role="button"
                tabindex="0"
                aria-label={`Hover to see details about ${region.region}`}
              >
                <!-- Punto principal -->
                <div
                  class="w-10 h-10 rounded-full flex items-center justify-center text-white font-bold text-sm transition-all duration-500 relative"
                  style="
                    background-color: {region.color};
                    box-shadow: {isHovered
                      ? `0 0 30px ${region.color}, 0 0 60px ${region.color}40`
                      : `0 0 15px ${region.color}60`};
                    transform: {isHovered ? 'scale(1.5)' : 'scale(1)'};
                  "
                >
                  {region.count}
                </div>

                <!-- Etiqueta -->
                <div class="absolute -bottom-8 left-1/2 transform -translate-x-1/2 text-center">
                  <div class="text-white text-xs font-semibold">{region.shortName}</div>
                </div>

                <!-- Tooltip -->
                {#if isHovered}
                  <div class="absolute -top-12 left-1/2 transform -translate-x-1/2 bg-gray-900 text-white px-3 py-2 rounded-lg text-xs whitespace-nowrap border border-gray-600 shadow-xl z-20">
                    <div class="font-semibold">{region.region}</div>
                    <div class="text-gray-300">{region.count} juegos</div>
                  </div>
                {/if}
              </div>
            {/each}

            <!-- Barrido del radar -->
            <div
              class="absolute inset-0 flex items-center justify-center pointer-events-none radar-sweep"
              style="
                background: conic-gradient(from 0deg, transparent 350deg, #00ff0020 360deg);
                border-radius: 50%;
              "
            ></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  @keyframes radar-sweep {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  
  .radar-sweep {
    animation: radar-sweep 4s linear infinite;
  }
</style>
