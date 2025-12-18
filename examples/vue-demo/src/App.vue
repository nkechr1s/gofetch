<template>
  <div id="app">
    <header>
      <h1>🚀 GoFetch WASM Demo (Vue)</h1>
      <p>A Go HTTP client compiled to WebAssembly, running in your browser!</p>
    </header>

    <div v-if="isLoading" class="loading">
      <div class="spinner"></div>
      <p>Loading GoFetch WASM...</p>
    </div>

    <div v-else-if="error" class="error">
      <h2>❌ Error Loading GoFetch</h2>
      <p>{{ error }}</p>
    </div>

    <div v-else class="container">
      <!-- Request Tester -->
      <div class="card">
        <h2>🧪 Test Requests</h2>
        
        <div class="button-group">
          <button @click="fetchUser" class="btn btn-primary">
            GET /users/1
          </button>
          <button @click="fetchUsers" class="btn btn-info">
            GET /users
          </button>
          <button @click="createPost" class="btn btn-success">
            POST /posts
          </button>
          <button @click="updatePost" class="btn btn-warning">
            PUT /posts/1
          </button>
          <button @click="deletePost" class="btn btn-danger">
            DELETE /posts/1
          </button>
        </div>
      </div>

      <!-- Response Display -->
      <div v-if="currentResponse" class="card">
        <h2>📦 Response</h2>
        <div class="response">
          <div class="response-meta">
            <span class="badge">Status: {{ currentResponse.statusCode }}</span>
          </div>
          <pre class="response-body">{{ JSON.stringify(currentResponse.data, null, 2) }}</pre>
        </div>
      </div>

      <!-- Logs -->
      <div class="card">
        <div class="logs-header">
          <h2>📋 Request Logs</h2>
          <button @click="clearLogs" class="btn btn-small">Clear Logs</button>
        </div>
        
        <div class="logs">
          <div 
            v-for="(log, index) in logs" 
            :key="index" 
            :class="['log-entry', `log-${log.type}`]"
          >
            <span class="log-time">{{ log.timestamp }}</span>
            <span class="log-message">{{ log.message }}</span>
            <pre v-if="log.data" class="log-data">{{ JSON.stringify(log.data, null, 2) }}</pre>
          </div>
          <div v-if="logs.length === 0" class="empty-logs">
            No logs yet. Try making a request!
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useGoFetch } from './composables/useGoFetch.js'

const { isLoading, error, gofetch, logs, clearLogs } = useGoFetch()
const currentResponse = ref(null)

const fetchUser = async () => {
  try {
    const response = await gofetch.value.get('/users/1')
    currentResponse.value = response
  } catch (err) {
    console.error('Request failed:', err)
  }
}

const fetchUsers = async () => {
  try {
    const response = await gofetch.value.get('/users', { _limit: 5 })
    currentResponse.value = response
  } catch (err) {
    console.error('Request failed:', err)
  }
}

const createPost = async () => {
  try {
    const response = await gofetch.value.post('/posts', null, {
      title: 'GoFetch WASM Demo',
      body: 'This post was created using GoFetch compiled to WebAssembly!',
      userId: 1
    })
    currentResponse.value = response
  } catch (err) {
    console.error('Request failed:', err)
  }
}

const updatePost = async () => {
  try {
    const response = await gofetch.value.put('/posts/1', null, {
      id: 1,
      title: 'Updated via GoFetch WASM',
      body: 'This post was updated using PUT request',
      userId: 1
    })
    currentResponse.value = response
  } catch (err) {
    console.error('Request failed:', err)
  }
}

const deletePost = async () => {
  try {
    const response = await gofetch.value.delete('/posts/1')
    currentResponse.value = response
  } catch (err) {
    console.error('Request failed:', err)
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  color: #333;
}

header {
  text-align: center;
  margin-bottom: 40px;
  padding: 40px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

header h1 {
  margin: 0 0 10px 0;
  font-size: 2.5rem;
}

header p {
  margin: 0;
  opacity: 0.9;
  font-size: 1.1rem;
}

.loading {
  text-align: center;
  padding: 60px 20px;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #667eea;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error {
  background: #fee;
  border: 2px solid #fcc;
  border-radius: 8px;
  padding: 30px;
  text-align: center;
}

.container {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.card {
  background: white;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.card h2 {
  margin: 0 0 20px 0;
  color: #667eea;
  font-size: 1.5rem;
}

.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.btn {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.btn-primary {
  background: #667eea;
  color: white;
}

.btn-info {
  background: #3498db;
  color: white;
}

.btn-success {
  background: #2ecc71;
  color: white;
}

.btn-warning {
  background: #f39c12;
  color: white;
}

.btn-danger {
  background: #e74c3c;
  color: white;
}

.btn-small {
  padding: 8px 16px;
  font-size: 12px;
}

.response {
  margin-top: 16px;
}

.response-meta {
  margin-bottom: 12px;
}

.badge {
  display: inline-block;
  background: #667eea;
  color: white;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 600;
}

.response-body {
  background: #f8f9fa;
  border: 1px solid #e9ecef;
  border-radius: 6px;
  padding: 16px;
  overflow-x: auto;
  font-size: 13px;
  line-height: 1.5;
}

.logs-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.logs-header h2 {
  margin: 0;
}

.logs {
  max-height: 400px;
  overflow-y: auto;
  border: 1px solid #e9ecef;
  border-radius: 6px;
  background: #f8f9fa;
}

.log-entry {
  padding: 12px 16px;
  border-bottom: 1px solid #e9ecef;
  font-size: 13px;
}

.log-entry:last-child {
  border-bottom: none;
}

.log-time {
  color: #6c757d;
  margin-right: 12px;
  font-weight: 600;
}

.log-message {
  color: #333;
}

.log-data {
  margin: 8px 0 0 0;
  padding: 8px;
  background: white;
  border-radius: 4px;
  font-size: 12px;
}

.log-success {
  background: #d4edda;
  border-left: 4px solid #28a745;
}

.log-error {
  background: #f8d7da;
  border-left: 4px solid #dc3545;
}

.log-info {
  background: #d1ecf1;
  border-left: 4px solid #17a2b8;
}

.empty-logs {
  padding: 40px;
  text-align: center;
  color: #6c757d;
  font-style: italic;
}

@media (max-width: 768px) {
  header h1 {
    font-size: 1.8rem;
  }
  
  .button-group {
    flex-direction: column;
  }
  
  .btn {
    width: 100%;
  }
}
</style>
