<script>
  import { onMount } from 'svelte';
  import { countryMarketData } from './gameData.js';
  
  let { isVisible = false } = $props();
  let selectedCountry = $state(null);
  let hoveredCountry = $state(null);
  
  // Simplified world map paths for major gaming countries
  const countryPaths = {
    "United States": "M158 140L158 180L200 180L200 140Z",
    "China": "M320 120L380 120L380 160L320 160Z",
    "Japan": "M380 130L400 130L400 150L380 150Z",
    "South Korea": "M370 140L385 140L385 155L370 155Z",
    "Germany": "M250 100L270 100L270 120L250 120Z",
    "United Kingdom": "M230 90L245 90L245 105L230 105Z",
    "France": "M240 110L260 110L260 130L240 130Z",
    "Canada": "M140 80L220 80L220 120L140 120Z",
    "Brazil": "M180 200L220 200L220 250L180 250Z",
    "Australia": "M340 220L380 220L380 250L340 250Z"
  };
  
  function handleCountryClick(countryName) {
    selectedCountry = selectedCountry === countryName ? null : countryName;
  }
  
  function handleCountryHover(countryName) {
    hoveredCountry = countryName;
  }
  
  function handleCountryLeave() {
    hoveredCountry = null;
  }
  
  function getCountryColor(countryName) {
    const data = countryMarketData[countryName];
    if (!data) return '#4b5563';
    
    // Color intensity based on revenue
    const intensity = Math.min(data.revenue / 50, 1);
    return `rgba(59, 130, 246, ${0.3 + intensity * 0.7})`;
  }
</script>

