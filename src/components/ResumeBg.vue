<script setup>
  import { onMounted, computed, reactive } from 'vue';

  const props = defineProps({
    anchors: Array,
  })

  const emit = defineEmits(['tcUpdate', 's3pos'])

  // Global variables
  let controllers = [1, 0, 0, 0];
  let bgColors = [
    [
      [214,   158,    100 ],
      [218,   171,    117 ],
      [222,   184,    135 ],
    ], [
      [0,     0,      0   ],
      [0,     32,     64  ],
      [0,     128,    128 ],
    ], [
      [72,    61,     139 ],
      [216,   108,    162 ],
      [255,   140,    0   ],
    ], [
      [240,   255,    240 ],
      [240,   255,    240 ],
      [240,   255,    240 ],
    ] 
  ];
  let textColors = [
    [228,   242,    247 ],
    [255,   255,    0   ],
    [255,   140,    0   ],
    [75,    0,      130 ]
  ]

  // new colors slots
  let nbc = reactive([214, 158, 100]);
  let nic = reactive([218, 171, 117]);
  let nsc = reactive([222, 184, 135]);
  let ntc = reactive([228, 242, 247]);

  onMounted(() => {
    // Observer options setup
    let threshold = [];
    for(let i=0; i<=1; i+=(1/10)){
      threshold.push(i)
    }
    const options = {
      threshold: threshold
    }

    // Observer callback function
    const observer = new IntersectionObserver((entries) => {
      // flush colors slots
      for (let slot of [nbc, nic, nsc, ntc]) {
        for (let channel in slot) {
          slot[channel] = 0
        }
      }

      // Update values
      for (let entry of entries) {
        const index = parseInt(entry.target.id[5]) - 1
        const ir = entry.intersectionRatio
        // only update if top-down value has changed (to prevent strobing effect)
        if (controllers[index] == ir && 
          ir > 0) 
          { break; }
        controllers[index] = ir;
      }

      // fx: Normalize ratios for color adjustment
      let modulus = controllers.reduce((ps, a) => ps + a, 0);
      const translateRatio = (ratio, modulus) => {
        let normal = ratio/modulus
        if      (normal <= 0.15)  { return 0;                   }
        else if (normal < 0.85)   { return (normal - 0.15)/0.7; }
        else                      { return 1;                   }
      }
      // fx: truncate values for each channel
      const truncateValue = (value => {
        if      (value > 255) { return 255;   }
        else if (value < 0)   { return 0;     }
        else                  { return value; }
      })
      // fx: normalize opacity change
      const s3irNormalizer = ((ir) => {
        if   (ir <= 2/3)  { return 0            }
        else              { return (3 * ir - 2) }
      })

      // apply color changes
      for (let channel in nbc) {
        controllers.forEach((ratio, index) => {
          const translated = translateRatio(ratio, modulus)
          nbc[channel] = nbc[channel] + parseInt(bgColors[index][0][channel] * translated);
          nic[channel] = nic[channel] + parseInt(bgColors[index][1][channel] * translated);
          nsc[channel] = nsc[channel] + parseInt(bgColors[index][2][channel] * translated);
          ntc[channel] = ntc[channel] + parseInt(textColors[index][channel] * translated);
          if (index == 2 ) { emit('s3pos', s3irNormalizer(translated)) }
        })
        // prevent overflow
        nbc[channel] = truncateValue(nbc[channel])
        nsc[channel] = truncateValue(nsc[channel])
        ntc[channel] = truncateValue(ntc[channel])
      }

      emit('tcUpdate', ntc)
    }, options)

    // finish observer setup
    for(let anchor of props.anchors){
      observer.observe(anchor.value)
    }
  })

  const stylings = computed(() => ({
    background: `radial-gradient(ellipse at bottom 0% right 33%, rgb(${nsc[0]}, ${nsc[1]}, ${nsc[2]}) 15%, rgb(${nic[0]}, ${nic[1]}, ${nic[2]}) 50%, rgb(${nbc[0]}, ${nbc[1]}, ${nbc[2]}) 75%)`
  }))
</script>

<template>
  <div class="background" :style="stylings" />
</template>

<style>
  .background {
    width: 100%;
    height: 100vh;
    left: 0px;
    top: -8px;
    position: fixed;
    z-index: -10;
    transition: all 0.2s;
  }
</style>