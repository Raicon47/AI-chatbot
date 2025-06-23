<script setup>
import ChatBox from './components/ChatBox.vue'
</script>

<template>
  <div id="app">
    <header class="app-header">
      <h1 class="lato fw-bold text-danger">AI Chat Assistant</h1>
      <p class="fst-italic text-muted poppins">Powered by Mistral.ai</p>
    </header>
    <main class="app-main">
      <ChatBox />
       <small class="poppins text-danger">&copycopyright | property of Cyprian Nwaerema | 2025</small>
    </main>
  </div>

</template>

<style>
.lato {
font-family: 'Lato', sans-serif;
}
.poppins {
 font-family: 'Poppins', sans-serif;
}
:root {
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  line-height: 1.5;
  font-weight: 400;
  color-scheme: light dark;
  color: rgba(255, 255, 255, 0.87);
  background-color: #242424;
  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  margin: 0;
  min-height: 100vh;
  display: flex;
  place-items: center;
}

#app {
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}



</style>