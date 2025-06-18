<script>
  import { onMount } from 'svelte';
  import { industryData } from './gameData.js';
  
  let geoContainer = $state();
  let isVisible = $state(false);
  
  const countryData = [
    { country: "China", revenue: 46.0, players: 744, growth: 7.8, color: "from-red-500 to-red-600" },
    { country: "Estados Unidos", revenue: 36.9, players: 191, growth: 2.1, color: "from-blue-500 to-blue-600" },
    { country: "Japón", revenue: 22.1, players: 78, growth: 1.5, color: "from-pink-500 to-pink-600" },
    { country: "Corea del Sur", revenue: 8.5, players: 33, growth: 4.2, color: "from-green-500 to-green-600" },
    { country: "Alemania", revenue: 6.2, players: 49, growth: 3.1, color: "from-yellow-500 to-yellow-600" },
    { country: "Reino Unido", revenue: 5.7, players: 37, growth: 2.8, color: "from-purple-500 to-purple-600" },
    { country: "Francia", revenue: 4.1, players: 38, growth: 2.5, color: "from-cyan-500 to-cyan-600" },
    { country: "Canadá", revenue: 3.6, players: 22, growth: 3.4, color: "from-orange-500 to-orange-600" }
  ];
  
const culturalTrends = [
    {
        region: "Asia-Pacífico",
        trends: [
            "Gaming móvil como prioridad",
            "Mecánicas gacha",
            "Juego social",
            "Dominio de los esports"
        ],
        description: "Lidera la innovación en juegos móviles y mecánicas sociales",
        icon: "🎮"
    },
    {
        region: "Norteamérica",
        trends: [
            "Superproducciones AAA",
            "Plataformas de streaming",
            "Adopción de VR/AR",
            "Modelos de suscripción"
        ],
        description: "Enfoque en producciones de alto presupuesto y nuevas tecnologías",
        icon: "🎬"
    },
    {
        region: "Europa",
        trends: [
            "Desarrollo indie",
            "Juegos de simulación",
            "Gaming educativo",
            "Liderazgo regulatorio"
        ],
        description: "Fuerte escena independiente y enfoque en juegos educativos",
        icon: "🎨"
    },
    {
        region: "Latinoamérica",
        trends: [
            "Crecimiento del free-to-play",
            "Expansión móvil",
            "Contenido local",
            "Construcción de comunidad"
        ],
        description: "Crecimiento explosivo en F2P y contenido culturalmente relevante",
        icon: "🌟"
    }
];
  
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
    
    if (geoContainer) {
      observer.observe(geoContainer);
    }
    
    return () => observer.disconnect();
  });
</script>

