# News-O-Gram

An Instagram-style news scrolling web application built with Vue 3, TypeScript, and Tailwind CSS. Browse through curated news from various categories with a smooth, doom-scrolling experience.

## Features

- **Instagram-Style Feed**: Single column, mobile-first design with infinite scroll
- **10 News Categories**: AI, Mobile, Tech, Medicine, Chip Industry, Internet, Security, Gaming, GPU, and Google
- **Location-Based Content**: News prioritized for India (Phase 1 - hardcoded)
- **Social Sharing**: Share articles via Facebook, Twitter, LinkedIn, Reddit, and WhatsApp
- **Lazy Loading**: Efficient infinite scroll with intersection observer
- **Responsive Design**: Optimized for mobile phones, tablets, laptops, and desktops
- **No Registration Required**: Open access for all users

## Tech Stack

- **Frontend Framework**: Vue 3 (Composition API)
- **Language**: TypeScript
- **Styling**: Tailwind CSS 3.4.17
- **Build Tool**: Vite
- **Package Manager**: npm

## Getting Started

### Prerequisites

- Node.js 16+
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd news-o-gram
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to:
```
http://localhost:5173
```

### Build for Production

```bash
npm run build
```

The production-ready files will be in the `dist` directory.

### Preview Production Build

```bash
npm run preview
```

## Project Structure

```
news-o-gram/
├── public/
│   └── sampleData.json         # Sample news data (10 items)
├── src/
│   ├── assets/                 # Static assets
│   ├── components/
│   │   ├── TopNavigation.vue   # Header with logo and search
│   │   ├── NewsCard.vue        # Individual news card with share
│   │   └── NewsFeed.vue        # Feed container with infinite scroll
│   ├── types/
│   │   └── news.ts             # TypeScript interfaces
│   ├── App.vue                 # Root component
│   ├── main.ts                 # App entry point
│   └── style.css               # Global styles + Tailwind
├── index.html
├── vite.config.ts
├── tailwind.config.js
├── tsconfig.json
├── PHASE2.md                   # Phase 2 roadmap
└── README.md
```

## Key Components

### TopNavigation
- Logo and app branding
- Search button (placeholder for Phase 1)
- Sticky header that stays at the top

### NewsCard
- News image (from Unsplash)
- Headline and description
- Source and timestamp
- Category tags
- Share button with social media options
- Copy URL functionality
- Link to original article

### NewsFeed
- Infinite scroll implementation
- Lazy loading (3 items at a time)
- Loading spinner
- End of feed indicator

## Data Structure

Each news item includes:
```typescript
{
  id: string;
  headline: string;
  description: string;
  imageUrl: string;
  source: string;
  timestamp: string;
  category: string;
  tags: string[];
  articleUrl: string;
  region: string;
}
```

## Responsive Breakpoints

- **Mobile**: < 640px
- **Tablet**: 640px - 1024px
- **Laptop/Desktop**: > 1024px

All layouts use a max-width container (2xl: 672px) for optimal readability.

## Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Performance

- Lazy loading images
- Intersection Observer for infinite scroll
- Optimized bundle size with Vite
- Tailwind CSS purging unused styles

## License

ISC

## Version Control

Current version: 0.1.1

## Contact

For questions or feedback, please open an issue on GitHub.

---

**Built with ❤️ using Vue 3, TypeScript, and Tailwind CSS**
