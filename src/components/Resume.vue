<script setup>
  import { useTemplateRef, ref, computed, onUpdated, onMounted, watch } from 'vue';
  import Background                                   from './ResumeBg.vue';
  import S3Parallax                                   from './s3Parallax.vue';

  //Background props data
  const slides = [
    useTemplateRef('slide1'),
    useTemplateRef('slide2'),
    useTemplateRef('slide3'),
    useTemplateRef('slide4'),
  ]
  let slide3Height = ref(0);
  let slide3OT     = ref(0);

  //data
  const c_name = ref('');
  const c_email = ref('');
  const c_body = ref('');
  const isPressed = ref(false);
  const s3pos = ref(0);
  const textColor = ref('rgb(228, 242, 247)');

  // standalone methods
  const c_press = () => {
    if (isPressed.value) { return }
    else { isPressed.value = true; }
  }

  // emission control methods
  const updateTc = (newColor) => {
    textColor.value = `rgb(${newColor[0]}, ${newColor[1]}, ${newColor[2]})`
  }

  const emitS3pos = (position) => {
    s3pos.value = position;
  }

  // computed data
  const globalStyle = computed(() => ({
    color: textColor.value
  }))

  const opacity = computed(() => ({
    opacity: s3pos.value/2
  }))


  onMounted(() => { 
    slide3Height.value = slides[2].value.scrollHeight;
    slide3OT.value     = slides[2].value.offsetTop;
  })

  onUpdated(() => { 
    slide3Height.value = slides[2].value.scrollHeight;
    slide3OT.value     = slides[2].value.offsetTop;
  })
  // i think i need a watcher for those values instead lmao
</script>

