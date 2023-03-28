<script>
    import { T } from '@threlte/core'
    import { OrbitControls } from '@threlte/extras'
    import { autoType, csvParse, extent, scaleLinear } from 'd3';
    import { onMount } from 'svelte';

    let data = []

    $: colorScale = scaleLinear()
        .domain(extent(data, d => d.MEAN))
        .range(["#0000ff", "#ff0000"])


    // position the cubes along a circle where y has to always be 0 
    $: position = (i,d) => {
        const angle = (i / data.length) * Math.PI * 2;
        return [Math.cos(angle) * 5, 0, Math.sin(angle) *5];
    }

    // rotate the cubes so that they are always facing the center
    $: rotation = (i) => {
        const angle = (i / data.length) * Math.PI * 2;
        return [0, - angle, 0];
    }

    $: console.log(position(1));

    async function load() {
        const climate_data= `/global-average-air-temperature-anomalies-5.csv`;
        const response = await fetch(climate_data);

        if(response.ok) {
            data = csvParse(await response.text(), autoType);
        }
    }

    onMount(() => {
		load();
	});

  </script>
 

<!-- create a box geometry for every data point in data -->
{#if data}
    {#each data as d, i}
        <T.Mesh position={position(i,d)} scale.y={1 + 15 * d.MEAN}  rotation={rotation(i)} castShadow receiveShadow>
            <T.BoxGeometry args={[0.15, 0.15, 0.15]} />
            <T.MeshStandardMaterial color={colorScale(d.MEAN)} />
        </T.Mesh>
    {/each}
{/if}

<T.PerspectiveCamera
  position={[0, 20, 20]}
  makeDefault
  fov={50}
>
  <OrbitControls enableDamping />
</T.PerspectiveCamera>

<!-- add an ambientlight -->
<T.AmbientLight intensity={0.5} />

<!-- add a directional light -->
<T.DirectionalLight position={[3, 10, 10]} intensity={3} castShadow />
<T.DirectionalLight position={[-3, 10, -10]} intensity={0.2} castShadow/>

<!-- create a round floor geometry -->
<T.Mesh position={[0, -2, 0]} rotation={[-Math.PI / 2, 0, 0]} receiveShadow>
  <T.CircleGeometry args={[10, 32]} />
  <T.MeshStandardMaterial color="#c0c0c0" />
</T.Mesh>