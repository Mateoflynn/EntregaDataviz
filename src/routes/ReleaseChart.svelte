<script>
  import { gameData } from './gameData.js';

  const width = 600;
  const height = 300;
  const margin = { top: 20, right: 20, bottom: 30, left: 40 };

  // Compute release counts per year
  const counts = {};
  for (const g of gameData) {
    counts[g.releaseYear] = (counts[g.releaseYear] || 0) + 1;
  }
  const years = Object.keys(counts).map(Number).sort((a, b) => a - b);
  const data = years.map((y) => ({ year: y, count: counts[y] }));

  const maxCount = Math.max(...data.map((d) => d.count));
  const minYear = years[0];
  const maxYear = years[years.length - 1];

  const xScale = (year) =>
    ((year - minYear) / (maxYear - minYear)) * (width - margin.left - margin.right) + margin.left;
  const yScale = (count) =>
    height - margin.bottom - (count / maxCount) * (height - margin.top - margin.bottom);

  const path = data
    .map((d, i) => `${i === 0 ? 'M' : 'L'} ${xScale(d.year)} ${yScale(d.count)}`)
    .join(' ');
</script>

<svg {width} {height} class="mx-auto my-12 bg-gray-800 rounded-lg">
  <path d={path} stroke="#22d3ee" fill="none" stroke-width="2" />
  {#each data as d}
    <circle cx={xScale(d.year)} cy={yScale(d.count)} r="3" fill="#ec4899" />
  {/each}
  <g font-size="10" fill="white">
    {#each data as d}
      <text x={xScale(d.year)} y={height - margin.bottom + 12} text-anchor="middle">{d.year}</text>
    {/each}
  </g>
  <line x1={margin.left} y1={height - margin.bottom} x2={width - margin.right} y2={height - margin.bottom} stroke="white" />
  <line x1={margin.left} y1={margin.top} x2={margin.left} y2={height - margin.bottom} stroke="white" />
</svg>

<style>
  svg {
    max-width: 100%;
  }
</style>