# Venture Compass

A modern web application for discovering, analyzing, and enriching startup company data in real-time. Built with React, TypeScript, Express, and powered by Google's Gemini API.

## 📋 Project Description

Venture Compass is an intelligent company discovery and enrichment platform designed for venture capitalists, startup analysts, and researchers. It provides a comprehensive interface to browse companies, view detailed profiles, take personal notes, and automatically enrich company data by analyzing their websites using AI.

The platform combines a sleek React frontend with a robust Express backend to deliver real-time company enrichment through intelligent web scraping and AI-powered analysis.

## ✨ Features

### Core Features
- **Company Discovery**: Browse and search through a curated list of companies
- **Advanced Filtering**: Filter by industry, stage, location, and more
- **Detailed Profiles**: View comprehensive company information including:
  - Company overview and description
  - Funding information and stage
  - Employee count and location
  - Website links
  - Company signals and recent activity
- **Personal Notes**: Add and save analysis notes for each company (stored locally)
- **Save to List**: Bookmark companies for future reference (persistent storage)

### Enrichment Features
- **Website Analysis**: Automatically fetch and parse company websites
- **AI-Powered Insights**: Extract structured data using Google Gemini API:
  - Company summary (1-2 sentences)
  - Services/products (3-6 bullet points)
  - Industry keywords (5-10 terms)
  - Business signals (hiring, blog, changelog, etc.)
- **Signal Detection**: Identify key indicators:
  - Active hiring (careers page)
  - Content marketing (blog section)
  - Product updates (changelog)
  - Technical depth (developer documentation)
  - Enterprise features (security certifications)
- **Source Tracking**: View and analyze URLs scraped from company websites

## 🛠️ Tech Stack

### Frontend
- **React 19** - UI framework
- **TypeScript** - Type safety
- **Vite** - Build tool and dev server
- **Tailwind CSS** - Styling
- **Shadcn/ui** - Component library
- **Wouter** - Lightweight routing
- **TanStack Query** - Data fetching
- **Lucide React** - Icons

### Backend
- **Express.js** - API framework
- **TypeScript** - Type safety
- **Axios** - HTTP client
- **Cheerio** - HTML parsing
- **Google Generative AI** - AI enrichment
- **Node.js** - Runtime