<div class="relative w-full h-96 bg-gray-900 rounded-lg border border-gray-700 overflow-hidden">
  <!-- World Map SVG -->
  <svg viewBox="0 0 400 280" class="w-full h-full">
    <!-- Background -->
    <rect width="400" height="280" fill="#1f2937" />
    
    <!-- Continents (simplified shapes) -->
    <g fill="#374151" stroke="#4b5563" stroke-width="1">
      <!-- North America -->
      <path d="M50 60L220 60L220 180L50 180Z" />
      <!-- South America -->
      <path d="M160 180L240 180L240 280L160 280Z" />
      <!-- Europe -->
      <path d="M220 60L300 60L300 140L220 140Z" />
      <!-- Asia -->
      <path d="M300 40L400 40L400 180L300 180Z" />
      <!-- Africa -->
      <path d="M240 140L320 140L320 240L240 240Z" />
      <!-- Australia -->
      <path d="M330 200L400 200L400 280L330 280Z" />
    </g>
    
    <!-- Country overlays -->
    {#each Object.entries(countryPaths) as [countryName, path]}
      {@const data = countryMarketData[countryName]}
      {#if data}
        <path
          d={path}
          fill={getCountryColor(countryName)}
          stroke="#ffffff"
          stroke-width="1"
          class="cursor-pointer transition-all duration-200 hover:stroke-2"
          class:ring-2={selectedCountry === countryName}
          class:ring-cyan-400={selectedCountry === countryName}
          onclick={() => handleCountryClick(countryName)}
          onmouseenter={() => handleCountryHover(countryName)}
          onmouseleave={handleCountryLeave}
          tabindex="0"
          role="button"
          aria-label={`Click to select ${countryName}`}
          onkeydown={(event) => {
            if (event.key === 'Enter' || event.key === ' ') {
              handleCountryClick(countryName);
            }
          }}
        />
      {/if}
    {/each}
    
    <!-- Country labels -->
    {#each Object.keys(countryPaths) as countryName}
      {@const data = countryMarketData[countryName]}
      {#if data}
        <text
          x={countryName === "United States" ? 179 : 
             countryName === "China" ? 350 :
             countryName === "Japan" ? 390 :
             countryName === "Germany" ? 260 :
             countryName === "United Kingdom" ? 237 :
             countryName === "Canada" ? 180 :
             countryName === "Brazil" ? 200 :
             countryName === "Australia" ? 360 : 300}
          y={countryName === "United States" ? 165 : 
             countryName === "China" ? 145 :
             countryName === "Japan" ? 145 :
             countryName === "Germany" ? 115 :
             countryName === "United Kingdom" ? 102 :
             countryName === "Canada" ? 105 :
             countryName === "Brazil" ? 230 :
             countryName === "Australia" ? 240 : 150}
          fill="white"
          font-size="8"
          text-anchor="middle"
          class="pointer-events-none select-none"
        >
          {countryName === "United States" ? "USA" : 
           countryName === "United Kingdom" ? "UK" : 
           countryName.length > 8 ? countryName.substring(0, 8) + "..." : countryName}
        </text>
      {/if}
    {/each}
  </svg>
  
  <!-- Hover tooltip -->
  {#if hoveredCountry && countryMarketData[hoveredCountry]}
    {@const data = countryMarketData[hoveredCountry]}
    <div class="absolute top-4 right-4 bg-gray-800 border border-gray-600 rounded-lg p-4 shadow-xl z-10">
      <h4 class="font-bold text-white mb-2">{hoveredCountry}</h4>
      <div class="space-y-1 text-sm">
        <div class="flex justify-between">
          <span class="text-gray-300">Ingresos:</span>
          <span class="text-green-400">${data.revenue}B</span>
        </div>
        <div class="flex justify-between">
          <span class="text-gray-300">Jugadores:</span>
          <span class="text-blue-400">{data.players}M</span>
        </div>
        <div class="flex justify-between">
          <span class="text-gray-300">Crecimiento:</span>
          <span class="text-yellow-400">+{data.growth}%</span>
        </div>
      </div>
    </div>
  {/if}
  
  <!-- Selected country details -->
  {#if selectedCountry && countryMarketData[selectedCountry]}
    {@const data = countryMarketData[selectedCountry]}
    <div class="absolute bottom-4 left-4 bg-gray-800 border border-gray-600 rounded-lg p-6 shadow-xl z-10 max-w-xs">
      <div class="flex items-center justify-between mb-4">
        <h4 class="font-bold text-white text-lg">{selectedCountry}</h4>
        <button 
          onclick={() => selectedCountry = null}
          class="text-gray-400 hover:text-white"
        >
          ✕
        </button>
      </div>
      
      <div class="space-y-3">
        <div class="bg-gray-900 rounded p-3">
          <div class="text-2xl font-bold text-green-400">${data.revenue}B</div>
          <div class="text-gray-400 text-sm">Ingresos anuales</div>
        </div>
        
        <div class="bg-gray-900 rounded p-3">
          <div class="text-2xl font-bold text-blue-400">{data.players}M</div>
          <div class="text-gray-400 text-sm">Jugadores activos</div>
        </div>
        
        <div class="bg-gray-900 rounded p-3">
          <div class="text-2xl font-bold text-yellow-400">+{data.growth}%</div>
          <div class="text-gray-400 text-sm">Crecimiento anual</div>
        </div>
      </div>
    </div>
  {/if}
  
  <!-- Legend -->
  <div class="absolute top-4 left-4 bg-gray-800 border border-gray-600 rounded-lg p-3">
    <div class="text-white text-sm font-semibold mb-2">Ingresos por País</div>
    <div class="space-y-1 text-xs">
      <div class="flex items-center space-x-2">
        <div class="w-3 h-3 bg-blue-300 rounded"></div>
        <span class="text-gray-300">Bajo (&lt;$5B)</span>
      </div>
      <div class="flex items-center space-x-2">
        <div class="w-3 h-3 bg-blue-500 rounded"></div>
        <span class="text-gray-300">Medio ($5-20B)</span>
      </div>
      <div class="flex items-center space-x-2">
        <div class="w-3 h-3 bg-blue-700 rounded"></div>
        <span class="text-gray-300">Alto (&gt;$20B)</span>
      </div>
    </div>
  </div>
</div>
