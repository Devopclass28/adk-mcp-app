# 🍸 ADK MCP Cocktail Application

A sophisticated web application that combines **Google's Agent Development Kit (ADK)** with **Model Context Protocol (MCP)** to create an intelligent cocktail recommendation system. This application demonstrates how to integrate AI agents with external APIs through MCP tools.

## 🎯 Use Case & Purpose

This application serves as a **practical example** of building AI-powered applications that can:

- **Interact with external APIs** through MCP (Model Context Protocol)
- **Provide intelligent responses** using Google's Gemini AI model
- **Handle real-time communication** via WebSocket connections
- **Serve as a foundation** for more complex AI agent applications

### Real-World Applications:
- **Restaurant/Hospitality**: Cocktail menu recommendations and ingredient suggestions
- **Educational**: Learning about mixology and cocktail recipes
- **Entertainment**: Interactive cocktail discovery and trivia
- **Business**: API integration patterns for AI applications

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Web Browser   │◄──►│   FastAPI App   │◄──►│   MCP Server    │
│   (Frontend)    │    │   (main.py)     │    │ (cocktail.py)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                              │                        │
                              ▼                        ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │  ADK Agent      │    │ TheCocktailDB   │
                       │  (Gemini AI)    │    │     API         │
                       └─────────────────┘    └─────────────────┘
```

## 📁 Project Structure

```
adk_mcp_app/
├── main.py                 # FastAPI web application with ADK integration
├── mcp_server/
│   └── cocktail.py        # MCP server with cocktail API tools
├── static/
│   └── index.html         # Web frontend with WebSocket client
├── requirements.txt       # Python dependencies
└── README.md             # This documentation
```

## 🔧 Core Components

### 1. **main.py** - FastAPI Web Application

**Purpose**: The main web application that orchestrates everything.

**Key Features**:
- **FastAPI Web Server**: Handles HTTP requests and WebSocket connections
- **ADK Agent Integration**: Creates and manages Google Gemini AI agents
- **MCP Tool Integration**: Connects to the MCP server for cocktail tools
- **Session Management**: Handles user sessions and conversation history
- **Error Handling**: Graceful fallbacks when MCP tools are unavailable

**Main Components**:
```python
# Core Services
session_service = InMemorySessionService()      # Manages user sessions
artifacts_service = InMemoryArtifactService()   # Handles artifacts
ct_server_params = StdioServerParameters(...)   # MCP server connection

# Agent Creation
def create_agent(server_params):                # Creates ADK agent with MCP tools
    mcp_toolset = MCPToolset(...)              # MCP tool integration
    root_agent = LlmAgent(...)                 # Gemini AI agent

# WebSocket Handler
async def run_adk_agent_session(...):          # Real-time communication
```

### 2. **mcp_server/cocktail.py** - MCP Server

**Purpose**: Provides cocktail-related tools to the AI agent through MCP protocol.

**Key Features**:
- **FastMCP Server**: Implements MCP protocol for tool communication
- **TheCocktailDB API Integration**: Fetches real cocktail data
- **Tool Functions**: 5 specialized cocktail tools
- **Error Handling**: Robust API error management
- **Data Formatting**: Clean, readable output formatting

**Available Tools**:
```python
@mcp.tool()
async def search_cocktail_by_name(name: str) -> str:
    """Search cocktails by name (e.g., 'margarita')"""

@mcp.tool()
async def list_cocktails_by_first_letter(letter: str) -> str:
    """List cocktails starting with a specific letter"""

@mcp.tool()
async def search_ingredient_by_name(name: str) -> str:
    """Search for ingredient information (e.g., 'vodka')"""

@mcp.tool()
async def list_random_cocktails() -> str:
    """Get a random cocktail recipe"""

@mcp.tool()
async def lookup_cocktail_details_by_id(cocktail_id: str) -> str:
    """Get detailed cocktail information by ID"""
```

### 3. **static/index.html** - Web Frontend

**Purpose**: User interface for interacting with the AI agent.

**Key Features**:
- **WebSocket Client**: Real-time communication with the backend
- **Markdown Rendering**: Displays formatted AI responses
- **Connection Management**: Auto-reconnection and error handling
- **Responsive Design**: Clean, modern interface
- **Message History**: Persistent conversation display

## 🚀 Getting Started

### Prerequisites

- **Python 3.8+** (Python 3.12 recommended)
- **Virtual Environment** (recommended)
- **Internet Connection** (for TheCocktailDB API)

### Installation Steps

1. **Clone or Download the Project**
   ```bash
   # If using git
   git clone <repository-url>
   cd adk_mcp_app
   
   # Or simply navigate to your project directory
   cd path/to/adk_mcp_app
   ```

2. **Create Virtual Environment**
   ```bash
   # Windows
   python -m venv venv
   venv\Scripts\activate
   
   # macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set Up Environment Variables** (Optional)
   ```bash
   # Create .env file if needed
   echo "GOOGLE_API_KEY=your_api_key_here" > .env
   ```

5. **Run the Application**
   ```bash
   uvicorn main:app --reload
   ```

6. **Access the Application**
   - Open your browser
   - Navigate to `http://127.0.0.1:8000`
   - Start chatting with the AI agent!

## 🔍 Why `uvicorn main:app --reload`?

### Understanding the Command:

```bash
uvicorn main:app --reload
```

**Breaking it down**:

- **`uvicorn`**: ASGI (Asynchronous Server Gateway Interface) server
  - Handles HTTP requests and WebSocket connections
  - Optimized for async Python applications
  - Production-ready server

