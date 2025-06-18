<script>
  import { onMount } from 'svelte';
  import { getEvolutionByDecade } from './gameData.js';
  
  let timelineContainer = $state();
  let isVisible = $state(false);
  
  const evolutionData = getEvolutionByDecade().sort((a, b) => a.decade - b.decade);
  
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
    
    if (timelineContainer) {
      observer.observe(timelineContainer);
    }
    
    return () => observer.disconnect();
  });
</script>

<section bind:this={timelineContainer} class="py-20 px-4 bg-gradient-to-br from-black via-gray-900 to-gray-800 relative overflow-hidden">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-10">
    <div class="absolute top-1/4 left-1/3 w-96 h-96 bg-cyan-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/4 right-1/3 w-96 h-96 bg-purple-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Section header -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 bg-clip-text text-transparent mb-6">
        Evolución Temporal
      </h2>
      <p class="text-xl text-gray-300 max-w-3xl mx-auto">
        Un viaje a través de las décadas: cómo han evolucionado los videojuegos desde los años 80
      </p>
    </div>

    <!-- Timeline -->
    <div class="relative">
      <!-- Timeline line -->
      <div class="absolute left-1/2 transform -translate-x-1/2 w-1 h-full bg-gradient-to-b from-cyan-500 via-purple-500 to-pink-500 rounded-full"></div>
      
      <!-- Timeline items -->
      <div class="space-y-16">
        {#each evolutionData as decade, index}
          <div 
            class="relative flex items-center {index % 2 === 0 ? 'justify-start' : 'justify-end'}"
            class:animate-fade-in-left={isVisible && index % 2 === 0}
            class:animate-fade-in-right={isVisible && index % 2 === 1}
            style="animation-delay: {index * 200}ms;"
          >
            <!-- Timeline dot -->
            <div class="absolute left-1/2 transform -translate-x-1/2 w-6 h-6 bg-gradient-to-r from-cyan-500 to-purple-500 rounded-full border-4 border-gray-900 z-10"></div>
            
            <!-- Content card -->
            <div class="w-5/12 bg-gray-800/70 backdrop-blur-sm rounded-lg p-6 border border-gray-700 {index % 2 === 0 ? 'mr-auto' : 'ml-auto'}">
              <div class="flex items-center justify-between mb-4">
                <h3 class="text-2xl font-bold text-white">{decade.decade}s</h3>
                <div class="text-cyan-400 font-semibold">{decade.gameCount} juegos</div>
              </div>
              
              <div class="space-y-3">
                <div class="flex justify-between items-center">
                  <span class="text-gray-300">Valoración promedio:</span>
                  <div class="flex items-center space-x-2">
                    <div class="w-20 bg-gray-700 rounded-full h-2">
                      <div 
                        class="bg-gradient-to-r from-yellow-500 to-orange-500 h-2 rounded-full transition-all duration-1000"
                        style="width: {isVisible ? (decade.avgRating / 10) * 100 : 0}%"
                      ></div>
                    </div>
                    <span class="text-yellow-400 font-semibold">{decade.avgRating.toFixed(1)}</span>
                  </div>
                </div>
                
                <div class="flex justify-between items-center">
                  <span class="text-gray-300">Dificultad promedio:</span>
                  <div class="flex items-center space-x-2">
                    <div class="w-20 bg-gray-700 rounded-full h-2">
                      <div 
                        class="bg-gradient-to-r from-red-500 to-pink-500 h-2 rounded-full transition-all duration-1000"
                        style="width: {isVisible ? (decade.avgDifficulty / 10) * 100 : 0}%"
                      ></div>
                    </div>
                    <span class="text-red-400 font-semibold">{decade.avgDifficulty.toFixed(1)}</span>
                  </div>
                </div>
                
                <div class="flex justify-between items-center">
                  <span class="text-gray-300">Ventas totales:</span>
                  <span class="text-green-400 font-semibold">{Math.round(decade.totalSales)}M</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-gray-300">Género dominante:</span>
                  <span class="text-purple-400 font-semibold">{decade.topGenre}</span>
                </div>
              </div>
              
              <!-- Featured games from this decade -->
              <div class="mt-4 pt-4 border-t border-gray-600">
                <h4 class="text-sm font-semibold text-gray-300 mb-2">Juegos destacados:</h4>
                <div class="flex flex-wrap gap-1">
                  {#each decade.games.slice(0, 3) as game}
                    <span class="inline-block px-2 py-1 bg-gray-700 rounded text-xs text-gray-300">
                      {game.name}
                    </span>
                  {/each}
                  {#if decade.games.length > 3}
                    <span class="inline-block px-2 py-1 bg-gray-600 rounded text-xs text-gray-400">
                      +{decade.games.length - 3} más
                    </span>
                  {/if}
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>

<style>
  @keyframes fade-in-left {
    from {
      opacity: 0;
      transform: translateX(-50px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }
  
  @keyframes fade-in-right {
    from {
      opacity: 0;
      transform: translateX(50px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }
  
  .animate-fade-in-left {
    animation: fade-in-left 0.8s ease-out forwards;
    opacity: 0;
  }
  
  .animate-fade-in-right {
    animation: fade-in-right 0.8s ease-out forwards;
    opacity: 0;
  }
</style>
