# Miktos Platform Architecture

## Overview

Miktos is an AI-first creative platform designed for 3D animation and content generation. The platform consists of multiple interconnected components that work together to provide a seamless creative workflow experience.

## System Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                    Miktos Platform                          │
├─────────────────┬─────────────────┬─────────────────────────┤
│   Desktop App   │   AI Bridge     │    Workflow Engine     │
│   (Frontend)    │   (Backend)     │    (ComfyUI)           │
│                 │                 │                         │
│  ┌───────────┐  │  ┌───────────┐  │  ┌───────────────────┐ │
│  │  React    │  │  │  FastAPI  │  │  │    ComfyUI        │ │
│  │TypeScript │  │  │  Python   │  │  │    Workflows      │ │
│  │  Tauri    │  │  │  Async    │  │  │    Custom Nodes   │ │
│  └───────────┘  │  └───────────┘  │  └───────────────────┘ │
└─────────────────┴─────────────────┴─────────────────────────┘
         │                   │                   │
         │        HTTP/WS    │       Python      │
         └───────────────────┼───────────────────┘
                             │
                    ┌────────▼────────┐
                    │   File System   │
                    │   Models        │
                    │   Workflows     │
                    │   Outputs       │
                    └─────────────────┘
```

## Component Architecture

### 1. Desktop Application (miktos-desktop)

**Technology Stack:**

- **Frontend**: React 18 with TypeScript
- **Backend**: Tauri (Rust)
- **Styling**: Tailwind CSS
- **Animation**: Framer Motion
- **Icons**: Lucide React

**Key Components:**

#### Frontend Layer

```text
src/
├── App.tsx                 # Main application component
├── components/            
│   ├── AICommandBar.tsx   # Natural language AI interface
│   ├── Sidebar.tsx        # Navigation and project management
│   ├── StatusBar.tsx      # System status and monitoring
│   └── WorkflowCanvas.tsx # Visual workflow builder
├── services/              # API integrations and utilities
└── store/                 # State management
```

#### Backend Layer (Tauri)

```text
src-tauri/
├── src/
│   └── main.rs           # Rust backend with system integration
├── Cargo.toml            # Rust dependencies
└── tauri.conf.json       # App configuration and permissions
```

**Responsibilities:**

- User interface and experience
- Real-time AI Bridge communication
- Workflow visualization and editing
- System status monitoring
- File system integration
- Cross-platform desktop functionality

### 2. AI Bridge (miktos-ai-bridge)

**Technology Stack:**

- **Framework**: FastAPI (Python 3.11+)
- **Async Processing**: asyncio, Celery
- **API Documentation**: OpenAPI/Swagger
- **Task Queue**: Redis (optional)
- **Logging**: Python logging with structured output

**Architecture:**

```text
src/
├── api/                   # FastAPI route handlers
│   ├── v1/               # API version 1
│   │   ├── status.py     # Health and status endpoints
│   │   ├── commands.py   # AI command processing
│   │   ├── workflows.py  # Workflow management
│   │   └── tasks.py      # Task monitoring
├── core/                 # Core business logic
│   ├── bridge.py         # Main bridge orchestrator
│   ├── config.py         # Configuration management
│   └── models.py         # Data models and schemas
├── comfyui/              # ComfyUI integration
│   ├── client.py         # ComfyUI API client
│   ├── workflows.py      # Workflow management
│   └── nodes.py          # Custom node definitions
├── connectors/           # External service integrations
└── workflows/            # Workflow templates and definitions
```

**Responsibilities:**

- API gateway for AI operations
- ComfyUI integration and management
- Workflow execution orchestration
- Task queue management
- Progress monitoring and reporting
- Error handling and recovery

### 3. Workflow Engine (ComfyUI Integration)

**Technology Stack:**

- **Core**: ComfyUI (Python)
- **Custom Nodes**: Python plugins
- **Models**: Stable Diffusion, ControlNet, etc.
- **Storage**: Local file system

**Integration Points:**

- HTTP API for workflow submission
- WebSocket for real-time progress updates
- File system for model and output management
- Custom nodes for Miktos-specific functionality

## Data Flow Architecture

### 1. Command Processing Flow

```text
User Input → Desktop App → AI Bridge → ComfyUI → Output
    ↓           ↓            ↓          ↓         ↓
Natural      Parse &     Queue      Execute    Generate
Language   → Validate  → Task    → Workflow → Content
Command      Command     Job        Pipeline    Files
```

### 2. Real-time Communication

```text
Desktop App ←→ AI Bridge ←→ ComfyUI
     ↓             ↓           ↓
WebSocket    HTTP/WebSocket  Python API
Updates      Status &         Direct
             Progress         Integration
```

### 3. State Management

```text
┌─────────────────────────────────────────────┐
│              Application State              │
├─────────────────┬───────────────────────────┤
│  Frontend State │      Backend State        │
│                 │                           │
│ • UI State      │ • Task Queue             │
│ • User Prefs    │ • Active Workflows       │
│ • Connection    │ • System Resources       │
│ • Progress      │ • Error States           │
└─────────────────┴───────────────────────────┘
```

## API Architecture

### RESTful API Design

**Base URL**: `http://localhost:8000/api/v1`

**Core Endpoints:**

