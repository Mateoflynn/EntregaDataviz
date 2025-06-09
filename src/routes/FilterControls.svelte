<script>
  let { 
    filters = {}, 
    sortBy = 'releaseYear', 
    filterChange, 
    sortChange 
  } = $props();

  const genres = ['Todos', 'Acción', 'Aventura', 'RPG', 'Estrategia', 'Deportes', 'Puzzle', 'Lucha', 'Simulación'];
  const platforms = ['Todas', 'PC', 'PlayStation', 'Nintendo', 'Xbox', 'Arcade', 'Retro'];
  const difficulties = ['Todas', 'Fácil (1-3)', 'Medio (4-6)', 'Difícil (7-10)'];
  const sortOptions = [
    { value: 'releaseYear', label: 'Año de lanzamiento' },
    { value: 'rating', label: 'Valoración' },
    { value: 'popularity', label: 'Popularidad' },
    { value: 'difficulty', label: 'Dificultad' },
    { value: 'name', label: 'Nombre' }
  ];

  function updateFilter(key, value) {
    const newFilters = { ...filters };
    if (value === 'Todos' || value === 'Todas') {
      delete newFilters[key];
    } else {
      newFilters[key] = value;
    }
    filterChange(newFilters);
  }

  function updateSort(value) {
    sortChange(value);
  }
</script>

<div class="bg-gray-800/50 backdrop-blur-sm rounded-lg p-6 mb-8 border border-gray-700">
  <h3 class="text-xl font-bold text-white mb-4 flex items-center">
    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 4a1 1 0 011-1h16a1 1 0 011 1v2.586a1 1 0 01-.293.707l-6.414 6.414a1 1 0 00-.293.707V17l-4 4v-6.586a1 1 0 00-.293-.707L3.293 7.707A1 1 0 013 7V4z"></path>
    </svg>
    Filtros y Ordenamiento
  </h3>
  
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4">
    <!-- Genre Filter -->
    <div>
      <label class="block text-sm font-medium text-gray-300 mb-2" for="genre">Género</label>
      <select 
        id="genre"
        class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 text-white focus:outline-none focus:ring-2 focus:ring-cyan-500"
        value={filters.genre || 'Todos'}
        onchange={(e) => updateFilter('genre', e.target.value)}
      >
        {#each genres as genre}
          <option value={genre}>{genre}</option>
        {/each}
      </select>
    </div>

    <!-- Platform Filter -->
    <div>
      <label class="block text-sm font-medium text-gray-300 mb-2" for="platform">Plataforma</label>
      <select 
        id="platform"
        class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 text-white focus:outline-none focus:ring-2 focus:ring-cyan-500"
        value={filters.platform || 'Todas'}
        onchange={(e) => updateFilter('platform', e.target.value)}
      >
        {#each platforms as platform}
          <option value={platform}>{platform}</option>
        {/each}
      </select>
    </div>

    <!-- Difficulty Filter -->
    <div>
      <label class="block text-sm font-medium text-gray-300 mb-2" for="difficulty">Dificultad</label>
      <select 
        id="difficulty"
        class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 text-white focus:outline-none focus:ring-2 focus:ring-cyan-500"
        value={filters.difficulty || 'Todas'}
        onchange={(e) => updateFilter('difficulty', e.target.value)}
      >
        {#each difficulties as difficulty}
          <option value={difficulty}>{difficulty}</option>
        {/each}
      </select>
    </div>

    <!-- Year Range -->
    <div>
      <label class="block text-sm font-medium text-gray-300 mb-2" for="decade">Década</label>
      <select 
        id="decade"
        class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 text-white focus:outline-none focus:ring-2 focus:ring-cyan-500"
        value={filters.decade || 'Todas'}
        onchange={(e) => updateFilter('decade', e.target.value)}
      >
        <option value="Todas">Todas</option>
        <option value="1980s">1980s</option>
        <option value="1990s">1990s</option>
        <option value="2000s">2000s</option>
        <option value="2010s">2010s</option>
        <option value="2020s">2020s</option>
      </select>
    </div>

    <!-- Sort By -->
    <div>
      <label class="block text-sm font-medium text-gray-300 mb-2" for="sort">Ordenar por</label>
      <select 
        id="sort"
        class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 text-white focus:outline-none focus:ring-2 focus:ring-cyan-500"
        value={sortBy}
        onchange={(e) => updateSort(e.target.value)}
      >
        {#each sortOptions as option}
          <option value={option.value}>{option.label}</option>
        {/each}
      </select>
    </div>
  </div>

  <!-- Active Filters Display -->
  {#if Object.keys(filters).length > 0}
    <div class="mt-4 flex flex-wrap gap-2">
      <span class="text-sm text-gray-300">Filtros activos:</span>
      {#each Object.entries(filters) as [key, value]}
        <span class="inline-flex items-center px-2 py-1 rounded-full text-xs bg-cyan-600 text-white">
          {value}
          <button 
            class="ml-1 hover:bg-cyan-700 rounded-full p-0.5"
            onclick={() => updateFilter(key, 'Todos')}
            aria-label={`Remove filter ${value}`}
          >
            <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </span>
      {/each}
    </div>
  {/if}
</div>
