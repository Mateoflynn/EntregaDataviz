<script>
  import { onMount } from 'svelte';
  
  let conclusionContainer = $state();
  let isVisible = $state(false);
  
  // Key insights from our data
  const keyInsights = [
    {
      icon: "🌍",
      title: "Universalidad",
      description: "Los videojuegos trascienden fronteras geográficas, culturales y generacionales, conectando a 3.2 billones de personas en todo el mundo."
    },
    {
      icon: "🎨",
      title: "Diversidad Creativa",
      description: "Desde puzzles simples hasta mundos épicos, cada género aporta una forma única de narrativa y expresión artística."
    },
    {
      icon: "📈",
      title: "Innovación Constante",
      description: "50 años de evolución tecnológica han transformado píxeles en experiencias inmersivas que rivalizan con cualquier medio."
    },
    {
      icon: "🤝",
      title: "Comunidad Global",
      description: "Los gamers han creado la comunidad más grande y diversa del entretenimiento, donde la colaboración supera a la competencia."
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
    
    if (conclusionContainer) {
      observer.observe(conclusionContainer);
    }
    
    return () => observer.disconnect();
  });
</script>

<section bind:this={conclusionContainer} class="py-20 px-4 bg-gradient-to-br from-gray-900 via-purple-900/30 to-black relative overflow-hidden">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-10">
    <div class="absolute top-1/4 left-1/3 w-96 h-96 bg-purple-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/4 right-1/3 w-96 h-96 bg-cyan-500 rounded-full blur-3xl"></div>
    <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-64 h-64 bg-pink-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-6xl mx-auto relative z-10">
    <!-- Main conclusion -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-6xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-8" style="font-family: 'zrnic rg', sans-serif;">
        El Legado de una Generación
      </h2>
      
      <div 
        class="max-w-4xl mx-auto space-y-6 text-lg md:text-xl text-gray-300 leading-relaxed transform transition-all duration-1000"
        class:translate-y-0={isVisible}
        class:opacity-100={isVisible}
        class:translate-y-10={!isVisible}
        class:opacity-0={!isVisible}
      >
        <p>
          Los videojuegos han evolucionado desde simples experimentos tecnológicos hasta convertirse en el 
          <strong class="text-cyan-400">medio de entretenimiento más influyente de nuestra era</strong>. 
          Con $184 billones en ingresos anuales y 3.2 billones de jugadores activos, hemos presenciado 
          el nacimiento de una nueva forma de arte interactivo.
        </p>
        
        <p>
          Pero más allá de los números, los videojuegos representan algo profundamente humano: 
          <em class="text-purple-400">nuestra necesidad innata de jugar, explorar y conectar</em>. 
          Han democratizado la creatividad, permitiendo que cualquier persona con una idea pueda 
          crear mundos que millones pueden habitar.
        </p>
        
        <p>
          Los <strong class="text-pink-400">gamers</strong> no son solo consumidores; son 
          <strong class="text-yellow-400">pioneros digitales</strong> que han construido comunidades, 
          desarrollado nuevas formas de comunicación y demostrado que la colaboración virtual 
          puede ser tan significativa como cualquier interacción física.
        </p>
      </div>
    </div>

    <!-- Key insights grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8 mb-16">
      {#each keyInsights as insight, index}
        <div 
          class="text-center p-6 bg-gray-800/50 backdrop-blur-sm rounded-lg border border-gray-700 transform transition-all duration-1000"
          class:translate-y-0={isVisible}
          class:opacity-100={isVisible}
          class:translate-y-10={!isVisible}
          class:opacity-0={!isVisible}
          style="transition-delay: {index * 150}ms;"
        >
          <div class="text-4xl mb-4">{insight.icon}</div>
          <h3 class="text-xl font-bold text-white mb-3">{insight.title}</h3>
          <p class="text-gray-300 text-sm leading-relaxed">{insight.description}</p>
        </div>
      {/each}
    </div>

    <!-- Gaming's impact statement -->
    <div 
      class="bg-gradient-to-r from-purple-900/50 via-pink-900/50 to-cyan-900/50 backdrop-blur-sm rounded-lg p-8 md:p-12 border border-purple-700 mb-16 transform transition-all duration-1000"
      class:scale-100={isVisible}
      class:opacity-100={isVisible}
      class:scale-95={!isVisible}
      class:opacity-0={!isVisible}
      style="transition-delay: 600ms;"
    >
      <div class="text-center">
        <h3 class="text-2xl md:text-3xl font-bold text-white mb-6">
          "Los videojuegos no son solo entretenimiento; son el laboratorio donde experimentamos con nuevas formas de ser humanos."
        </h3>
        <div class="space-y-4 text-gray-300">
          <p class="text-lg">
            Han enseñado a generaciones enteras sobre <strong class="text-cyan-400">resolución de problemas</strong>, 
            <strong class="text-purple-400">trabajo en equipo</strong> y <strong class="text-pink-400">perseverancia</strong>. 
            Han creado espacios seguros para la <em class="text-yellow-400">experimentación de identidad</em> y 
            han demostrado que el <strong class="text-green-400">fracaso</strong> es simplemente otra oportunidad para aprender.
          </p>
          <p class="text-lg">
            Los gamers han construido el futuro del entretenimiento interactivo, y apenas estamos comenzando a comprender 
            el alcance completo de su impacto en la sociedad, la educación y la cultura global.
          </p>
        </div>
      </div>
    </div>

    <!-- Thank you message -->
    <div 
      class="text-center transform transition-all duration-1000"
      class:translate-y-0={isVisible}
      class:opacity-100={isVisible}
      class:translate-y-10={!isVisible}
      class:opacity-0={!isVisible}
      style="transition-delay: 800ms;"
    >
      <div class="bg-gray-800/30 backdrop-blur-sm rounded-lg p-8 border border-gray-600">
        <h3 class="text-2xl md:text-3xl font-bold text-white mb-4" style="font-family: 'zrnic rg', sans-serif;">
          Gracias por Jugar con Nosotros
        </h3>
        <p class="text-lg text-gray-300 max-w-3xl mx-auto leading-relaxed">
          Esperamos que esta visualización interactiva haya despertado tu curiosidad sobre el fascinante mundo 
          de los videojuegos y su impacto en nuestra cultura. Cada controlador que moviste, cada dato que exploraste, 
          forma parte de esta historia más grande que todos estamos escribiendo juntos.
        </p>
        <div class="mt-6 flex justify-center">
          <div class="text-4xl animate-pulse">🎮✨</div>
        </div>
      </div>
    </div>

    <!-- Call to action -->
    <div 
      class="text-center mt-12 transform transition-all duration-1000"
      class:translate-y-0={isVisible}
      class:opacity-100={isVisible}
      class:translate-y-10={!isVisible}
      class:opacity-0={!isVisible}
      style="transition-delay: 1000ms;"
    >
      <p class="text-gray-400 text-lg mb-4">
        ¿Listo para seguir explorando?
      </p>
      <button 
        onclick={(event) => { event.preventDefault(); window.scrollTo({ top: 0, behavior: 'smooth' }); }}
        class="px-8 py-3 bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 rounded-lg font-semibold text-lg transition-all duration-300 transform hover:scale-105 shadow-lg"
        style="font-family: 'zrnic rg', sans-serif;"
      >
        Volver al Inicio
      </button>
    </div>
  </div>
</section>
