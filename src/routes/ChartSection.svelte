<script>
  import { onMount } from 'svelte';
  
  // Datos de ejemplo basados en tu archivo - CON ICONOS (Svelte syntax)
  const genreData = [
    { genre: "Acción", count: 34, color: "#ef4444", icon: "⚔️" },
    { genre: "Aventura", count: 21, color: "#22c55e", icon: "🗺️" },
    { genre: "RPG", count: 15, color: "#8b5cf6", icon: "🐉" },
    { genre: "Estrategia", count: 7, color: "#06b6d4", icon: "♟️" },
    { genre: "Puzzle", count: 6, color: "#ec4899", icon: "🧩" },
    { genre: "Simulación", count: 5, color: "#10b981", icon: "🏗️" },
    { genre: "Lucha", count: 5, color: "#dc2626", icon: "👊" },
    { genre: "Carreras", count: 4, color: "#3b82f6", icon: "🏎️" },
    { genre: "Deportes", count: 3, color: "#f59e0b", icon: "⚽" }
  ];

  const regionalData = [
    { region: "North America", count: 44, color: "#3b82f6", shortName: "NA" },
    { region: "Asia", count: 40, color: "#ef4444", shortName: "Asia" },
    { region: "Europe", count: 15, color: "#22c55e", shortName: "EU" },
    { region: "Oceania", count: 1, color: "#f59e0b", shortName: "OCE" }
  ];

  let isVisible = $state(false);
  let hoveredGenre = $state(null);
  let hoveredRegion = $state(null);
  let currentPage = $state(0);

  const total = genreData.reduce((sum, item) => sum + item.count, 0);

  // Dividir géneros en páginas de 6 (2x3)
  const genresPerPage = 6;
  const totalPages = Math.ceil(genreData.length / genresPerPage);
  
  // CAMBIO: Usar $derived en lugar de $:
  const currentGenres = $derived(
    genreData
      .sort((a, b) => b.count - a.count)
      .slice(currentPage * genresPerPage, (currentPage + 1) * genresPerPage)
  );

  const nextPage = () => {
    currentPage = (currentPage + 1) % totalPages;
  };

  const prevPage = () => {
    currentPage = (currentPage - 1 + totalPages) % totalPages;
  };

  onMount(() => {
    const timer = setTimeout(() => isVisible = true, 300);
    return () => clearTimeout(timer);
  });
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
      <!-- Gráfico de Géneros - CON ICONOS -->
      <div class="bg-gray-900/50 border border-gray-700 backdrop-blur-sm rounded-lg">
        <div class="flex flex-row items-center justify-between p-6 pb-0">
          <h2 class="text-2xl font-bold text-white flex items-center gap-3">
            <div class="w-3 h-3 bg-gradient-to-r from-purple-500 to-pink-500 rounded-full"></div>
            Distribución por Género
          </h2>
          <div class="flex items-center gap-2">
            <button
              onclick={prevPage}
              class="text-gray-400 hover:text-white hover:bg-gray-800 p-2 rounded transition-colors"
              disabled={totalPages <= 1}
            >
              ←
            </button>
            <span class="text-gray-400 text-sm px-2">
              {currentPage + 1} / {totalPages}
            </span>
            <button
              onclick={nextPage}
              class="text-gray-400 hover:text-white hover:bg-gray-800 p-2 rounded transition-colors"
              disabled={totalPages <= 1}
            >
              →
            </button>
          </div>
        </div>
        <div class="p-6">
          <div class="relative">
            <!-- Grid 2x3 de géneros CON ICONOS -->
            <div class="grid grid-cols-3 grid-rows-2 gap-4 mb-6 h-80">
              {#each currentGenres as genre, index}
                {@const percentage = (genre.count / total) * 100}
                {@const isHovered = hoveredGenre === genre.genre}
                
                <div
                  class="relative group cursor-pointer"
                  onmouseenter={() => hoveredGenre = genre.genre}
                  onmouseleave={() => hoveredGenre = null}
                >
                  <!-- Tarjeta principal -->
                  <div
                    class="relative p-4 rounded-xl border-2 transition-all duration-500 overflow-hidden h-full flex flex-col justify-center"
                    style="
                      background-color: {genre.color}10;
                      border-color: {isHovered ? genre.color : genre.color + '30'};
                      transform: {isHovered ? 'scale(1.05) translateY(-3px)' : 'scale(1)'};
                      box-shadow: {isHovered 
                        ? `0 15px 30px ${genre.color}30, 0 0 20px ${genre.color}20`
                        : `0 3px 10px ${genre.color}20`};
                    "
                  >
                    <!-- Fondo animado -->
                    <div
                      class="absolute inset-0 opacity-0 group-hover:opacity-20 transition-opacity duration-500"
                      style="background: radial-gradient(circle at center, {genre.color}, transparent);"
                    ></div>

                    <!-- Contenido -->
                    <div class="relative z-10 text-center">
                      <!-- ICONO DEL GÉNERO -->
                      <div class="text-3xl mb-3 transform transition-transform duration-300 group-hover:scale-110">
                        {genre.icon}
                      </div>

                      <!-- Número principal -->
                      <div
                        class="text-2xl font-bold mb-2 transition-colors duration-300"
                        style="color: {isHovered ? genre.color : '#ffffff'};"
                      >
                        {genre.count}
                      </div>

                      <!-- Nombre del género -->
                      <div class="text-gray-200 font-medium text-sm mb-3">{genre.genre}</div>

                      <!-- Barra de progreso -->
                      <div class="w-full h-2 bg-gray-800 rounded-full overflow-hidden">
                        <div
                          class="h-full rounded-full transition-all duration-1000 ease-out"
                          style="
                            width: {isVisible ? percentage + '%' : '0%'};
                            background-color: {genre.color};
                            box-shadow: 0 0 8px {genre.color}60;
                          "
                        ></div>
                      </div>

                      <!-- Porcentaje -->
                      <div class="text-gray-400 text-xs mt-2">{percentage.toFixed(1)}%</div>
                    </div>

                    <!-- Efecto de partículas -->
                    {#if isHovered}
                      <div class="absolute inset-0 pointer-events-none">
                        <div
                          class="absolute top-2 right-2 w-2 h-2 rounded-full animate-ping"
                          style="background-color: {genre.color};"
                        ></div>
                        <div
                          class="absolute bottom-2 left-2 w-1 h-1 rounded-full animate-pulse"
                          style="background-color: {genre.color};"
                        ></div>
                      </div>
                    {/if}
                  </div>

                  <!-- Tooltip flotante -->
                  {#if isHovered}
                    <div class="absolute -top-12 left-1/2 transform -translate-x-1/2 bg-gray-900 text-white px-3 py-1.5 rounded-lg text-xs whitespace-nowrap border border-gray-600 shadow-xl z-20">
                      <div class="font-semibold flex items-center gap-2">
                        <span>{genre.icon}</span>
                        {genre.genre}
                      </div>
                      <div class="text-gray-300">
                        {genre.count} juegos ({percentage.toFixed(1)}%)
                      </div>
                      <div class="absolute top-full left-1/2 transform -translate-x-1/2 w-0 h-0 border-l-2 border-r-2 border-t-2 border-transparent border-t-gray-900"></div>
                    </div>
                  {/if}
                </div>
              {/each}
            </div>

            <!-- Indicadores de página -->
            <div class="flex justify-center gap-2 mb-4">
              {#each Array(totalPages) as _, index}
                <button
                  onclick={() => currentPage = index}
                  class="w-2 h-2 rounded-full transition-all duration-300 {index === currentPage ? 'bg-purple-500 w-6' : 'bg-gray-600 hover:bg-gray-500'}"
                ></button>
              {/each}
            </div>

            <!-- Estadísticas mejoradas -->
            <div class="grid grid-cols-3 gap-3">
              <div class="p-3 bg-gradient-to-br from-purple-900/30 to-purple-800/30 rounded-lg border border-purple-700">
                <div class="text-center">
                  <div class="text-xl font-bold text-purple-400 mb-1">{total}</div>
                  <div class="text-xs text-gray-400">Total</div>
                </div>
              </div>
              <div class="p-3 bg-gradient-to-br from-pink-900/30 to-pink-800/30 rounded-lg border border-pink-700">
                <div class="text-center">
                  <div class="text-xl font-bold text-pink-400 mb-1">{genreData.length}</div>
                  <div class="text-xs text-gray-400">Géneros</div>
                </div>
              </div>
              <div class="p-3 bg-gradient-to-br from-cyan-900/30 to-cyan-800/30 rounded-lg border border-cyan-700">
                <div class="text-center">
                  <div class="text-xl font-bold text-cyan-400 mb-1">{genreData[0]?.count || 0}</div>
                  <div class="text-xs text-gray-400">Top</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Gráfico Regional - SIN SÍMBOLOS -->
      <div class="bg-gray-900/50 border border-gray-700 backdrop-blur-sm rounded-lg">
        <div class="p-6 pb-0">
          <h2 class="text-2xl font-bold text-white flex items-center gap-3">
            <div class="w-3 h-3 bg-gradient-to-r from-cyan-500 to-blue-500 rounded-full"></div>
            Distribución Regional
          </h2>
        </div>
        <div class="p-6">
          <div class="space-y-6">
            <!-- Gráfico de barras horizontales limpio -->
            <div class="space-y-4">
              {#each regionalData.sort((a, b) => b.count - a.count) as region, index}
                {@const percentage = (region.count / regionalData.reduce((sum, r) => sum + r.count, 0)) * 100}
                {@const isHovered = hoveredRegion === region.region}
                
                <div
                  class="group cursor-pointer"
                  onmouseenter={() => hoveredRegion = region.region}
                  onmouseleave={() => hoveredRegion = null}
                >
                  <!-- Encabezado de la región -->
                  <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center gap-3">
                      <div class="w-4 h-4 rounded-full" style="background-color: {region.color};"></div>
                      <span class="text-gray-200 font-medium">{region.region}</span>
                    </div>
                    <div class="flex items-center gap-3">
                      <span class="text-gray-400 text-sm">{percentage.toFixed(1)}%</span>
                      <div
                        class="px-3 py-1 rounded-full text-sm font-bold"
                        style="
                          background-color: {region.color}20;
                          color: {region.color};
                          border: 1px solid {region.color}40;
                        "
                      >
                        {region.count}
                      </div>
                    </div>
                  </div>

                  <!-- Barra horizontal -->
                  <div class="w-full h-4 bg-gray-800 rounded-full overflow-hidden">
                    <div
                      class="h-full rounded-full transition-all duration-1000 ease-out relative"
                      style="
                        width: {isVisible ? percentage + '%' : '0%'};
                        background: linear-gradient(90deg, {region.color}, {region.color}80);
                        box-shadow: {isHovered ? `0 0 20px ${region.color}60` : `0 0 10px ${region.color}30`};
                        transform: {isHovered ? 'scaleY(1.2)' : 'scaleY(1)'};
                      "
                    >
                      {#if isHovered}
                        <div class="absolute inset-0 bg-white/20 rounded-full animate-pulse"></div>
                      {/if}
                    </div>
                  </div>
                </div>
              {/each}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>