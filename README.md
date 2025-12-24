# 🌍 Nationality Predictor

A modern, AI-powered web application that predicts the most likely nationalities from any given name. Built with Nuxt 4 and Vue 3, featuring a beautiful, responsive UI with interactive visualizations.

## ✨ Features

- 🔮 **AI-Powered Predictions** - Get instant nationality predictions with high accuracy
- 📊 **Visual Data Representation** - Beautiful pie charts and probability bars for each prediction
- 💡 **AI Insights** - Receive intelligent insights summarizing the predictions
- 🎨 **Modern UI** - Clean, responsive design with smooth animations
- 🌐 **Comprehensive Country Support** - Supports 150+ countries with flag emojis
- 📱 **Fully Responsive** - Works seamlessly on desktop, tablet, and mobile devices
- ⚡ **Fast & Interactive** - Real-time predictions with loading states and error handling
- 🔄 **Horizontal Scrolling** - Easy-to-browse prediction results in a scrollable format

## 🛠️ Tech Stack

- **Framework**: [Nuxt 4](https://nuxt.com/)
- **UI Framework**: Vue 3 (Composition API)
- **Styling**: Tailwind CSS
- **Package Manager**: npm/yarn/pnpm/bun

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- Node.js (v18 or higher)
- npm, yarn, pnpm, or bun

## 🚀 Getting Started

### Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd nationality-predictor
```

2. Install dependencies:

```bash
# Using npm
npm install

# Using yarn
yarn install

# Using pnpm
pnpm install

# Using bun
bun install
```

### Environment Variables

Create a `.env` file in the root directory:

```env
NUXT_BACKEND_URL=http://localhost:8000
```

Replace `http://localhost:8000` with your backend API URL.

### Development

Start the development server:

```bash
# Using npm
npm run dev

# Using yarn
yarn dev

# Using pnpm
pnpm dev

# Using bun
bun run dev
```

The application will be available at `http://localhost:3000`

## 🔌 API Integration

The application expects a backend API endpoint that accepts POST requests with the following structure:

### Request

**Endpoint**: `{BACKEND_URL}/nationality/predict-with-insight`

**Method**: `POST`

**Headers**:

```
Content-Type: application/json
```

**Body**:

```json
{
  "name": "John"
}
```

### Response

The API should return a JSON response in the following format:

```json
{
  "countries": [
    {
      "countryCode": "US",
      "probability": 0.45
    },
    {
      "countryCode": "GB",
      "probability": 0.32
    },
    {
      "countryCode": "CA",
      "probability": 0.15
    }
  ],
  "insight": "The name 'John' is most commonly associated with English-speaking countries, particularly the United States and United Kingdom."
}
```

## 🏗️ Building for Production

### Build

```bash
# Using npm
npm run build

# Using yarn
yarn build

# Using pnpm
pnpm build

# Using bun
bun run build
```

### Preview Production Build

```bash
# Using npm
npm run preview

# Using yarn
yarn preview

# Using pnpm
pnpm preview

# Using bun
bun run preview
```

### Generate Static Site

```bash
# Using npm
npm run generate

# Using yarn
yarn generate

# Using pnpm
pnpm generate

# Using bun
bun run generate
```

## 📁 Project Structure

```
nationality-predictor/
├── app/
│   ├── assets/          # Static assets (images, logos)
│   ├── pages/           # Vue pages/routes
│   │   ├── index.vue    # Main nationality predictor page
│   │   └── predictor.vue
│   ├── utils/           # Utility functions
│   └── app.vue          # Root component
├── public/              # Static files served as-is
│   ├── favicon.png      # Application favicon
│   └── robots.txt
├── nuxt.config.ts       # Nuxt configuration
├── tailwind.config.js   # Tailwind CSS configuration
├── package.json         # Dependencies and scripts
└── README.md            # This file
```

## 🎨 Features in Detail

### Prediction Display

- **Pie Charts**: Visual circular progress indicators showing probability percentages
- **Probability Bars**: Horizontal progress bars for quick comparison
- **Country Flags**: Flag emojis for each country prediction
- **Ranking**: The most likely nationality is highlighted with special styling
- **Horizontal Scroll**: All predictions are displayed in a horizontally scrollable container

### UI Components

- Gradient header with integrated search form
- Animated transitions and hover effects
- Loading states with spinner animations
- Error handling with user-friendly messages
- Responsive design that adapts to all screen sizes

## 🌐 Supported Countries

The application supports 150+ countries including:

- All major countries in North and South America
- All European countries
- Major Asian countries (China, India, Japan, etc.)
- African countries
- Oceania countries
- Middle Eastern countries

Each country is displayed with its proper flag emoji and full country name.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is private and proprietary.

## 🔗 Links

- [Nuxt Documentation](https://nuxt.com/docs)
- [Vue 3 Documentation](https://vuejs.org/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

---

Made with ❤️ using Nuxt 4 and Vue 3