<section bind:this={geoContainer} class="py-20 px-4 bg-gradient-to-br from-gray-900 via-blue-900/20 to-gray-900 relative overflow-hidden">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-5">
    <div class="absolute top-1/4 left-1/3 w-96 h-96 bg-blue-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/4 right-1/3 w-96 h-96 bg-green-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Section header -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-6xl font-bold bg-gradient-to-r from-blue-400 via-green-400 to-cyan-400 bg-clip-text text-transparent mb-6">
        Gaming Sin Fronteras
      </h2>
      <p class="text-xl text-gray-300 max-w-4xl mx-auto leading-relaxed">
        Los videojuegos han trascendido barreras culturales y geográficas, creando un lenguaje universal 
        que conecta a jugadores de todos los continentes.
      </p>
    </div>

    <!-- World Map Representation -->
    <div class="mb-20">
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Mercados Globales por País</h3>
      <div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-8 border border-gray-700">
        
        <!-- Simplified world map with data -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
          {#each countryData as country, index}
            <div 
              class="bg-gradient-to-br {country.color} p-6 rounded-lg transform transition-all duration-1000 hover:scale-105"
              class:translate-y-0={isVisible}
              class:opacity-100={isVisible}
              class:translate-y-10={!isVisible}
              class:opacity-0={!isVisible}
              style="transition-delay: {index * 100}ms;"
            >
              <h4 class="text-lg font-bold text-white mb-3">{country.country}</h4>
              <div class="space-y-2 text-white">
                <div class="flex justify-between">
                  <span class="text-sm opacity-90">Ingresos:</span>
                  <span class="font-semibold">${country.revenue}B</span>
                </div>
                <div class="flex justify-between">
                  <span class="text-sm opacity-90">Jugadores:</span>
                  <span class="font-semibold">{country.players}M</span>
                </div>
                <div class="flex justify-between">
                  <span class="text-sm opacity-90">Crecimiento:</span>
                  <span class="font-semibold">+{country.growth}%</span>
                </div>
              </div>
              
              <!-- Revenue bar -->
              <div class="mt-4 w-full bg-black/20 rounded-full h-2">
                <div 
                  class="bg-white h-2 rounded-full transition-all duration-1000"
                  style="width: {isVisible ? (country.revenue / 46) * 100 : 0}%"
                ></div>
              </div>
            </div>
          {/each}
        </div>
        
        <!-- Global insights -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div class="text-center p-6 bg-gray-900/50 rounded-lg">
            <div class="text-3xl font-bold text-blue-400 mb-2">49%</div>
            <div class="text-gray-300">Asia-Pacífico domina el mercado global</div>
          </div>
          <div class="text-center p-6 bg-gray-900/50 rounded-lg">
            <div class="text-3xl font-bold text-green-400 mb-2">2.8B</div>
            <div class="text-gray-300">Jugadores móviles en todo el mundo</div>
          </div>
          <div class="text-center p-6 bg-gray-900/50 rounded-lg">
            <div class="text-3xl font-bold text-cyan-400 mb-2">195</div>
            <div class="text-gray-300">Países con mercados gaming activos</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Cultural Gaming Trends 
    <div class="mb-20">
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Tendencias Culturales por Región</h3>
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        {#each culturalTrends as trend, index}
          <div 
            class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-8 border border-gray-700 transform transition-all duration-1000"
            class:translate-x-0={isVisible}
            class:opacity-100={isVisible}
            class:translate-x-10={!isVisible && index % 2 === 1}
            class:translate-x-[-10]={!isVisible && index % 2 === 0}
            class:opacity-0={!isVisible}
            style="transition-delay: {index * 200}ms;"
          >
            <div class="flex items-center mb-6">
              <div class="text-4xl mr-4">{trend.icon}</div>
              <div>
                <h4 class="text-2xl font-bold text-white">{trend.region}</h4>
                <p class="text-gray-400">{trend.description}</p>
              </div>
            </div>
            
            <div class="space-y-3">
              <h5 class="text-lg font-semibold text-cyan-400 mb-3">Tendencias Clave:</h5>
              {#each trend.trends as trendItem}
                <div class="flex items-center space-x-3 p-3 bg-gray-900/30 rounded-lg">
                  <div class="w-2 h-2 bg-gradient-to-r from-cyan-500 to-blue-500 rounded-full"></div>
                  <span class="text-gray-300">{trendItem}</span>
                </div>
              {/each}
            </div>
          </div>
        {/each}
      </div>
    </div>
    -->
    <!-- Cross-Cultural Gaming Phenomena -->
    <div>
      <h3 class="text-3xl font-bold text-white mb-8 text-center">Fenómenos Gaming Globales</h3>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        
        <!-- Esports -->
        <div 
          class="bg-gradient-to-br from-red-900/50 to-red-800/50 backdrop-blur-sm rounded-lg p-8 border border-red-700 transform transition-all duration-1000"
          class:translate-y-0={isVisible}
          class:opacity-100={isVisible}
          class:translate-y-10={!isVisible}
          class:opacity-0={!isVisible}
          style="transition-delay: 100ms;"
        >
          <div class="text-center mb-6">
            <div class="text-5xl mb-4">🏆</div>
            <h4 class="text-2xl font-bold text-white">Esports</h4>
          </div>
          <div class="space-y-4 text-center">
            <div>
              <div class="text-3xl font-bold text-red-400">495M</div>
              <div class="text-gray-300 text-sm">Espectadores globales</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-red-400">$1.4B</div>
              <div class="text-gray-300 text-sm">Ingresos anuales</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-red-400">40+</div>
              <div class="text-gray-300 text-sm">Países con ligas profesionales</div>
            </div>
          </div>
        </div>

        <!-- Streaming -->
        <div 
          class="bg-gradient-to-br from-purple-900/50 to-purple-800/50 backdrop-blur-sm rounded-lg p-8 border border-purple-700 transform transition-all duration-1000"
          class:translate-y-0={isVisible}
          class:opacity-100={isVisible}
          class:translate-y-10={!isVisible}
          class:opacity-0={!isVisible}
          style="transition-delay: 200ms;"
        >
          <div class="text-center mb-6">
            <div class="text-5xl mb-4">📺</div>
            <h4 class="text-2xl font-bold text-white">Streaming</h4>
          </div>
          <div class="space-y-4 text-center">
            <div>
              <div class="text-3xl font-bold text-purple-400">2.9B</div>
              <div class="text-gray-300 text-sm">Horas vistas mensualmente</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-purple-400">15M</div>
              <div class="text-gray-300 text-sm">Streamers activos</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-purple-400">180+</div>
              <div class="text-gray-300 text-sm">Idiomas de transmisión</div>
            </div>
          </div>
        </div>

        <!-- Mobile Gaming -->
        <div 
          class="bg-gradient-to-br from-green-900/50 to-green-800/50 backdrop-blur-sm rounded-lg p-8 border border-green-700 transform transition-all duration-1000"
          class:translate-y-0={isVisible}
          class:opacity-100={isVisible}
          class:translate-y-10={!isVisible}
          class:opacity-0={!isVisible}
          style="transition-delay: 300ms;"
        >
          <div class="text-center mb-6">
            <div class="text-5xl mb-4">📱</div>
            <h4 class="text-2xl font-bold text-white">Mobile Gaming</h4>
          </div>
          <div class="space-y-4 text-center">
            <div>
              <div class="text-3xl font-bold text-green-400">52%</div>
              <div class="text-gray-300 text-sm">Del mercado global</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-green-400">2.8B</div>
              <div class="text-gray-300 text-sm">Jugadores móviles</div>
            </div>
            <div>
              <div class="text-3xl font-bold text-green-400">95B</div>
              <div class="text-gray-300 text-sm">Ingresos anuales (USD)</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>