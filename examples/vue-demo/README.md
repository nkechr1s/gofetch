# GoFetch Vue Demo

A demonstration of using GoFetch WASM in a Vue 3 + Vite application.

## Features

- 🚀 Vue 3 with Composition API
- ⚡️ Vite for fast development
- �� GoFetch compiled to WebAssembly
- 🎨 Clean, responsive UI
- 📋 Request logging
- 🧪 Interactive HTTP method testing

## Getting Started

### Install Dependencies

```bash
npm install
```

### Run Development Server

```bash
npm run dev
```

Open your browser to the URL shown (usually `http://localhost:5173`)

## Usage

The demo provides buttons to test different HTTP methods:

- **GET /users/1** - Fetch a single user
- **GET /users** - Fetch multiple users
- **POST /posts** - Create a new post
- **PUT /posts/1** - Update an existing post
- **DELETE /posts/1** - Delete a post

All requests are made using the GoFetch WASM client, which is a Go HTTP client compiled to WebAssembly.

## Project Structure

```
vue-demo/
├── src/
│   ├── composables/
│   │   └── useGoFetch.js     # Vue composable for GoFetch
│   ├── App.vue                # Main component
│   ├── main.js                # Entry point
│   └── style.css              # Global styles
├── vite.config.js             # Vite configuration
└── package.json               # Dependencies
```

## How It Works

1. **GoFetch WASM** - The `gofetch-wasm` package contains a Go HTTP client compiled to WebAssembly
2. **Vue Composable** - `useGoFetch.js` provides a reactive wrapper around the WASM client
3. **Vite Configuration** - Configured to handle WASM files properly

## Vite Configuration

The `vite.config.js` includes special settings for WASM:

```javascript
export default defineConfig({
  optimizeDeps: {
    exclude: ['gofetch-wasm']  // Don't pre-bundle WASM
  },
  build: {
    target: 'esnext'            // Modern JS for WASM support
  },
  assetsInclude: ['**/*.wasm']  // Include WASM as assets
})
```

## Learn More

- [GoFetch GitHub](https://github.com/fourth-ally/gofetch)
- [Vue 3 Documentation](https://vuejs.org/)
- [Vite Documentation](https://vitejs.dev/)