<template>
  <Background :anchors="slides" @tcUpdate="updateTc" @s3pos="emitS3pos" />
  <div class="content" :style="globalStyle">
    <div ref="slide1" id="slide1" class="slide">
      <div class="contain">
        <div class="larger slide1">
          <h1>Flavio Pallini Medina</h1>
          <p>I am a programmer with experience mostly in front-end web development, and formal education based on software development and project management.</p>
          <p>I am originally from Chile, currently living in Australia through a working holiday visa, valid until February 2026.</p>
          <p>I have experience working mostly with Javascript (including Vue and React) and Python (including Pandas and Django.)</p>
          <a href="#slide4">(Skip the bells and whistles)</a>
        </div>
        <div class="smaller">
          <h1>This placeholder is holding the place for something else that's hopefully magical and insane.</h1>
        </div>
      </div>
    </div>
    <div ref="slide2" id="slide2" class="slide">
      <div class="slide2-in">
        <h2>Work Experience</h2>
        <!-- Turn into component that demonstrates tabs and keeps everything contained. -->
        <p class="central-s2">I mostly have experience working with Javascript and Python through web development frameworks, such as Vue.js, React, Django, etc.</p>
        <table class="exp-table">
          <tr class="et-row">
            <td rowspan="2" class="et-year">2024</td>
            <th class="et-left">
              <h3>Data Annotator</h3>
            </th>
            <th class="et-right">
              <a class="et-ref" href="https://www.hotmail.com">Data Annotation</a>
            </th>
          </tr>
          <tr><td colspan="2">
            <p>Freelance work on Data Annotation, correcting AI responses to math, coding and coding-adjacent prompts.</p>
          </td></tr>
          <tr class="et-row">
            <td rowspan="2" class="et-year">2021</td>
            <th class="et-left">
              <h3>Front-end web developer</h3>
            </th>
            <th class="et-right">
              <a class="et-ref" href="">CyD Tecnología</a>
            </th>
          </tr>
          <tr><td colspan="2">
            <p>Full time job in one of the largest civil engineering companies in Chile. I was in charge of constructing the front-end interfaces for an internal management site, based either from team-assembled wireframes or by my own design. I also assisted the design of the corresponding data structures and implementation of some back-end functionalities.</p>
          </td></tr>
          <tr class="et-row">
            <td rowspan="2" class="et-year">2018</td>
            <th class="et-left">
              <h3>Back-end programmer</h3>
            </th>
            <th class="et-right">
              <a class="et-ref" href="">Bindex SpA.</a>
            </th>
          </tr>
          <tr><td colspan="2">
            <p>Two months long internship, later extended into part time job for four more months. My work there involved using web scrapers to catalog and graph financial data, to then be displayed according to dynamic web based templates, also implemented by myself. I also assisted the construction of this interface's back-end to front-end communication.</p>
          </td></tr>
          <tr class="et-row">
            <td rowspan="2" class="et-year">2016~2019</td>
            <th class="et-left">
              <h3>Teacher Assistant</h3>
            </th>
            <th class="et-right">
              <a class="et-ref" href="">Universidad Diego Portales</a>
            </th>
          </tr>
          <tr><td colspan="2">
            <p>I gave demonstrations, prepared study material and graded assessments for telecommunications and mathematics courses, which helped me get familiar with common functionalities of the Microsoft Office suite.</p>
          </td></tr>
        </table>
        <a href="">Other lives I've lived (or, "why are there gaps in my resume?")</a>
      </div>
    </div>
    <div ref="slide3" id="slide3" class="slide">
      <div class="slide3-in">
        <S3Parallax 
          :style="opacity"
          :s3pos="s3pos"
          :height="slide3Height"
          :offsetTop="slide3OT" /><!-- Make it a lazy load cunt  -->
        <div class="card">
          <div class="marginalizator">          
            <h2>Education</h2>
            <p class="central-s2">I have studied for web development, but I also have a deep-set interest in the arts, particularly 2.5D animation.</p>
            <table class="exp-table">
              <tr class="et-row">
                <td rowspan="3" class="et-year">2015 -- 2020</td>
                <td class="et-title">
                  <h3>Titulo de Ingenieria Civil en Informatica y Telecomunicaciones
                    <sup class="tool">TR
                      <h6 class="tooltip">Computer Science and Telecommunications Engineering Degree (Bachelor of Science equivalent)</h6>
                    </sup>
                  </h3>
                </td>
              </tr>
              <tr class="et-row">
                <td class="et-school"><span class="fa-solid fa-graduation-cap mr1em"></span>Universidad Diego Portales</td>
              </tr>
              <tr class="et-row">
                <td class="et-desc">
                  <ul>
                    <li>11 semester program with courses on Computer Science and Software Engineering, IT projects management, and digital telecommunications networks.</li>
                    <li>Electives taken include Large Volume Database Management, Internet of Things and Inclusivity Technologies.</li>
                  </ul>
                </td>
              </tr>
              <tr class="et-row">
                <td rowspan="3" class="et-year">2018</td>
                <td class="et-title">
                  <h3>Study Abroad semester</h3>
                </td>
                <tr class="et-row">
                  <td class="et-school"><span class="fa-solid fa-graduation-cap mr1em"></span>King's College London</td>
                </tr>
                <tr class="et-row">
                  <td class="et-desc">
                    <ul>
                      <li>International student exchange program in one of the best colleges in the world, entirely in English.</li>
                      <li>Electives taken include Artificial Intelligence and Computer Graphics Systems.</li>
                    </ul>
                  </td>
                </tr>
              </tr>
              <tr class="et-row">
                <td rowspan="2" class="et-year">2000 -- 2011</td>
                <td class="et-title">
                  <h3>Primary and Secondary Education</h3>
                </td>
              </tr>
              <tr class="et-row">
                <td class="et-school"><span class="fa-solid fa-school mr1em"></span>Colegio de los Sagrados Corazones</td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div ref="slide4" id="slide4" class="slide">
      <div class="fullwidth">
        <p>I am currently available for freelance web development work and for hiring into larger projects.</p>
        <div class="contain">
          <div class="smaller mr1em">
            <h3>Formal, one-page resume pdf:</h3>
            
            <object class="pdf" 
                    data= "/resume.pdf"
                    width="100%"
                    height="500">
            </object>
            <!-- embed class="pdf" src="../../public/resume.pdf" -->
          </div>
          <div class="larger ml1em">
            <h3>Contact form:</h3>
            <p>If you're interested in my work, don't hesitate to reach out!</p>
            <div class="card">
              <form class="card-form" onsubmit="return false">
                <label>
                  Your name:
                  <!-- input type="text" name="c-name" class="fullwidth text"><br-->
                  <input v-model="c_name" placeholder="Jean Doe" class="fullwidth mt1em mb1em">
                </label><br>
                <label>
                  Your email address:  
                  <!-- input type="email" name="c-email" class="fullwidth text"><br-->
                  <input v-model="c_email" placeholder="Jeandoe@gmail.com" class="fullwidth mt1em mb1em">
                </label><br>
                <label>
                  Email body: 
                  <!--textarea name="c-body" class="fullwidth text" rows="5"></textarea><br-->
                  <textarea v-model="c_body" class="fullwidth mt1em mb1em" rows="5"></textarea>
                </label><br>
                <div class="btn-holder">
                  <input type="submit" value="Send" class="send-btn" 
                    :class="{pressed: isPressed}" @click="c_press"
                  ><br>
                </div>
              </form>
              <!--Formulario-->
            </div>
          </div>
        </div>
        <div class="fullwidth mt1em tac">
          <a href="#slide1" class="top-btn">Return to top.</a>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  html { scroll-behavior: smooth; }
  body { font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif; }
  table, th, td { border: 1px solid; }
  .contain { display: flex; } /* This may be causing issues with the parallax */
  .autom { margin: auto; }
  .smaller { width: 45%; }
  .larger { width: 55%; }
  .fullwidth { width: 100%; }
  .mr1em { margin-right: 1em; }
  .ml1em { margin-left: 1em; }
  .mt1em { margin-top: 1em; }
  .mb1em { margin-bottom: 1em; }
  
  .aid {
    position: fixed;
    background-color: #943;
    color: #fff;
  }
  
  .content {
    top: -8px;
    font-size: 16pt;
  }
  
  .slide {
    min-height: 100vh;
    padding: 5vh 10vw 0;
  }

  #slide1 {
    overflow: hidden;
    text-shadow: 2px 2px #CE884A;
  }

  #slide1 a:link, #slide1 a:visited { color: linen; }
  #slide1 a:active, #slide1 a:hover { color: pink; }
 
  #slide2 { 
    font-family: "Lucida Console", "Courier New", monospace; 
    font-size: 12pt;
  }

  #slide2 a:link { color: orange }
  #slide2 a:visited { color: pink }
  
  .slide2-in { 
    padding: 25vh 5vw;
  }
  
  .slide3-in { padding: 25vh 0; }
  
  #slide3 { 
    font-family: 'Edu QLD Beginner', cursive;
  }
  
  #slide4 { 
    height: calc(85vh - 8px);
    display: flex;
    align-items: center;
  }

  img.fill {
    width: calc(100% - 10vw);
    padding: 5vw;
  }

  .card {
    border-radius: 10%;
  }
  
  #slide3 .card {
    opacity: 75%;
    background-color: #801349;
    font-size: medium;
    padding: 5%;
    width: 67%;
    box-shadow: 1vw 1vh 1.5vw #372F407f;  /* Make this into parallax! */
    text-shadow: 1px 1px #261C21;
  }
  
  .marginalizator {
    width: 67%
  }
  
  #slide4 .card {
    background-color: #B0EBD7;    
    margin: 0 auto;
    width: 85%;
    box-shadow: 1vw 1vh 1.5vw aquamarine;  /* Make this into parallax! */
  }
  
  .card-form { padding: 10%; }
  .card-form label { padding: 0 0 16px }
  .card-form input { padding: 2px 8px 4px; }
  .card-form textarea { resize: none }
  .card-form .form-divider { height: 8px }
  .card-form .btn-holder { text-align: right; }

  .card-form textarea, .card-form input {
    -webkit-box-sizing: border-box; /* Safari/Chrome, other WebKit */
    -moz-box-sizing: border-box;    /* Firefox, other Gecko */
    box-sizing: border-box;         /* Opera/IE 8+ */
  }

  .card-form .send-btn {
    background-color: seagreen;
    font-size: large;
    color: white;
    padding: 1vh 1vw;
    margin: auto 0 5vh;
    border: none;
    box-shadow: 3px 2px 3px teal;
    border-radius: 15%;
    transition: all 0.2s;
  }

  .card-form .send-btn:hover { 
    background-color: mediumseagreen; 
    box-shadow: 3px 2px 3px seagreen;
  }

  .card-form .send-btn:active { 
    transition: all 0s;
    background-color: yellowgreen; 
    box-shadow: 3px 2px 3px seagreen;
  }

  .card-form .send-btn.pressed { 
    background-color: cadetblue; 
    box-shadow: 3px 2px 3px steelblue;
  }

  .tac { text-align: center; }
  .top-btn {
    color: white;
    padding: 1vh 1vw;
    margin: auto 0 5vh;
    border: none;
    box-shadow: 1vw 1vh 1.5vw #BFFFEA;
    border-radius: 10%;
    transition: all 0.2s;
  }

  a.top-btn:link,   a.top-btn:visited { background-color: rgb(201, 184, 237); }
  a.top-btn:active, a.top-btn:hover   { background-color: mediumpurple; }

  /* Storebought CSS lol */
  .vertical-center {
    margin: 0;
    position: relative; 
    top: 50%;
    -ms-transform: translateY(-50%);
    transform: translateY(-50%);
  }

  /* Tooltip container */
  .tool {
    position: relative;
    display: inline-block;
    border-bottom: 1px dotted black; /* If you want dots under the hoverable text */
  }

  /* Tooltip text */
  .tool .tooltip {
    visibility: hidden;
    width: 120px;
    background-color: #555;
    color: #fff;
    text-align: center;
    padding: 5px 0;
    border-radius: 6px;

    /* Position the tooltip text */
    position: absolute;
    z-index: 1;
    bottom: 125%;
    left: 50%;
    margin-left: -60px;

    /* Fade in tooltip */
    opacity: 0;
    transition: opacity 0.3s;
  }

  /* Tooltip arrow */
  .tool .tooltip::after {
    content: "";
    position: absolute;
    top: 100%;
    left: 50%;
    margin-left: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: #555 transparent transparent transparent;
  }

  /* Show the tooltip text when you mouse over the tooltip container */
  .tool:hover .tooltip {
    visibility: visible;
    opacity: 1;
  } 
</style>

<script>
</script>