```text
GET    /status                    # System health and status
POST   /execute-command           # Execute natural language commands
GET    /workflows                 # List available workflows
POST   /workflows/{id}/execute    # Execute specific workflow
GET    /task/{id}                 # Get task status and progress
GET    /models                    # List available AI models
POST   /models/install           # Install new models
```

**Response Format:**

```json
{
  "status": "success|error",
  "data": { /* response data */ },
  "message": "Human readable message",
  "timestamp": "2025-01-XX 12:00:00",
  "request_id": "uuid"
}
```

### WebSocket Communication

**Connection**: `ws://localhost:8000/ws`

**Message Types:**

- `status_update`: System status changes
- `task_progress`: Task execution progress
- `workflow_complete`: Workflow completion notification
- `error`: Error notifications

## Security Architecture

### 1. Desktop Application Security

- **Tauri Security**: Restricted API access and CSP policies
- **Input Validation**: All user inputs validated on frontend and backend
- **File System**: Controlled access through Tauri's secure APIs
- **Network**: HTTPS/WSS for all external communications

### 2. API Security

- **Authentication**: API key based authentication (future)
- **Rate Limiting**: Request throttling to prevent abuse
- **Input Sanitization**: All inputs validated and sanitized
- **CORS**: Configured for localhost development

### 3. Data Security

- **Local Processing**: All AI processing happens locally
- **No Cloud Dependencies**: Optional cloud features only
- **File Permissions**: Proper file system permissions
- **Secrets Management**: Environment-based configuration

## Performance Architecture

### 1. Frontend Performance

- **Code Splitting**: Lazy loading of components
- **Memoization**: React.memo and useMemo for expensive operations
- **Virtual Scrolling**: For large lists and canvases
- **Debounced Updates**: Status updates and API calls

### 2. Backend Performance

- **Async Processing**: Non-blocking I/O operations
- **Task Queue**: Background processing for long-running operations
- **Connection Pooling**: Efficient resource management
- **Caching**: Response caching where appropriate

### 3. Resource Management

- **Memory**: Efficient memory usage with cleanup
- **CPU**: Multi-core utilization for AI processing
- **Storage**: Optimized file I/O and cleanup
- **Network**: Efficient API communication

## Deployment Architecture

### Development Environment

```text
Local Machine
├── Node.js 18+ (Desktop App)
├── Python 3.11+ (AI Bridge)
├── Rust 1.70+ (Tauri Backend)
└── ComfyUI (AI Engine)
```

### Production Distribution

```text
Desktop Application
├── Windows (.msi installer)
├── macOS (.dmg bundle)
└── Linux (.AppImage)

AI Bridge
├── Python Package (pip installable)
├── Docker Container (optional)
└── Standalone Executable (future)
```

## Extension Architecture

### 1. Plugin System

- **Custom Nodes**: ComfyUI node development
- **Workflow Templates**: JSON-based workflow definitions
- **UI Extensions**: React component extensions
- **API Extensions**: FastAPI route plugins

### 2. Integration Points

- **File System**: Custom import/export handlers
- **External APIs**: Third-party service integrations
- **Model Support**: Custom model loaders
- **Rendering**: Custom output formats

## Monitoring and Observability

### 1. Logging

- **Structured Logging**: JSON-formatted logs
- **Log Levels**: DEBUG, INFO, WARN, ERROR
- **Log Rotation**: Automatic log file management
- **Performance Metrics**: Execution time tracking

### 2. Health Monitoring

- **System Health**: CPU, memory, disk usage
- **Service Health**: AI Bridge and ComfyUI status
- **Connection Health**: Network connectivity monitoring
- **Performance Metrics**: Response times and throughput

### 3. Error Tracking

- **Error Reporting**: Comprehensive error logging
- **Recovery Mechanisms**: Automatic retry and fallback
- **User Notifications**: User-friendly error messages
- **Debug Information**: Detailed technical information

## Future Architecture Considerations

### 1. Scalability

- **Distributed Processing**: Multi-machine AI processing
- **Cloud Integration**: Optional cloud AI services
- **Load Balancing**: Multiple AI Bridge instances
- **Microservices**: Service decomposition

### 2. Collaboration

- **Real-time Collaboration**: Multiple users on projects
- **Version Control**: Project versioning and history
- **Sharing**: Workflow and project sharing
- **Cloud Storage**: Optional cloud project storage

### 3. Advanced Features

- **Plugin Marketplace**: Community extensions
- **Advanced AI**: Custom model training
- **Enterprise Features**: Team management and analytics
- **Mobile Support**: Mobile companion apps

## Technology Decisions

### Why These Technologies?

1. **Tauri + React**: Native performance with web development speed
2. **FastAPI**: Modern Python framework with excellent async support
3. **ComfyUI**: Mature and extensible AI workflow system
4. **TypeScript**: Type safety and better developer experience
5. **Tailwind CSS**: Rapid UI development with consistency

### Alternative Considerations

- **Electron vs Tauri**: Chose Tauri for better performance and security
- **Django vs FastAPI**: Chose FastAPI for modern async capabilities
- **Automatic1111 vs ComfyUI**: Chose ComfyUI for workflow flexibility
- **Vue vs React**: Chose React for ecosystem and familiarity
- **SCSS vs Tailwind**: Chose Tailwind for rapid development

This architecture provides a solid foundation for the Miktos platform while maintaining flexibility for future enhancements and scalability requirements.
