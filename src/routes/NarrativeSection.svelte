<script>
  import { onMount } from 'svelte';
  import Joystick from './Joystick.svelte';
  import FilterControls from './FilterControls.svelte';
  import { gameData } from './gameData.js';
  
  let gridRef = $state(null);
  let isVisible = $state(false);
  let filters = $state({});
  let sortBy = $state('releaseYear');
  let games = $state([...gameData]);
  
  // Filter and sort games
  function updateGames() {
    let filteredGames = [...gameData];
    
    // Apply filters
    if (filters.genre) {
      filteredGames = filteredGames.filter(game => game.genre === filters.genre);
    }
    
    if (filters.platform) {
      filteredGames = filteredGames.filter(game => game.platform === filters.platform);
    }
    
    if (filters.difficulty) {
      if (filters.difficulty === 'Fácil (1-3)') {
        filteredGames = filteredGames.filter(game => game.difficulty >= 1 && game.difficulty <= 3);
      } else if (filters.difficulty === 'Medio (4-6)') {
        filteredGames = filteredGames.filter(game => game.difficulty >= 4 && game.difficulty <= 6);
      } else if (filters.difficulty === 'Difícil (7-10)') {
        filteredGames = filteredGames.filter(game => game.difficulty >= 7 && game.difficulty <= 10);
      }
    }
    
    if (filters.decade) {
      const decadeStart = parseInt(filters.decade.substring(0, 4));
      filteredGames = filteredGames.filter(game => {
        const gameDecade = Math.floor(game.releaseYear / 10) * 10;
        return gameDecade === decadeStart;
      });
    }
    
    // Apply sorting
    filteredGames.sort((a, b) => {
      switch (sortBy) {
        case 'releaseYear':
          return a.releaseYear - b.releaseYear;
        case 'rating':
          return b.rating - a.rating;
        case 'popularity':
          return b.popularity - a.popularity;
        case 'difficulty':
          return b.difficulty - a.difficulty;
        case 'name':
          return a.name.localeCompare(b.name);
        default:
          return 0;
      }
    });
    
    games = filteredGames;
  }
  
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
    
    if (gridRef) {
      observer.observe(gridRef);
    }
    
    return () => observer.disconnect();
  });

  function handleFilterChange(newFilters) {
    filters = newFilters;
    updateGames();
  }

  function handleSortChange(newSort) {
    sortBy = newSort;
    updateGames();
  }
</script>

<section id="joystick-grid-section" bind:this={gridRef} class="py-20 px-4 bg-gradient-to-br from-gray-900 via-gray-800 to-gray-900 relative overflow-hidden" style="font-family: 'zrnic rg', sans-serif;">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-10">
    <div class="absolute top-1/4 left-1/4 w-96 h-96 bg-purple-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-cyan-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Section header -->
    <div class="text-center mb-8">
      <h2 class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-6" style="font-family: 'zrnic rg', sans-serif;">
        <span class="bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent">
          UNIVERSO INTERACTIVO
        </span>
      </h2>
      
      <!-- Enhanced Stats -->
      <div class="flex flex-wrap justify-center gap-8 mb-12">
        <div class="text-center">
          <div class="text-3xl font-bold text-cyan-400" style="font-family: 'zrnic rg', sans-serif;">{games.length}</div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Juegos Mostrados</div>
        </div>
        <div class="text-center">
          <div class="text-3xl font-bold text-purple-400" style="font-family: 'zrnic rg', sans-serif;">
            {new Set(games.map(g => g.platform)).size}
          </div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Plataformas</div>
        </div>
        <div class="text-center">
          <div class="text-3xl font-bold text-pink-400" style="font-family: 'zrnic rg', sans-serif;">
            {new Set(games.map(g => g.genre)).size}
          </div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Géneros</div>
        </div>
        <div class="text-center">
          <div class="text-3xl font-bold text-yellow-400" style="font-family: 'zrnic rg', sans-serif;">
            {Math.round(games.reduce((sum, game) => sum + game.rating, 0) / games.length * 10) / 10}
          </div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Valoración Media</div>
        </div>
      </div>
    </div>
    
    <!-- Filter Controls -->
    <FilterControls 
      {filters} 
      {sortBy} 
      filterChange={handleFilterChange}
      sortChange={handleSortChange}
    />
    
    <!-- Controllers grid -->
    {#if games.length > 0}
      <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-6 md:gap-8">
        {#each games as game, index}
          <div 
            class="transform transition-all duration-500"
            class:animate-fade-in-scale={isVisible}
            style="animation-delay: {index * 50}ms;"
          >
            <Joystick {game} />
          </div>
        {/each}
      </div>
    {:else}
      <div class="text-center py-16">
        <div class="text-6xl mb-4">🎮</div>
        <h3 class="text-2xl font-bold text-gray-400 mb-2">No se encontraron juegos</h3>
        <p class="text-gray-500">Intenta ajustar los filtros para ver más resultados</p>
      </div>
    {/if}
    
    <!-- Footer message -->
    <div class="text-center mt-16">
      <p class="text-gray-400 text-lg" style="font-family: 'zrnic rg', sans-serif;">
        Cada controlador cuenta una historia única a través del movimiento, color y brillo
      </p>
    </div>
  </div>
</section>

<style>
  @keyframes fade-in-scale {
    from {
      opacity: 0;
      transform: scale(0.8) translateY(20px);
    }
    to {
      opacity: 1;
      transform: scale(1) translateY(0);
    }
  }
  
  .animate-fade-in-scale {
    animation: fade-in-scale 0.6s ease-out forwards;
  }
</style>
