<script setup>
  import { reactive, ref, defineProps, onMounted, onUpdated, computed, watchEffect } from 'vue';

  // the original pictures were drawn on this resolution
  const ref_res = 1440
  const init_pos = {
    book:    { width:  653, left:  310, bottom:  -56 },
    rarm:    { width:  199, left:  518, bottom: -315 },
    larm:    { width:  461, right:  97, bottom: -380 },
    student: { width:  799, right:  17, top:     -29 },
    tree:    { width: 1388, right:   0, top:    -380 },
    clouds4: { width:  953, left: -186, top:     117 },
    clouds3: { width: 1130, left: -102, bottom:  167 },
    clouds2: { width:  685, right: 103, bottom:   85 },
    clouds1: { width:  708, left:   21, bottom:   72 },
  }
  
  // props
  const props = defineProps({
    s3pos: Number,
    height: Number,
    offsetTop: Number,
  })

  // protected data
  let scrollY = 0;
  let width = 0;
  let res_ratio = 0;
  let px_ratio = 0;

  // public data
  const styles = ref({});

  const updatePositions = () => {
    scrollY = window.scrollY;
    width = window.innerWidth;
    res_ratio = width/ref_res;
    console.log(`px_ratio = height/(scrollY-offsetTop)`)
    console.log(`${px_ratio} = ${props.height}/(${scrollY}-${props.offsetTop})`)
    let ret = {}
    for (let layer in init_pos) {
      let aux = {}
      for (let property in init_pos[layer]) {
        aux[property] = `${parseInt((property == 'width')? init_pos[layer][property]*res_ratio: init_pos[layer][property]*res_ratio)}px`
      }
      ret[layer] = aux
    }
    styles.value = ret
    console.log(styles.value)
  }

  const sizeUpdate = (e) => {
    updatePositions()
  }

  watchEffect(async () => {
    // scrollY = window.scrollY
    px_ratio = props.height/(scrollY-props.offsetTop)
    console.log(`Updated: height. PX ratio: ${px_ratio}`)
  })

  onMounted(() => {
    window.addEventListener("resize", sizeUpdate)
    updatePositions()
  })
</script>

<template>
  <div class="canvas" id="student">
    <div class="student-layer zm2" :style="styles['book']">
      <img id="book" class="student-image" src="src/images/book.png">
    </div>
    <div class="student-layer zm3" :style="styles['rarm']">
      <img id="rarm" class="student-image" src="src/images/rarm.png">
    </div>
    <div class="student-layer zm3" :style="styles['larm']">
      <img id="larm" class="student-image" src="src/images/larm.png">
    </div>
    <div class="student-layer zm4" :style="styles['student']">
      <img id="student" class="student-image" src="src/images/student.png">
    </div>
    <div class="student-layer zm5" :style="styles['tree']">
      <img id="tree" class="student-image" src="src/images/tree.png">
    </div>
    <div class="student-layer zm6" :style="styles['clouds4']">
      <img id="clouds4" class="student-image" src="src/images/clouds4.png">
    </div>
    <div class="student-layer zm7" :style="styles['clouds3']">
      <img id="clouds3" class="student-image" src="src/images/clouds3.png">
    </div>
    <div class="student-layer zm8" :style="styles['clouds2']">
      <img id="clouds2" class="student-image" src="src/images/clouds2.png">
    </div>
    <div class="student-layer zm8" :style="styles['clouds1']">
      <img id="clouds1" class="student-image" src="src/images/clouds1.png">
    </div>
  </div>
</template>

<style>
  .zm2 { z-index: -2; }
  .zm3 { z-index: -3; }
  .zm4 { z-index: -4; }
  .zm5 { z-index: -5; }
  .zm6 { z-index: -6; }
  .zm7 { z-index: -7; }
  .zm8 { z-index: -8; }
  .parallax-container {
    width: 100vw;
    height: 100vh;
    left: 0px;
    top: -8px;
    position: fixed;
    z-index: -9;
    transition: all 0.2s;
  }

  .canvas {
    overflow: hidden;
    position: fixed;
    opacity: 50%;
    z-index: -1;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);    
    width: 100vw;
    height: calc(100vw*9/16);
    box-shadow: inset 0px 0px 1vw 0px #000;
  }

  .student-layer {
    position: absolute;
    overflow: hidden;
  }

  .student-image {
    display: flex;
    position: relative;
    width: 100%;
    height: auto;
  }

</style>