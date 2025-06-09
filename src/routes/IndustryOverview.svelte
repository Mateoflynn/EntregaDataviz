<script>
  import { onMount } from 'svelte';
  import { industryData } from './gameData.js';
  
  let overviewContainer = $state();
  let isVisible = $state(false);
  
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
    
    if (overviewContainer) {
      observer.observe(overviewContainer);
    }
    
    return () => observer.disconnect();
  });
</script>

<section bind:this={overviewContainer} class="py-20 px-4 bg-gradient-to-br from-black via-gray-900 to-purple-900/20 relative overflow-hidden">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-5">
    <div class="absolute top-1/3 left-1/4 w-96 h-96 bg-cyan-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/3 right-1/4 w-96 h-96 bg-purple-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Section header -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-6xl font-bold bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 bg-clip-text text-transparent mb-6">
        La Industria Global del Gaming
      </h2>
      <p class="text-xl text-gray-300 max-w-4xl mx-auto leading-relaxed">
        Desde sus humildes comienzos en los años 70, los videojuegos se han convertido en la industria de entretenimiento más grande del mundo, 
        superando al cine y la música combinados.
      </p>
    </div>

    <!-- Market Size Evolution -->
    <div class="mb-20">
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Evolución del Mercado Global</h3>
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-8 border border-gray-700">
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
          <!-- Chart area -->
          <div class="relative h-80 bg-gray-900 rounded-lg p-6">
            <!-- Axes -->
            <div class="absolute bottom-6 left-6 right-6 h-px bg-gray-600"></div>
            <div class="absolute bottom-6 left-6 top-6 w-px bg-gray-600"></div>
            
            <!-- Axis labels - properly positioned -->
            <div class="absolute bottom-2 left-1/2 transform -translate-x-1/2 text-gray-400 text-sm z-0">
              Año
            </div>
            <div class="absolute left-2 top-1/2 transform -translate-y-1/2 -rotate-90 text-gray-400 text-sm z-0">
              Ingresos (Billones USD)
            </div>
            
            <!-- Data points and lines -->
            {#each industryData.globalMarketSize as point, index}
              {#if index > 0}
                <div 
                  class="absolute h-0.5 bg-gradient-to-r from-cyan-500 to-purple-500 transition-all duration-1000"
                  style="
                    left: {6 + ((industryData.globalMarketSize[index-1].year - 1980) / (2023 - 1980)) * 82}%; 
                    bottom: {6 + (industryData.globalMarketSize[index-1].value / 184) * 82}%;
                    width: {((point.year - industryData.globalMarketSize[index-1].year) / (2023 - 1980)) * 82}%;
                    transform: rotate({Math.atan2((point.value - industryData.globalMarketSize[index-1].value) / 184 * 82, ((point.year - industryData.globalMarketSize[index-1].year) / (2023 - 1980)) * 82) * 180 / Math.PI}deg);
                    transform-origin: left center;
                    opacity: {isVisible ? 1 : 0};
                  "
                ></div>
              {/if}
              <div 
                class="absolute w-3 h-3 bg-cyan-400 rounded-full transition-all duration-1000 hover:scale-150 cursor-pointer group z-10"
                style="
                  left: {isVisible ? 6 + ((point.year - 1980) / (2023 - 1980)) * 82 : 6}%; 
                  bottom: {isVisible ? 6 + (point.value / 184) * 82 : 6}%;
                  opacity: {isVisible ? 1 : 0};
                "
                title="{point.year}: ${point.value}B"
              >
                <!-- Tooltip -->
                <div class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 opacity-0 group-hover:opacity-100 transition-opacity duration-200 z-20">
                  <div class="bg-gray-800 text-white p-2 rounded text-xs whitespace-nowrap border border-gray-600">
                    <div class="font-semibold">{point.year}</div>
                    <div>${point.value}B USD</div>
                  </div>
                </div>
              </div>
            {/each}
          </div>
          
          <!-- Key insights -->
          <div class="space-y-6">
            <div class="bg-gradient-to-r from-cyan-900/50 to-cyan-800/50 p-6 rounded-lg border border-cyan-700">
              <h4 class="text-xl font-bold text-cyan-400 mb-3">$184 Billones</h4>
              <p class="text-gray-300">Tamaño del mercado global en 2023, creciendo un 920% desde el año 2000</p>
            </div>
            
            <div class="bg-gradient-to-r from-purple-900/50 to-purple-800/50 p-6 rounded-lg border border-purple-700">
              <h4 class="text-xl font-bold text-purple-400 mb-3">3.2 Billones</h4>
              <p class="text-gray-300">Jugadores activos en todo el mundo, casi la mitad de la población global</p>
            </div>
            
            <div class="bg-gradient-to-r from-pink-900/50 to-pink-800/50 p-6 rounded-lg border border-pink-700">
              <h4 class="text-xl font-bold text-pink-400 mb-3">Mobile First</h4>
              <p class="text-gray-300">Los dispositivos móviles representan el 52% de todos los ingresos gaming</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Regional Markets - Fixed to 6 items -->
    <div class="mb-20">
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Mercados Regionales</h3>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {#each industryData.regionalMarkets as region, index}
          <div 
            class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700 transform transition-all duration-1000"
            class:translate-y-0={isVisible}
            class:opacity-100={isVisible}
            class:translate-y-10={!isVisible}
            class:opacity-0={!isVisible}
            style="transition-delay: {index * 100}ms;"
          >
            <div class="flex items-center justify-between mb-4">
              <h4 class="text-lg font-bold text-white">{region.region}</h4>
              <div class="text-2xl font-bold text-cyan-400">{region.marketShare}%</div>
            </div>
            
            <div class="space-y-3">
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Ingresos:</span>
                <span class="text-green-400 font-semibold">${region.revenue}B</span>
              </div>
              
              <div class="flex justify-between items-center">
                <span class="text-gray-300">Crecimiento:</span>
                <span class="text-yellow-400 font-semibold">+{region.growth}%</span>
              </div>
              
              <!-- Market share bar with label -->
              <div class="mt-4">
                <div class="flex justify-between text-xs text-gray-400 mb-1">
                  <span>Participación del mercado global</span>
                  <span>{region.marketShare}%</span>
                </div>
                <div class="w-full bg-gray-700 rounded-full h-2">
                  <div 
                    class="bg-gradient-to-r from-cyan-500 to-purple-500 h-2 rounded-full transition-all duration-1000"
                    style="width: {isVisible ? region.marketShare : 0}%"
                  ></div>
                </div>
              </div>
            </div>
          </div>
        {/each}
        
        <!-- Add sixth item for balance -->
        <div 
          class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 border border-gray-700 transform transition-all duration-1000"
          class:translate-y-0={isVisible}
          class:opacity-100={isVisible}
          class:translate-y-10={!isVisible}
          class:opacity-0={!isVisible}
          style="transition-delay: 500ms;"
        >
          <div class="flex items-center justify-between mb-4">
            <h4 class="text-lg font-bold text-white">Resto del Mundo</h4>
            <div class="text-2xl font-bold text-cyan-400">1%</div>
          </div>
          
          <div class="space-y-3">
            <div class="flex justify-between items-center">
              <span class="text-gray-300">Ingresos:</span>
              <span class="text-green-400 font-semibold">$1.8B</span>
            </div>
            
            <div class="flex justify-between items-center">
              <span class="text-gray-300">Crecimiento:</span>
              <span class="text-yellow-400 font-semibold">+15.2%</span>
            </div>
            
            <!-- Market share bar with label -->
            <div class="mt-4">
              <div class="flex justify-between text-xs text-gray-400 mb-1">
                <span>Participación del mercado global</span>
                <span>1%</span>
              </div>
              <div class="w-full bg-gray-700 rounded-full h-2">
                <div 
                  class="bg-gradient-to-r from-cyan-500 to-purple-500 h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? 1 : 0}%"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Gaming Innovation Waves - Replacing Technology Milestones -->
    <div class="mb-20">
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Olas de Innovación Gaming</h3>
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-8 border border-gray-700">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          
          <!-- Arcade Era -->
          <div 
            class="text-center p-6 bg-gradient-to-br from-yellow-900/50 to-yellow-800/50 rounded-lg border border-yellow-700 transform transition-all duration-1000"
            class:scale-100={isVisible}
            class:opacity-100={isVisible}
            class:scale-90={!isVisible}
            class:opacity-0={!isVisible}
            style="transition-delay: 100ms;"
          >
            <div class="text-4xl mb-4">🕹️</div>
            <h4 class="text-xl font-bold text-yellow-400 mb-2">Era Arcade</h4>
            <div class="text-sm text-gray-300 mb-4">1970s - 1980s</div>
            <p class="text-gray-400 text-sm">Pong, Pac-Man, Space Invaders. Los videojuegos nacen como fenómeno social.</p>
          </div>

          <!-- Console Revolution -->
          <div 
            class="text-center p-6 bg-gradient-to-br from-red-900/50 to-red-800/50 rounded-lg border border-red-700 transform transition-all duration-1000"
            class:scale-100={isVisible}
            class:opacity-100={isVisible}
            class:scale-90={!isVisible}
            class:opacity-0={!isVisible}
            style="transition-delay: 200ms;"
          >
            <div class="text-4xl mb-4">🎮</div>
            <h4 class="text-xl font-bold text-red-400 mb-2">Revolución Consolas</h4>
            <div class="text-sm text-gray-300 mb-4">1980s - 1990s</div>
            <p class="text-gray-400 text-sm">Nintendo, Sega, PlayStation. Gaming llega a los hogares del mundo.</p>
          </div>

          <!-- 3D & Online -->
          <div 
            class="text-center p-6 bg-gradient-to-br from-blue-900/50 to-blue-800/50 rounded-lg border border-blue-700 transform transition-all duration-1000"
            class:scale-100={isVisible}
            class:opacity-100={isVisible}
            class:scale-90={!isVisible}
            class:opacity-0={!isVisible}
            style="transition-delay: 300ms;"
          >
            <div class="text-4xl mb-4">🌐</div>
            <h4 class="text-xl font-bold text-blue-400 mb-2">3D & Online</h4>
            <div class="text-sm text-gray-300 mb-4">1990s - 2000s</div>
            <p class="text-gray-400 text-sm">Doom, Quake, World of Warcraft. Mundos virtuales y multijugador masivo.</p>
          </div>

          <!-- Mobile & Social -->
          <div 
            class="text-center p-6 bg-gradient-to-br from-green-900/50 to-green-800/50 rounded-lg border border-green-700 transform transition-all duration-1000"
            class:scale-100={isVisible}
            class:opacity-100={isVisible}
            class:scale-90={!isVisible}
            class:opacity-0={!isVisible}
            style="transition-delay: 400ms;"
          >
            <div class="text-4xl mb-4">📱</div>
            <h4 class="text-xl font-bold text-green-400 mb-2">Mobile & Social</h4>
            <div class="text-sm text-gray-300 mb-4">2000s - Presente</div>
            <p class="text-gray-400 text-sm">iPhone, Angry Birds, Fortnite. Gaming se vuelve ubicuo y social.</p>
          </div>
        </div>
        
        <!-- Innovation Impact Metrics -->
        <div class="mt-8 grid grid-cols-1 md:grid-cols-3 gap-6">
          <div class="text-center p-4 bg-gray-900/50 rounded-lg">
            <div class="text-2xl font-bold text-cyan-400 mb-2">50+</div>
            <div class="text-gray-300 text-sm">Años de innovación continua</div>
          </div>
          <div class="text-center p-4 bg-gray-900/50 rounded-lg">
            <div class="text-2xl font-bold text-purple-400 mb-2">1000x</div>
            <div class="text-gray-300 text-sm">Crecimiento en poder de procesamiento</div>
          </div>
          <div class="text-center p-4 bg-gray-900/50 rounded-lg">
            <div class="text-2xl font-bold text-pink-400 mb-2">∞</div>
            <div class="text-gray-300 text-sm">Posibilidades creativas</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Demographics Evolution - With proper labels -->
    <div>
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Evolución Demográfica del Gaming</h3>
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-8 border border-gray-700">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
          {#each ['avgAge', 'femalePercent', 'casualPercent'] as metric}
            <div class="text-center">
              <h4 class="text-xl font-bold text-white mb-4">
                {metric === 'avgAge' ? 'Edad Promedio' : metric === 'femalePercent' ? 'Jugadoras Femeninas' : 'Jugadores Casuales'}
              </h4>
              <div class="relative h-40 bg-gray-900 rounded-lg p-4">
                <!-- Year labels -->
                <div class="absolute bottom-1 left-0 right-0 flex justify-between text-xs text-gray-500">
                  {#each industryData.demographicShifts as point, index}
                    {#if index % 2 === 0}
                      <span>{point.year}</span>
                    {/if}
                  {/each}
                </div>
                
                {#each industryData.demographicShifts as point, index}
                  <div 
                    class="absolute w-3 bg-gradient-to-t from-cyan-500 to-purple-500 rounded-t transition-all duration-1000 hover:bg-gradient-to-t hover:from-cyan-400 hover:to-purple-400 cursor-pointer group"
                    style="
                      left: {10 + (index / (industryData.demographicShifts.length - 1)) * 75}%; 
                      bottom: 15px;
                      height: {isVisible ? (point[metric] / (metric === 'avgAge' ? 40 : 100)) * 75 : 0}%;
                      opacity: {isVisible ? 1 : 0};
                    "
                    title="{point.year}: {point[metric]}{metric === 'avgAge' ? ' años' : '%'}"
                  >
                    <!-- Tooltip -->
                    <div class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 opacity-0 group-hover:opacity-100 transition-opacity duration-200 z-10">
                      <div class="bg-gray-800 text-white p-2 rounded text-xs whitespace-nowrap border border-gray-600">
                        <div class="font-semibold">{point.year}</div>
                        <div>{point[metric]}{metric === 'avgAge' ? ' años' : '%'}</div>
                      </div>
                    </div>
                  </div>
                {/each}
              </div>
              <div class="mt-4">
                <span class="text-2xl font-bold text-cyan-400">
                  {industryData.demographicShifts[industryData.demographicShifts.length - 1][metric]}{metric === 'avgAge' ? ' años' : '%'}
                </span>
                <div class="text-gray-400 text-sm">en 2023</div>
              </div>
              
              <!-- Legend -->
              <div class="mt-3 text-xs text-gray-500">
                Cada barra representa un año desde 2000 hasta 2023
              </div>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>
</section>
