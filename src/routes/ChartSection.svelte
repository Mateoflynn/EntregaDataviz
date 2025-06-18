<script>
  import { onMount } from 'svelte';
  import { getGenreDistribution, getRegionalDistribution, getDifficultyVsPopularity, getMainstreamVsNiche } from './gameData.js';
  
  let chartContainer = $state();
  let isVisible = $state(false);
  
  const genreData = getGenreDistribution();
  const regionalData = getRegionalDistribution();
  const difficultyData = getDifficultyVsPopularity();
  const mainstreamData = getMainstreamVsNiche();
  
  // Genre colors for consistency
  const genreColors = {
    'Acción': '#ef4444',
    'Aventura': '#22c55e',
    'RPG': '#7c3aed',
    'Estrategia': '#06b6d4',
    'Deportes': '#f59e0b',
    'Puzzle': '#ec4899',
    'Carreras': '#3b82f6',
    'Lucha': '#dc2626',
    'Simulación': '#0d9488'
  };

  const regionColors = {
    'North America': '#3b82f6',
    'Asia': '#ef4444',
    'Europe': '#22c55e',
    'Oceania': '#f59e0b'
  };
  
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
        Exploremos los patrones ocultos en nuestros 100 juegos seleccionados a través de visualizaciones interactivas
      </p>
    </div>

    <!-- Charts Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 mb-16">
      
      <!-- Genre Distribution - Circular Chart -->
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700">
        <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
          <div class="w-4 h-4 bg-gradient-to-r from-purple-500 to-pink-500 rounded mr-3"></div>
          Distribución por Género
        </h3>
        
        <!-- Circular visualization -->
        <div class="relative w-80 h-80 mx-auto">
          <svg viewBox="0 0 320 320" class="w-full h-full">
            <!-- Background circle -->
            <circle cx="160" cy="160" r="140" fill="none" stroke="#374151" stroke-width="2"/>
            
            {#each genreData.sort((a, b) => b.count - a.count) as genre, index}
              {@const angle = (index / genreData.length) * 2 * Math.PI}
              {@const x = 160 + Math.cos(angle - Math.PI/2) * 120}
              {@const y = 160 + Math.sin(angle - Math.PI/2) * 120}
              {@const barHeight = (genre.count / Math.max(...genreData.map(d => d.count))) * 60}
              
              <!-- Genre bar -->
              <g class="cursor-pointer hover:opacity-80 transition-opacity duration-300">
                <rect 
                  x={x - 8} 
                  y={y - barHeight/2} 
                  width="16" 
                  height={isVisible ? barHeight : 0}
                  fill={genreColors[genre.genre] || '#6b7280'}
                  rx="8"
                  class="transition-all duration-1000"
                  style="transition-delay: {index * 100}ms;"
                />
                
                <!-- Genre label -->
                <text 
                  x={160 + Math.cos(angle - Math.PI/2) * 145} 
                  y={160 + Math.sin(angle - Math.PI/2) * 145}
                  text-anchor="middle"
                  class="text-xs fill-gray-300 font-semibold"
                  transform="rotate({angle * 180 / Math.PI}, {160 + Math.cos(angle - Math.PI/2) * 145}, {160 + Math.sin(angle - Math.PI/2) * 145})"
                >
                  {genre.genre}
                </text>
                
                <!-- Count -->
                <text 
                  x={x} 
                  y={y + 4}
                  text-anchor="middle"
                  class="text-xs fill-white font-bold"
                >
                  {genre.count}
                </text>
              </g>
            {/each}
            
            <!-- Center label -->
            <text x="160" y="155" text-anchor="middle" class="text-sm fill-gray-300 font-semibold">Total</text>
            <text x="160" y="170" text-anchor="middle" class="text-xl fill-white font-bold">100</text>
          </svg>
        </div>
      </div>

      <!-- Regional Distribution - Globe-style visualization -->
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700">
        <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
          <div class="w-4 h-4 bg-gradient-to-r from-cyan-500 to-blue-500 rounded mr-3"></div>
          Distribución Regional
        </h3>
        
        <div class="relative w-80 h-80 mx-auto">
          <svg viewBox="0 0 320 320" class="w-full h-full">
            <!-- Globe background -->
            <circle cx="160" cy="160" r="120" fill="none" stroke="#374151" stroke-width="2" stroke-dasharray="5,5"/>
            
            {#each regionalData.sort((a, b) => b.count - a.count) as region, index}
              {@const radius = 30 + (region.count / Math.max(...regionalData.map(d => d.count))) * 40}
              {@const positions = [
                {x: 160, y: 100}, // North America - top
                {x: 220, y: 140}, // Asia - right
                {x: 100, y: 140}, // Europe - left
                {x: 200, y: 220}  // Oceania - bottom right
              ]}
              {@const pos = positions[index] || {x: 160, y: 160}}
              
              <g class="cursor-pointer hover:scale-110 transition-transform duration-300">
                <!-- Region circle -->
                <circle 
                  cx={pos.x} 
                  cy={pos.y} 
                  r={isVisible ? radius : 0}
                  fill={regionColors[region.region] || '#6b7280'}
                  opacity="0.8"
                  class="transition-all duration-1000"
                  style="transition-delay: {index * 200}ms;"
                />
                
                <!-- Region label -->
                <text 
                  x={pos.x} 
                  y={pos.y - 5}
                  text-anchor="middle"
                  class="text-xs fill-white font-semibold"
                >
                  {region.region}
                </text>
                
                <!-- Count -->
                <text 
                  x={pos.x} 
                  y={pos.y + 8}
                  text-anchor="middle"
                  class="text-sm fill-white font-bold"
                >
                  {region.count}
                </text>
              </g>
            {/each}
          </svg>
        </div>
        
        <!-- Legend -->
        <div class="mt-4 space-y-2">
          {#each regionalData.sort((a, b) => b.count - a.count) as region}
            <div class="flex items-center justify-between text-sm">
              <div class="flex items-center space-x-2">
                <div class="w-3 h-3 rounded-full" style="background-color: {regionColors[region.region] || '#6b7280'}"></div>
                <span class="text-gray-300">{region.region}</span>
              </div>
              <span class="text-white font-semibold">{region.count} juegos</span>
            </div>
          {/each}
        </div>
      </div>
    </div>

    <!-- Difficulty vs Popularity Scatter Plot - Enhanced -->
    <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700 mb-12">
      <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
        <div class="w-4 h-4 bg-gradient-to-r from-yellow-500 to-orange-500 rounded mr-3"></div>
        Dificultad vs Popularidad
      </h3>
      <div class="relative h-96 bg-gray-900 rounded-lg p-6">
        <!-- Chart axes -->
        <div class="absolute bottom-6 left-6 right-6 h-px bg-gray-600"></div>
        <div class="absolute bottom-6 left-6 top-6 w-px bg-gray-600"></div>
        
        <!-- Axis labels -->
        <div class="absolute bottom-2 left-1/2 transform -translate-x-1/2 text-gray-400 text-sm z-0">
          Dificultad (1-10)
        </div>
        <div class="absolute left-2 top-1/2 transform -translate-y-1/2 -rotate-90 text-gray-400 text-sm z-0">
          Popularidad (%)
        </div>
        
        <!-- Grid lines -->
        {#each [2, 4, 6, 8] as gridX}
          <div class="absolute bottom-6 top-6 w-px bg-gray-700 opacity-30" style="left: {6 + (gridX / 10) * 82}%"></div>
        {/each}
        {#each [75, 80, 85, 90, 95] as gridY}
          <div class="absolute left-6 right-6 h-px bg-gray-700 opacity-30" style="bottom: {6 + ((gridY - 70) / 30) * 82}%"></div>
        {/each}
        
        <!-- Data points with enhanced styling -->
        {#each difficultyData as game}
          <div 
            class="absolute w-4 h-4 rounded-full transition-all duration-1000 hover:scale-150 cursor-pointer group z-10"
            style="
              left: {isVisible ? 6 + ((game.difficulty - 1) / 9) * 82 : 6}%; 
              bottom: {isVisible ? 6 + ((game.popularity - 70) / 30) * 82 : 6}%;
              opacity: {isVisible ? 0.9 : 0};
              background: {game.isMainstream ? 'linear-gradient(45deg, #22c55e, #16a34a)' : 'linear-gradient(45deg, #8b5cf6, #7c3aed)'};
              box-shadow: 0 0 10px {game.isMainstream ? '#22c55e40' : '#8b5cf640'};
            "
            title="{game.name} - Dificultad: {game.difficulty}, Popularidad: {game.popularity}%"
          >
            <!-- Enhanced tooltip -->
            <div class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 opacity-0 group-hover:opacity-100 transition-opacity duration-200 z-20">
              <div class="bg-gray-800 text-white p-3 rounded-lg text-xs whitespace-nowrap border border-gray-600 shadow-xl">
                <div class="font-semibold text-sm mb-1">{game.name}</div>
                <div class="space-y-1">
                  <div>Dificultad: <span class="text-red-400">{game.difficulty}/10</span></div>
                  <div>Popularidad: <span class="text-green-400">{game.popularity}%</span></div>
                  <div>Valoración: <span class="text-yellow-400">{game.rating}/10</span></div>
                  <div>Género: <span class="text-purple-400">{game.genre}</span></div>
                </div>
              </div>
            </div>
          </div>
        {/each}
        
        <!-- Enhanced Legend -->
        <div class="absolute top-6 right-6 bg-gray-800 p-4 rounded-lg border border-gray-600 z-10">
          <div class="text-white text-sm font-semibold mb-3">Categorías</div>
          <div class="space-y-2">
            <div class="flex items-center space-x-2">
              <div class="w-4 h-4 rounded-full bg-gradient-to-r from-green-500 to-green-600"></div>
              <span class="text-gray-300 text-xs">Mainstream ({mainstreamData.mainstream.count})</span>
            </div>
            <div class="flex items-center space-x-2">
              <div class="w-4 h-4 rounded-full bg-gradient-to-r from-purple-500 to-purple-600"></div>
              <span class="text-gray-300 text-xs">Nicho ({mainstreamData.niche.count})</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Mainstream vs Niche Comparison - Enhanced cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      <div class="bg-gradient-to-br from-green-900/30 via-green-800/30 to-green-900/30 backdrop-blur-sm rounded-lg p-8 border border-green-700 relative overflow-hidden">
        <!-- Background pattern -->
        <div class="absolute inset-0 opacity-10">
          <div class="absolute top-0 right-0 w-32 h-32 bg-green-500 rounded-full blur-3xl"></div>
        </div>
        
        <div class="relative z-10">
          <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
            <div class="w-6 h-6 bg-green-500 rounded-full mr-3 flex items-center justify-center">
              <span class="text-xs font-bold text-white">M</span>
            </div>
            Juegos Mainstream
          </h3>
          
          <div class="space-y-6">
            <div class="flex justify-between items-center p-4 bg-gray-900/30 rounded-lg">
              <span class="text-gray-300 font-medium">Cantidad:</span>
              <span class="text-green-400 font-bold text-2xl">{mainstreamData.mainstream.count}</span>
            </div>
            
            <div class="space-y-4">
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Valoración promedio:</span>
                <span class="text-green-400 font-bold">{mainstreamData.mainstream.avgRating.toFixed(1)}/10</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-green-500 to-green-400 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? (mainstreamData.mainstream.avgRating / 10) * 100 : 0}%"
                ></div>
              </div>
              
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Popularidad promedio:</span>
                <span class="text-green-400 font-bold">{mainstreamData.mainstream.avgPopularity.toFixed(0)}%</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-green-500 to-green-400 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? mainstreamData.mainstream.avgPopularity : 0}%"
                ></div>
              </div>
              
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Dificultad promedio:</span>
                <span class="text-green-400 font-bold">{mainstreamData.mainstream.avgDifficulty.toFixed(1)}/10</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-green-500 to-green-400 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? (mainstreamData.mainstream.avgDifficulty / 10) * 100 : 0}%"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="bg-gradient-to-br from-purple-900/30 via-purple-800/30 to-purple-900/30 backdrop-blur-sm rounded-lg p-8 border border-purple-700 relative overflow-hidden">
        <!-- Background pattern -->
        <div class="absolute inset-0 opacity-10">
          <div class="absolute top-0 right-0 w-32 h-32 bg-purple-500 rounded-full blur-3xl"></div>
        </div>
        
        <div class="relative z-10">
          <h3 class="text-2xl font-bold text-white mb-6 flex items-center">
            <div class="w-6 h-6 bg-purple-500 rounded-full mr-3 flex items-center justify-center">
              <span class="text-xs font-bold text-white">N</span>
            </div>
            Juegos de Nicho
          </h3>
          
          <div class="space-y-6">
            <div class="flex justify-between items-center p-4 bg-gray-900/30 rounded-lg">
              <span class="text-gray-300 font-medium">Cantidad:</span>
              <span class="text-purple-400 font-bold text-2xl">{mainstreamData.niche.count}</span>
            </div>
            
            <div class="space-y-4">
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Valoración promedio:</span>
                <span class="text-purple-400 font-bold">{mainstreamData.niche.avgRating.toFixed(1)}/10</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-purple-500 to-purple-400 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? (mainstreamData.niche.avgRating / 10) * 100 : 0}%"
                ></div>
              </div>
              
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Popularidad promedio:</span>
                <span class="text-purple-400 font-bold">{mainstreamData.niche.avgPopularity.toFixed(0)}%</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-purple-500 to-purple-400 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? mainstreamData.niche.avgPopularity : 0}%"
                ></div>
              </div>
              
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Dificultad promedio:</span>
                <span class="text-purple-400 font-bold">{mainstreamData.niche.avgDifficulty.toFixed(1)}/10</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-purple-500 to-purple-400 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? (mainstreamData.niche.avgDifficulty / 10) * 100 : 0}%"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
