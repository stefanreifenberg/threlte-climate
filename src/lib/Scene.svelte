<script>
    import { T } from '@threlte/core'
    import { OrbitControls, HTML, interactivity } from '@threlte/extras'
    import { autoType, csvParse, extent, scaleLinear } from 'd3';
    import { onMount } from 'svelte';
    import { Color } from 'three'; 
    interactivity()

    let data = []

    $: colorScale = scaleLinear()
        .domain(extent(data, d => d.MEAN))
        .range(["#0000ff", "#ff0000"])

    // position the cubes along a circle the y position should result in 0 taking d into account
    $: position = (i, d) => {
        const angle = (i / data.length) * Math.PI * 2;
        return [Math.cos(angle) * 5, 0 + Math.abs(d), Math.sin(angle) * 5];
    }

    // rotate the cubes so that they are always facing the center
    $: rotation = (i) => {
        const angle = (i / data.length) * Math.PI * 2;
        return [0, - angle, 0];
    }

    async function load() {
        const climate_data= `/global-average-air-temperature-anomalies-5.csv`;
        const response = await fetch(climate_data);

        if(response.ok) {
            data = csvParse(await response.text(), autoType);
            // arrange the data by Year
            data.sort((a, b) => a.Year - b.Year);
        }
    }

    onMount(() => {
		load();
	});

    let Year
    let Temperature
    let currentColor

    // onPointerEnter
    const getTooltipData = (d) => {
        Year = d.Year
        Temperature = d.MEAN
    }

    const getEvent = (e) => {
        currentColor = e.object.material.color
        e.object.material.color = new Color("#3f3f3f");
    }

    const clickEvent = (e) => {
        // 
    }

    const resetColor = (e) => {
        e.object.material.color = new Color(currentColor);
    }
  </script>
 

<!-- create a box geometry for every data point in data -->
{#if data}
    {#each data as d, i}
        <T.Mesh position={position(i,d.MEAN)}  rotation={rotation(i)} castShadow receiveShadow on:pointerenter={getTooltipData(d)} on:pointerenter={getEvent} on:pointerleave={resetColor} on:click={getTooltipData(d)}
            >
            <T.BoxGeometry args={[0.17, 2 * d.MEAN, 0.17]} />
            <T.MeshStandardMaterial color={colorScale(d.MEAN)} />
        </T.Mesh>

        {#if i % 20 === 0}
            <HTML position={position(i, 1)} >
                <h1 class="label">{d.Year}</h1>
            </HTML>

            <T.Mesh position={position(i, 0.5)} rotation={rotation(i)} castShadow receiveShadow>
                <T.CylinderGeometry args={[0.01, 0.01, 1, 32]} />
                <T.MeshStandardMaterial color="#000000" />
            </T.Mesh>
        {/if}
    {/each}
{/if}

<!-- add a tooltip that is only shown on hover -->
{#if Year}
<HTML position={[0, 0, 0]} >
    <div class="tooltip">
        <h3>{Year}</h3>
        <h1>°C {Temperature}</h1>
    </div>
</HTML>

<T.Mesh position={[0, Temperature, 0]} rotation={[0, 0, 0]} castShadow receiveShadow>
    <T.BoxGeometry args={[0.17, 2 * Temperature, 0.17]} />
    <T.MeshStandardMaterial color={colorScale(Temperature)} />
</T.Mesh>
{/if}


<T.PerspectiveCamera
  position={[0, 10, 10]}
  makeDefault
  fov={50}
>
  <OrbitControls enableDamping />
</T.PerspectiveCamera>

<!-- add an ambientlight -->
<T.AmbientLight intensity={0.7}/>

<!-- add a directional light -->
<T.DirectionalLight position={[3, 10, 10]} intensity={0.2} castShadow />
<T.DirectionalLight position={[-3, 10, -10]} intensity={1} castShadow />

<!-- create a round floor geometry -->
<T.Mesh position={[0, -0.001, 0]} rotation={[-Math.PI / 2, 0, 0]} receiveShadow>
  <T.CircleGeometry args={[15, 32]} />
  <T.MeshStandardMaterial />
</T.Mesh>

<style>
    .tooltip {
        width: 200px;
    }
    .label {
        pointer-events: none;
    }
</style>