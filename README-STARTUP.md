# Your AI Startup Platform

> Built on OpenWebUI - Customized for Your Startup Vision

## 🚀 Project Overview

This is a customized version of OpenWebUI designed specifically for your startup. It provides a robust foundation for building AI-powered chat interfaces and applications.

### Key Features
- 🤖 Multiple LLM support (Ollama, OpenAI, etc.)
- 👥 User management and authentication
- 📱 Responsive design with PWA support
- 🔧 Customizable UI and branding
- 📚 RAG (Retrieval Augmented Generation) capabilities
- 🌐 Web search integration
- 🎨 Image generation support

## 🛠 Development Setup

### Prerequisites
- Node.js 18+ 
- Python 3.11+
- Git

### Quick Start
```bash
# Install frontend dependencies
npm install --legacy-peer-deps

# Install backend dependencies  
cd backend
python3 -m pip install -r requirements.txt

# Start backend server
python3 -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080

# In another terminal, start frontend
npm run dev
```

### Access Points
- Frontend: http://localhost:5173
- Backend API: http://localhost:8080
- API Docs: http://localhost:8080/docs

## 📁 Project Structure
```
├── src/                 # Frontend Svelte components
├── backend/            # Python FastAPI backend
├── static/             # Static assets
├── docs/               # Documentation
└── scripts/            # Build and utility scripts
```

## 🎨 Customization

### Branding
- Update logos in `static/` directory
- Modify colors in `tailwind.config.js`
- Customize components in `src/lib/components/`

### Features
- Add new API endpoints in `backend/open_webui/routers/`
- Create custom frontend components in `src/lib/components/`
- Extend database models in `backend/open_webui/models/`

## 🚢 Deployment

### Docker
```bash
# Build and run with Docker
docker build -t your-startup-ai .
docker run -p 3000:8080 your-startup-ai
```

### Manual Deployment
1. Build frontend: `npm run build`
2. Set environment variables
3. Deploy backend with your preferred method

## 📝 License

This project is based on OpenWebUI. See the original LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

**Built with ❤️ for your startup journey**