<script>
  import { onMount } from 'svelte';
  import { getGenreDistribution, getRegionalDistribution, getDifficultyVsPopularity, getMainstreamVsNiche } from './gameData.js';
  
  let chartContainer = $state();
  let isVisible = $state(false);
  
  const genreData = getGenreDistribution();
  const regionalData = getRegionalDistribution();
  const difficultyData = getDifficultyVsPopularity();
  const mainstreamData = getMainstreamVsNiche();
  
  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            isVisible = true;
          }
        });
      },
      { threshold: 0.1 }
    );
    
    if (chartContainer) {
      observer.observe(chartContainer);
    }
    
    return () => observer.disconnect();
  });
</script>

<section bind:this={chartContainer} class="py-20 px-4 bg-gradient-to-br from-gray-800 via-gray-900 to-black relative overflow-hidden">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-5">
    <div class="absolute top-1/3 left-1/4 w-96 h-96 bg-purple-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/3 right-1/4 w-96 h-96 bg-cyan-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Section header -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-6">
        Análisis de Datos de Nuestra Colección
      </h2>
      <p class="text-xl text-gray-300 max-w-3xl mx-auto">
        Exploremos los patrones ocultos en nuestros 30 juegos seleccionados a través de visualizaciones tradicionales
      </p>
    </div>

    <!-- Charts Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 mb-16">
      
      <!-- Genre Distribution Chart -->
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700">
        <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
          <div class="w-4 h-4 bg-gradient-to-r from-purple-500 to-pink-500 rounded mr-3"></div>
          Distribución por Género
        </h3>
        <div class="space-y-4">
          {#each genreData.sort((a, b) => b.count - a.count) as { genre, count }}
            <div class="flex items-center justify-between">
              <span class="text-gray-300 text-sm">{genre}</span>
              <div class="flex items-center space-x-3 flex-1 ml-4">
                <div class="flex-1 bg-gray-700 rounded-full h-2">
                  <div 
                    class="bg-gradient-to-r from-purple-500 to-pink-500 h-2 rounded-full transition-all duration-1000"
                    style="width: {isVisible ? (count / Math.max(...genreData.map(d => d.count))) * 100 : 0}%"
                  ></div>
                </div>
                <span class="text-cyan-400 font-semibold text-sm w-8">{count}</span>
              </div>
            </div>
          {/each}
        </div>
      </div>

      <!-- Regional Distribution - Centered layout for 5 items -->
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700">
        <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
          <div class="w-4 h-4 bg-gradient-to-r from-cyan-500 to-blue-500 rounded mr-3"></div>
          Distribución Regional
        </h3>
        <div class="space-y-4">
          {#each regionalData.sort((a, b) => b.count - a.count) as { region, count }}
            <div class="flex items-center justify-between">
              <span class="text-gray-300 text-sm">{region}</span>
              <div class="flex items-center space-x-3 flex-1 ml-4">
                <div class="flex-1 bg-gray-700 rounded-full h-2">
                  <div 
                    class="bg-gradient-to-r from-cyan-500 to-blue-500 h-2 rounded-full transition-all duration-1000"
                    style="width: {isVisible ? (count / Math.max(...regionalData.map(d => d.count))) * 100 : 0}%"
                  ></div>
                </div>
                <span class="text-cyan-400 font-semibold text-sm w-8">{count}</span>
              </div>
            </div>
          {/each}
          <!-- Add padding items to center the layout -->
          <div class="h-4"></div>
          <div class="h-4"></div>
          <div class="h-4"></div>
        </div>
      </div>
    </div>

    <!-- Difficulty vs Popularity Scatter Plot - Improved scaling -->
    <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700 mb-12">
      <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
        <div class="w-4 h-4 bg-gradient-to-r from-yellow-500 to-orange-500 rounded mr-3"></div>
        Dificultad vs Popularidad
      </h3>
      <div class="relative h-96 bg-gray-900 rounded-lg p-6">
        <!-- Chart axes -->
        <div class="absolute bottom-6 left-6 right-6 h-px bg-gray-600"></div>
        <div class="absolute bottom-6 left-6 top-6 w-px bg-gray-600"></div>
        
        <!-- Axis labels - properly positioned -->
        <div class="absolute bottom-2 left-1/2 transform -translate-x-1/2 text-gray-400 text-sm z-0">
          Dificultad (1-10)
        </div>
        <div class="absolute left-2 top-1/2 transform -translate-y-1/2 -rotate-90 text-gray-400 text-sm z-0">
          Popularidad (%)
        </div>
        
        <!-- Grid lines for better readability -->
        {#each [2, 4, 6, 8] as gridX}
          <div class="absolute bottom-6 top-6 w-px bg-gray-700 opacity-30" style="left: {6 + (gridX / 10) * 82}%"></div>
        {/each}
        {#each [20, 40, 60, 80] as gridY}
          <div class="absolute left-6 right-6 h-px bg-gray-700 opacity-30" style="bottom: {6 + (gridY / 100) * 82}%"></div>
        {/each}
        
        <!-- Data points - improved scaling -->
        {#each difficultyData as game}
          <div 
            class="absolute w-3 h-3 rounded-full transition-all duration-1000 hover:scale-150 cursor-pointer group z-10"
            class:bg-green-500={game.isMainstream}
            class:bg-purple-500={!game.isMainstream}
            style="
              left: {isVisible ? 6 + ((game.difficulty - 1) / 9) * 82 : 6}%; 
              bottom: {isVisible ? 6 + ((game.popularity - 70) / 30) * 82 : 6}%;
              opacity: {isVisible ? 0.8 : 0};
            "
            title="{game.name} - Dificultad: {game.difficulty}, Popularidad: {game.popularity}%"
          >
            <!-- Tooltip -->
            <div class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 opacity-0 group-hover:opacity-100 transition-opacity duration-200 z-20">
              <div class="bg-gray-800 text-white p-2 rounded text-xs whitespace-nowrap border border-gray-600">
                <div class="font-semibold">{game.name}</div>
                <div>Dificultad: {game.difficulty}/10</div>
                <div>Popularidad: {game.popularity}%</div>
                <div>Valoración: {game.rating}/10</div>
              </div>
            </div>
          </div>
        {/each}
        
        <!-- Legend -->
        <div class="absolute top-6 right-6 bg-gray-800 p-3 rounded border border-gray-600 z-10">
          <div class="text-white text-sm font-semibold mb-2">Leyenda</div>
          <div class="flex items-center space-x-2 mb-1">
            <div class="w-3 h-3 bg-green-500 rounded-full"></div>
            <span class="text-gray-300 text-xs">Mainstream</span>
          </div>
          <div class="flex items-center space-x-2">
            <div class="w-3 h-3 bg-purple-500 rounded-full"></div>
            <span class="text-gray-300 text-xs">Nicho</span>
          </div>
        </div>
        
        <!-- Scale note -->
        <div class="absolute top-6 left-6 bg-gray-800 p-2 rounded border border-gray-600 text-xs text-gray-400 z-10">
          Escala optimizada: Popularidad 70-100%
        </div>
      </div>
    </div>

    <!-- Mainstream vs Niche Comparison -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      <div class="bg-gradient-to-br from-green-900/50 to-green-800/50 backdrop-blur-sm rounded-lg p-6 border border-green-700">
        <h3 class="text-2xl font-bold text-white mb-4 flex items-center">
          <div class="w-4 h-4 bg-green-500 rounded mr-3"></div>
          Juegos Mainstream
        </h3>
        <div class="space-y-4">
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Cantidad:</span>
            <span class="text-green-400 font-bold text-xl">{mainstreamData.mainstream.count}</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Valoración promedio:</span>
            <span class="text-green-400 font-bold">{mainstreamData.mainstream.avgRating.toFixed(1)}/10</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Popularidad promedio:</span>
            <span class="text-green-400 font-bold">{mainstreamData.mainstream.avgPopularity.toFixed(0)}%</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Dificultad promedio:</span>
            <span class="text-green-400 font-bold">{mainstreamData.mainstream.avgDifficulty.toFixed(1)}/10</span>
          </div>
        </div>
      </div>

      <div class="bg-gradient-to-br from-purple-900/50 to-purple-800/50 backdrop-blur-sm rounded-lg p-6 border border-purple-700">
        <h3 class="text-2xl font-bold text-white mb-4 flex items-center">
          <div class="w-4 h-4 bg-purple-500 rounded mr-3"></div>
          Juegos de Nicho
        </h3>
        <div class="space-y-4">
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Cantidad:</span>
            <span class="text-purple-400 font-bold text-xl">{mainstreamData.niche.count}</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Valoración promedio:</span>
            <span class="text-purple-400 font-bold">{mainstreamData.niche.avgRating.toFixed(1)}/10</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Popularidad promedio:</span>
            <span class="text-purple-400 font-bold">{mainstreamData.niche.avgPopularity.toFixed(0)}%</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Dificultad promedio:</span>
            <span class="text-purple-400 font-bold">{mainstreamData.niche.avgDifficulty.toFixed(1)}/10</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
