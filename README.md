# AgentX-VoiceActor

A comprehensive legal document analysis platform combining a multi-agent AI backend with an intuitive Next.js frontend

## 🎯 Project Overview

This system consists of two main components:
- **Agent Backend**: Multi-agent system for analyzing legal documents
- **Client Frontend**: Next.js web application for contract upload, risk visualization, and negotiation tips

## 🏗️ Project Structure

```
AgentX-VoiceActor/
├── agent/
│   ├── local_agent_team.py      # local Streamlit app
│   ├── agent_team.py            # Main Streamlit app that uses open ai api, qdrant, agno
│   ├── requirements.txt         # Python dependencies
│   ├── …                       # Agents, knowledge‐base, Qdrant integration, tools, etc.
├── client/
│   ├── pages/                  # Next.js routes
│   ├── components/             # React components & UI widgets
│   ├── public/                 # Images, fonts, favicon, etc.
│   ├── styles/                 # Tailwind CSS or custom CSS
│   └── package.json            # Node.js dependencies
└── README.md                   # Project overview (this file)

```

## ✨ Key Features

### Backend (Agent)

- **Multi-Agent Team**:
   - Legal Researcher: Searches external/legal references (DuckDuckGo, Qdrant)
   - Contract Analyst: Reviews contracts for clauses/issues
   - Legal Strategist: Develops negotiation strategies and mitigation advice
   - Team Coordinator: Orchestrates the above agents and aggregates results
- **Multiple Analysis Types**:
   - Contract Review
   - Legal Research
   - Risk Assessment (based on PRAC³, NIST, OWASP, etc.)
   - Custom Query
- **Vector Database Storage**:
   - Qdrant (via Agno’s Qdrant wrapper) for embedding-powered document search
   - PDF documents are chunked, embedded (OpenAIEmbedder), and stored in Qdrant


### Frontend (Client)
- **Drag-and-Drop Upload**:
   - PDF contract upload UI
- **Multi-Step Analysis Workflow**:
   - Select analysis type (Contract Review, Legal Research, Risk Assessment, Custom Query)
   - View dynamic tabs: Detailed Analysis, Key Points, Recommendations (or Risk Review + Scoring for Risk Assessment)
- **Interactive Highlights & Risk Scoring**:
   - Highlighted clauses in the PDF
   - Risk scoring tables (for “Risk Assessment” flow)
- **Negotiation Guidance**:
   - Talking points and best-practice clause suggestions

## 🚀 Quick Start

### Prerequisites

- **Python 3.8+** (or newer)
- **Node.js 16+** (or newer)
- **Docker** (for running Qdrant locally)
- **Git**

### 1. Clone the Repository

```bash
git clone https://github.com/GPS-Research-Lab/2025-AgentX-Hackathon.git
cd 2025-AgentX-Hackathon
```

### 2. Backend Setup (Agent)

Choose between local AI processing or external API usage:

#### Option A: Local AI Processing (Recommended for Privacy)

```bash
# Navigate to agent directory
cd agent

# Install Python dependencies
pip install -r requirements.txt

# Install and setup Ollama
# Visit https://ollama.com/download for installation instructions

# Pull required AI models
ollama pull qwen2.5:7b      # For LLM agents
ollama pull openhermes      # For embeddings

# Start Qdrant vector database
docker pull qdrant/qdrant
docker run -p 6333:6333 -p 6334:6334 \
    -v "$(pwd)/qdrant_storage:/qdrant/storage:z" \
    qdrant/qdrant

# Start the backend server
streamlit run local_agent_team.py

# You can also run agent_team.py that uses  external apis
streamlit run agent_team.py
```

The backend will be available at `http://localhost:8501`

#### Option B: External APIs (Faster but requires API keys)

```bash
# Navigate to agent directory
cd agent

# Install Python dependencies
pip install -r requirements.txt

# Start the API server
python3 api_server.py
```

**Note**: For external APIs, ensure you have configured your API keys in the environment or configuration files.

### 3. Frontend Setup (Client)

```bash
# Open a new terminal and navigate to client directory
cd client

# Install Node.js dependencies
npm install

# Create environment configuration
cp .env.example .env.local
# Edit .env.local with your configuration

# Start the development server
npm run dev
```

The frontend will be available at `http://localhost:3000`

## ⚙️ Configuration

### Backend Configuration

#### For Local AI Processing:
- Ollama models: `qwen2.5:7b` and `openhermes`
- Qdrant vector database running on port 6333

#### For External APIs:
- Configure API keys in your environment or configuration files
- No local models or vector database required

### Frontend Configuration
Create `.env.local` in the client directory:

```env
NEXT_PUBLIC_API_URL=http://localhost:8501
```

## 📖 Usage

1. **Start Both Services**: Ensure both backend (port 8501) and frontend (port 3000) are running
2. **Upload Contract**: Use the frontend to drag-and-drop your PDF contract
3. **Select Analysis Type**: Choose from Contract Review, Risk Assessment, Legal Research, or Custom Query
4. **Review Results**:
    - View risk scores and highlighted clauses in the frontend
    - Access detailed agent analysis in the backend interface
5. **Get Negotiation Tips**: Use AI-powered suggestions for contract improvements

## 🛠️ Development

### Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request to the `develop` branch

## 🔧 Troubleshooting

### Backend Issues
#### Local AI Setup:
- **Model not found error**: Ensure you've pulled the correct models with `ollama pull`
- **Connection error**: Verify Qdrant server is running on port 6333
- **Embedding error**: Check that you're using compatible embedding models

#### External API Setup:
- **API connection failed**: Verify your API keys are correctly configured
- **Rate limiting**: Check if you've exceeded your API quota
- **Authentication error**: Ensure API keys are valid and have proper permissions

### Frontend Issues
- **API connection failed**: Verify backend is running on port 8501
- **Environment variables**: Ensure `.env.local` is properly configured
- **PDF rendering issues**: Check PDF.js compatibility with your document format

### Common Solutions
- Restart both services if experiencing connection issues
- Check that all required ports (3000, 6333, 8501) are available
- Verify Docker is running for Qdrant database

## 📋 Requirements

### Backend Requirements
- Python 3.8+
- Ollama with qwen2.5:7b and openhermes models
- Qdrant vector database
- Required Python packages (see agent/requirements.txt)

### Frontend Requirements
- Node.js 16+
- npm or yarn
- Modern web browser with PDF.js support
