<script>
    import { Canvas } from '@threlte/core'
    import Scene from '../lib/Scene.svelte'
    import Overlay from '../lib/Overlay.svelte'
  </script>
  
  <div class="canvas-wrapper">
    <Overlay />
    <Canvas>
        <Scene />
    </Canvas>
  </div>

  <style>
    .canvas-wrapper {
        position: fixed;
        top: 0;
        left:0;
        width: 100vw;
        height: 100vh;
    }
    </style>
    