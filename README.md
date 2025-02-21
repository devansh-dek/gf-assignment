# 🔍 Modern Wikipedia Search

A modern, high-performance Wikipedia search application built with Vue.js 3, featuring real-time search capabilities and an elegant user interface. ✨

## ⭐ Features

**🎯 Core Functionality:**
* 🔄 Real-time search with smart debouncing
* ♾️ Infinite scroll pagination
* 🎨 Dynamic result highlighting
* 📑 Expandable search results
* 📊 Word count tracking

**💻 Technical Highlights:**
* ⚡ Vue 3 Composition API
* 🎭 Tailwind CSS styling
* 📱 Responsive design
* 🎬 Smooth animations
* ♿ Accessibility features

## 🚀 Installation

```bash
# Clone the repository
git clone https://github.com/devansh-dek/gf-assignment.git

# Navigate to project directory
cd gf-assiggment

# Install dependencies
npm install

# Start development server
npm run dev
```

## 📁 Project Structure

```
src/
├── components/
│   ├── CustomLoader.vue     # Loading animation
│   ├── SearchBar.vue        # Search input field
│   ├── SearchResultItem.vue # Result card component
│   └── SearchResultsList.vue # Results container
├── assets/
│   └── main.css            # Global styles
├── App.vue                  # Root component
└── main.js                 # Entry point
```

## 🧩 Component Overview

### 🔍 SearchBar Component

```vue
<!-- Search input with debounce -->
<input
  type="text"
  v-model="searchInput"
  @input="debounceSearch"
  placeholder="Search Wikipedia..."
  class="search-input"
/>
```

**✨ Features:**
* ⌛ Debounced search input
* 🔄 Clear button
* 📜 Search history
* 💭 Auto-suggestions

### 📑 SearchResultItem Component

```vue
<!-- Result card with expandable content -->
<div class="result-card">
  <h3 v-html="highlightMatch(result.title)"></h3>
  <p v-html="highlightMatch(result.snippet)"></p>
</div>
```

**✨ Features:**
* 📂 Expandable details
* 🎯 Highlighted search terms
* 📊 Word count display
* 🔗 Direct Wikipedia links

### 📋 SearchResultsList Component

```vue
<!-- Results container with infinite scroll -->
<transition-group
  name="staggered-fade"
  tag="div"
  class="results-container"
>
  <SearchResultItem
    v-for="result in results"
    :key="result.pageid"
    :result="result"
  />
</transition-group>
```

**✨ Features:**
* ♾️ Infinite scroll
* ⌛ Loading states
* 🎬 Smooth transitions
* ⚠️ Error handling

## ⚙️ Configuration

### 🎨 Tailwind Configuration

```javascript
// tailwind.config.js
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        primary: '#4F46E5',
        secondary: '#6366F1'
      }
    }
  }
}
```

### 🔍 Search Configuration

```javascript
// SearchBar.vue
const debounceSearch = () => {
  clearTimeout(debounceTimeout);
  debounceTimeout = setTimeout(() => {
    emit('search', searchInput.value);
  }, 300);
};
```

## 📖 Usage

1. Start the development server:
   ```bash
   npm run dev
   ```

2. Open your browser:
   ```
   http://localhost:5173
   ```

3. Begin searching:
   * ⌨️ Type in the search bar
   * 🔄 Results appear in real-time
   * 📜 Scroll for more results
   * 👆 Click results to expand

## 👨‍💻 Development

### 🛠️ Commands

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run tests
npm run test
```

### 📝 Code Style

This project follows Vue.js style guide and uses ESLint with the following configuration:

```javascript
// .eslintrc.js
module.exports = {
  extends: [
    'plugin:vue/vue3-recommended',
    'eslint:recommended'
  ]
}
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. Commit your changes
   ```bash
   git commit -m 'Add AmazingFeature'
   ```
4. Push to the branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
5. Open a Pull Request

---

Made with ❤️ using Vue.js and Tailwind CSS
