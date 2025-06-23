<script setup>
import { ref } from 'vue'
import axios from 'axios'

const messages = ref([])
const userInput = ref('')
const isLoading = ref(false)

const MISTRAL_ENDPOINT = 'https://api.mistral.ai/v1/chat/completions'
const MISTRAL_KEY = 'fwwGB0dVtGh8U2oUvMMuFitICyCZ6RBF' // Replace with your actual key

const sendMessage = async () => {
  if (!userInput.value.trim()) return
  
  messages.value.push({ role: 'user', content: userInput.value })
  const currentMessage = userInput.value
  userInput.value = ''
  isLoading.value = true
  
  try {
    const response = await axios.post(
      MISTRAL_ENDPOINT,
      {
        model: 'mistral-tiny', // or 'mistral-small', 'mistral-medium'
        messages: messages.value,
        temperature: 0.7,
        max_tokens: 1000
      },
      {
        headers: {
          'Authorization': `Bearer ${MISTRAL_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    )
    
    const aiResponse = response.data.choices[0].message.content
    messages.value.push({ role: 'assistant', content: aiResponse })
  } catch (error) {
    console.error('Mistral API error:', error)
    messages.value.push({ 
      role: 'system', 
      content: 'Error getting response from Mistral' 
    })
  } finally {
    isLoading.value = false
  }
}
</script>

<template>
  <div class="container-fluid d-flex flex-column p-0">

    <!-- Error Alert -->
    <div v-if="error" class="alert alert-danger alert-dismissible fade show m-3" role="alert">
      {{ error }}
      <button type="button" class="btn-close" @click="error = null" aria-label="Close"></button>
    </div>

    <!-- Chat Messages -->
    <div class="flex-grow-1 overflow-auto p-3">
      <div v-for="(message, index) in messages" :key="index" 
           class="mb-3 d-flex" 
           :class="{'justify-content-end': message.role === 'user'}">
        <div class="card" 
             :class="{
               'bg-danger text-white poppins my-2': message.role === 'user',
               'bg-dark text-light poppins p-2 fst-italic': message.role === 'assistant',
               'bg-warning': message.role === 'system'
             }">
          <div class="card-body p-2">
            <p class="card-text mb-0">{{ message.content }}</p>
          </div>
        </div>
      </div>

      <!-- Typing Indicator -->
      <div v-if="isLoading" class="mb-3 d-flex">
        <div class="card bg-light">
          <div class="card-body p-2">
            <div class="typing-indicator">
              <span class="dot"></span>
              <span class="dot"></span>
              <span class="dot"></span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Input Area -->
    <div class="p-3">
      <form @submit.prevent="sendMessage" class="input-group">
        <input
          v-model="userInput"
          type="text"
          class="form-control"
          :disabled="isLoading"
          placeholder="Type your message..."
          autocomplete="off"
        >
        <button 
          class="btn btn-dark" 
          type="submit" 
          :disabled="!userInput.trim() || isLoading">
          <span v-if="!isLoading">Send</span>
          <span v-else class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
        </button>
      </form>
    </div>
  </div>
  
</template>

<style scoped>
/* Custom typing indicator animation */
.typing-indicator {
  display: inline-flex;
  align-items: center;
  height: 17px;
}

.dot {
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #6c757d;
  margin: 0 2px;
  animation: bounce 1.4s infinite ease-in-out;
}

.dot:nth-child(2) {
  animation-delay: 0.2s;
}

.dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes bounce {
  0%, 60%, 100% { transform: translateY(0); }
  30% { transform: translateY(-4px); }
}

/* Custom card styles for messages */
.card {
  max-width: 80%;
  border-radius: 15px;
}

.card-body {
  padding: 0.5rem 1rem;
}

/* Responsive adjustments */
@media (max-width: 576px) {
  .card {
    max-width: 90%;
  }
}
</style>