- **`main:app`**: Module and application reference
  - **`main`**: Refers to `main.py` file
  - **`app`**: Refers to the FastAPI application instance (`app = FastAPI()`)
  - This tells uvicorn where to find the web application

- **`--reload`**: Development mode
  - Automatically restarts server when code changes
  - Enables hot-reloading for faster development
  - **Remove this flag for production deployment**

### Alternative Commands:

```bash
# Development (with auto-reload)
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Production (without reload)
uvicorn main:app --host 0.0.0.0 --port 8000

# With specific workers (production)
uvicorn main:app --workers 4 --host 0.0.0.0 --port 8000
```

## 🛠️ MCP (Model Context Protocol) Explained

### What is MCP?

**MCP** is a protocol that allows AI agents to use external tools and data sources. It's like giving the AI agent a "toolbox" of functions it can call.

### How MCP Works in This Application:

1. **MCP Server** (`cocktail.py`): Provides cocktail-related tools
2. **MCP Client** (`main.py`): Connects to the server and makes tools available to the AI
3. **AI Agent**: Can call these tools when users ask cocktail-related questions

### MCP Communication Flow:

```
User Question → AI Agent → MCP Tool → TheCocktailDB API → Response → AI Agent → User
```

### Example MCP Tool Usage:

```python
# When user asks: "What's in a margarita?"
# The AI agent can call:
result = await search_cocktail_by_name("margarita")
# This fetches real data from TheCocktailDB API
```

## 🤖 ADK (Agent Development Kit) Explained

### What is ADK?

**Google's Agent Development Kit** is a framework for building AI agents that can:
- Use multiple tools and data sources
- Maintain conversation context
- Handle complex multi-step tasks
- Integrate with various APIs and services

### ADK Components in This Application:

1. **LlmAgent**: The main AI agent powered by Gemini
2. **Runner**: Manages agent execution and session handling
3. **SessionService**: Maintains conversation history
4. **ArtifactService**: Handles generated content and files

### Agent Configuration:

```python
root_agent = LlmAgent(
    model="gemini-2.5-flash",           # AI model to use
    name="ai_assistant",                # Agent name
    instruction=agent_instruction,      # System prompt
    tools=[mcp_toolset],               # Available tools
)
```

## 💡 Usage Examples

### Example Conversations:

**User**: "What cocktails can I make with vodka?"
**AI**: *Uses `search_ingredient_by_name("vodka")` tool to find vodka-based cocktails*

**User**: "Show me a random cocktail recipe"
**AI**: *Uses `list_random_cocktails()` tool to get a random recipe*

**User**: "Tell me about margaritas"
**AI**: *Uses `search_cocktail_by_name("margarita")` tool to get detailed information*

**User**: "What cocktails start with 'M'?"
**AI**: *Uses `list_cocktails_by_first_letter("M")` tool to list cocktails*

## 🔧 Configuration & Customization

### Environment Variables:

Create a `.env` file for configuration:

```env
# Google API Configuration
GOOGLE_API_KEY=your_google_api_key_here

# Server Configuration
HOST=127.0.0.1
PORT=8000

# MCP Configuration
MCP_TIMEOUT=30.0
```

### Customizing the AI Agent:

Edit the agent instruction in `main.py`:

```python
agent_instruction = """You are a cocktail expert AI assistant. 
Your role is to help users discover and learn about cocktails.
- Always provide accurate information from the cocktail database
- Suggest variations and alternatives when appropriate
- Explain ingredients and techniques clearly
- Be friendly and enthusiastic about mixology
"""
```

### Adding New MCP Tools:

1. **Add tool function** in `mcp_server/cocktail.py`:
```python
@mcp.tool()
async def your_new_tool(param: str) -> str:
    """Description of what this tool does."""
    # Your implementation here
    return result
```

2. **Restart the application** to load new tools

## 🐛 Troubleshooting

### Common Issues:

1. **MCP Timeout Errors**:
   - Increase timeout in `main.py`: `timeout=60.0`
   - Check internet connection for API calls

2. **Import Errors**:
   - Ensure virtual environment is activated
   - Run `pip install -r requirements.txt`

3. **WebSocket Connection Issues**:
   - Check if port 8000 is available
   - Try different port: `uvicorn main:app --reload --port 8001`

4. **API Rate Limits**:
   - TheCocktailDB has rate limits
   - Add delays between requests if needed

### Debug Mode:

Enable detailed logging:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## 🚀 Deployment

### Production Deployment:

1. **Remove development flags**:
   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
   ```

2. **Use production ASGI server**:
   ```bash
   pip install gunicorn
   gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
   ```

3. **Set up reverse proxy** (nginx/Apache)

4. **Configure environment variables** for production

## 📚 Learning Resources

### Key Technologies:
- **FastAPI**: https://fastapi.tiangolo.com/
- **Google ADK**: https://github.com/google/adk
- **MCP Protocol**: https://modelcontextprotocol.io/
- **TheCocktailDB API**: https://www.thecocktaildb.com/api.php

### Next Steps:
- Add more MCP tools (weather, news, etc.)
- Implement user authentication
- Add database for conversation history
- Create mobile app interface
- Deploy to cloud platforms

## 📄 License

This project is for educational purposes. Please respect TheCocktailDB API terms of service.

## 🤝 Contributing

Feel free to submit issues, feature requests, or pull requests to improve this application!

---

**Happy Mixing! 🍸✨**
