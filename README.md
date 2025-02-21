🔍 Modern Wikipedia Search
A sleek and modern Wikipedia search application built with Vue.js, featuring real-time search, infinite scrolling, and a beautiful responsive design with dark mode support.
Show Image
✨ Features

Real-time Search: Dynamic search results with debouncing for optimal performance
Infinite Scrolling: Seamlessly load more results as you scroll
Beautiful UI: Modern, clean interface with smooth animations
Dark Mode: Full support for light and dark themes
Responsive Design: Works perfectly on all device sizes
Result Highlighting: Search terms are highlighted in results
Interactive Results: Expandable result cards with additional information
Loading States: Elegant loading animations and states

🚀 Getting Started
Prerequisites

Node.js (v14 or higher)
npm or yarn

Installation

Clone the repository:

bashCopygit clone https://github.com/yourusername/modern-wikipedia-search.git
cd modern-wikipedia-search

Install dependencies:

bashCopynpm install
# or
yarn install

Start the development server:

bashCopynpm run dev
# or
yarn dev

Open your browser and visit http://localhost:5173

🏗️ Project Structure
Copysrc/
├── components/
│   ├── CustomLoader.vue      # Loading animation component
│   ├── SearchBar.vue         # Search input component
│   ├── SearchResultItem.vue  # Individual result card
│   └── SearchResultsList.vue # Results container with infinite scroll
├── assets/
│   └── main.css             # Global styles
├── App.vue                   # Root component
└── main.js                   # Application entry point
🎨 Key Components
SearchBar

Debounced search input
Clear button functionality
Smooth animations and transitions
Accessible search interface

SearchResultItem

Expandable result cards
Word count calculation
Search term highlighting
External Wikipedia link integration

SearchResultsList

Infinite scroll implementation
Staggered animation effects
Loading states
End of results detection

🛠️ Technical Features

Built with Vue 3 Composition API
Tailwind CSS for styling
Intersection Observer for infinite scrolling
Debounced search functionality
Transition and animation systems
Dark mode with system preference detection

🔧 Customization
Styling
The application uses Tailwind CSS for styling. You can customize the theme by modifying the tailwind.config.js file:
javascriptCopymodule.exports = {
  theme: {
    extend: {
      colors: {
        // Your custom colors
      }
    }
  }
}
Search Parameters
You can adjust the search debounce timing in SearchBar.vue:
javascriptCopyconst debounceSearch = () => {
  clearTimeout(debounceTimeout);
  debounceTimeout = setTimeout(() => {
    emit('search', searchInput.value);
  }, 300); // Adjust this value (in milliseconds)
};
📝 License
This project is licensed under the MIT License - see the LICENSE.md file for details.
🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

Fork the project
Create your feature branch (git checkout -b feature/AmazingFeature)
Commit your changes (git commit -m 'Add some AmazingFeature')
Push to the branch (git push origin feature/AmazingFeature)
Open a Pull Request

🙏 Acknowledgments

Built with Vue.js
Styled with Tailwind CSS
Icons from Heroicons
Wikipedia API for search functionality