### Database & Storage
- **localStorage** (Frontend) - Notes and saved lists
- **PostgreSQL** (Optional) - User authentication setup

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm
- Google Gemini API key ([Get one free](https://aistudio.google.com/app/apikey))

### Installation

1. **Clone and navigate to project**
   ```bash
   cd Venture-Compass
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create environment file**
   ```bash
   cp .env.example .env
   ```

4. **Add your Gemini API key to `.env`**
   ```
   GEMINI_API_KEY=your_api_key_here
   NODE_ENV=development
   ```

5. **Start development server**
   ```bash
   npm run dev:client
   ```
   
   The application will be available at `http://localhost:5000`

## 🔧 Environment Variables

Create a `.env` file in the project root with the following variables:

```env
# Required: Google Gemini API Key
# Get from: https://aistudio.google.com/app/apikey
GEMINI_API_KEY=your_gemini_api_key_here

# Optional: Environment setting
NODE_ENV=development
```

### Obtaining a Gemini API Key

1. Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click "Create API Key"
3. Copy the generated key
4. Paste it into your `.env` file

**Free Tier Limits:**
- 60 requests per minute
- Sufficient for development and testing

## 📚 How Enrichment Works

The enrichment process involves three main steps:

### 1. **Website Fetching**
- Sends HTTP request to the company website
- Handles redirects and SSL certificates
- 10-second timeout for each request

### 2. **Content Extraction**
- Parses HTML using Cheerio
- Extracts meaningful content from:
  - Page title and meta descriptions
  - Main content sections
  - Feature and product sections
  - Navigation links
- Removes JavaScript and style code noise

### 3. **AI Analysis**
- Sends extracted content to Google Gemini API
- Generates:
  - **Summary**: 1-2 sentence description
  - **Services**: 3-6 key products/services
  - **Keywords**: 5-10 industry terms
  - **Signals**: Business indicators

### 4. **Signal Detection**
Automatically identifies:
- **Careers page** → Hiring activity indicator
- **Blog/News** → Content strategy signal
- **Changelog** → Product development activity
- **API/Docs** → Developer focus
- **Security badges** → Enterprise readiness
- **integrations** → Ecosystem connection
- **Mobile app** → Multi-platform strategy

## 📁 Project Structure

```
Venture-Compass/
├── client/                    # React frontend
│   ├── src/
│   │   ├── pages/            # Page components
│   │   │   ├── Companies.tsx    # Company listing
│   │   │   ├── CompanyProfile.tsx # Profile with enrichment
│   │   │   └── Dashboard.tsx
│   │   ├── components/       # Reusable components
│   │   ├── lib/              # Utilities and mock data
│   │   └── hooks/            # Custom React hooks
│   └── index.html            # Entry point
├── server/                    # Express backend
│   ├── index.ts              # Server setup
│   ├── routes.ts             # API routes
│   ├── enrichment.ts         # Enrichment logic
│   └── storage.ts            # Data storage
├── shared/                    # Shared types
├── .env.example              # Environment template
├── package.json
├── tsconfig.json
└── README.md                 # This file
```

## 🔌 API Endpoints

### POST /api/enrich

Enriches company data by analyzing their website.

**Request:**
```bash
POST /api/enrich
Content-Type: application/json

{
  "website": "stripe.com"
}
```

**Response:**
```json
{
  "summary": "Global payment processing platform",
  "whatTheyDo": [
    "Payment processing",
    "Billing management",
    "Fraud detection"
  ],
  "keywords": ["fintech", "payments", "api"],
  "signals": [
    "Careers page exists - actively hiring",
    "Blog exists - sharing updates"
  ],
  "sources": ["/careers", "/blog", "/docs"],
  "timestamp": "2026-02-20T10:30:45.123Z"
}
```

**Error Responses:**
- `400` - Invalid website URL
- `404` - Website not found/unreachable
- `504` - Request timeout
- `500` - Server error

For detailed API documentation, see [API_ENRICHMENT.md](./API_ENRICHMENT.md)

## 💾 Data Persistence

### Frontend Storage
- **Notes**: Stored in browser's localStorage with key `company-notes-{companyId}`
- **Saved Companies**: Array stored in localStorage under `saved-companies`
- **Persistence**: Survives browser refresh, cleared when cache is cleared

### Backend Storage
- **API Key**: Stored securely in `.env` file (server-side only)
- **User Data**: Can be extended to use PostgreSQL (optional)

## 🏗️ Build & Deployment

### Building for Production

```bash
# Build both client and server
npm run build

# Output:
# - Client: dist/public/
# - Server: dist/index.cjs
```

### Running in Production

```bash
# Set environment
export NODE_ENV=production

# Start server (serves both API and frontend)
npm start

# Server runs on http://localhost:3000 (configurable)
```

### Docker Deployment (Optional)

Create a `Dockerfile`:
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY dist ./dist

ENV NODE_ENV=production
EXPOSE 3000

CMD ["node", "dist/index.cjs"]
```

Build and run:
```bash
docker build -t venture-compass .
docker run -e GEMINI_API_KEY=your_key -p 3000:3000 venture-compass
```

### Deploying to Vercel/Replit

1. Push code to GitHub
2. Connect repository to deployment platform
3. Add environment variables:
   - `GEMINI_API_KEY`
   - `NODE_ENV=production`
4. Deployment automatically builds and deploys

## 🧪 Development Commands

```bash
# Start development servers (client + watch)
npm run dev:client

# Build for production
npm run build

# Start production server
npm start

# Type check
npm run check

# Database operations (if using Drizzle)
npm run db:push
```

## 🔒 Security Considerations

1. **API Key Protection**
   - Never commit `.env` file to version control
   - Keys stored server-side only
   - Never expose in frontend code

2. **URL Validation**
   - All URLs validated before processing
   - Prevents injection attacks

3. **Timeout Protection**
   - 10-second timeout on all web requests
   - Prevents hanging connections

4. **Error Handling**
   - Sensitive errors never exposed to frontend
   - Proper HTTP status codes
   - Logged server-side for debugging

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

MIT License - feel free to use this project for personal or commercial purposes.

## 🆘 Troubleshooting

### API Key not working
- Verify key is from [aistudio.google.com](https://aistudio.google.com/app/apikey)
- Check `.env` file has `GEMINI_API_KEY=` (no quotes)
- Restart development server after changing `.env`

### Website enrichment fails
- Ensure website is publicly accessible
- Check if website blocks automated requests
- Verify internet connection
- Try a different company website

### Navigation not working
- Clear browser cache/localStorage
- Check browser console for errors
- Ensure dev server is running (`npm run dev:client`)
- Try hard refresh (Ctrl+Shift+R)

### localhost:5000 not accessible
- Confirm dev server is running
- Check if port 5000 is already in use
- Try different port: `npx vite dev --port 3000`

## 📞 Support

For issues, questions, or feature requests, please create an issue on the GitHub repository.

## 🙏 Acknowledgments

- Built with [Shadcn/ui](https://ui.shadcn.com/) components
- Powered by [Google Gemini API](https://ai.google.dev/)
- Styled with [Tailwind CSS](https://tailwindcss.com/)
- Icons by [Lucide React](https://lucide.dev/)

---

**Last Updated:** February 20, 2026
**Version:** 1.0.0
