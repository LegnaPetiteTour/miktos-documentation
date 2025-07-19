# 🚀 Miktos Development - Getting Started

> **Quick setup guide** for local development of the Miktos AI-first creative platform

## 📋 Prerequisites

Before starting development, ensure you have the following installed:

### **Required Software**
- **Node.js** 18+ and npm
- **Rust** 1.70+ and Cargo (for Tauri)
- **Python** 3.9+ with pip
- **Git** for version control

### **Development Tools**
- **VS Code** (recommended IDE)
- **Tauri CLI**: `npm install -g @tauri-apps/cli`
- **Python Virtual Environment**: `python -m venv`

## 🛠️ Quick Setup

### **1. Desktop Application (Tauri + React)**

```bash
# Navigate to desktop app
cd miktos-desktop

# Install dependencies
npm install

# Start development server
npm run tauri:dev
```

### **2. AI Bridge (Python + FastAPI)**

```bash
# Navigate to AI bridge
cd miktos-ai-bridge

# Create virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Start AI bridge server
python main.py
```

### **3. Verify Setup**

1. **Desktop App**: Should open Miktos application window
2. **AI Bridge**: Should show "AI Bridge Connected" in status bar
3. **API**: Visit http://localhost:8000 to see AI Bridge API

## 📁 Project Structure

```
Miktos-Development/
├── miktos-desktop/           # Main desktop application
│   ├── src/                 # React frontend source
│   ├── src-tauri/           # Tauri backend source
│   └── package.json         # Node.js dependencies
├── miktos-ai-bridge/        # AI bridge and ComfyUI integration
│   ├── src/                 # Python source code
│   ├── requirements.txt     # Python dependencies
│   └── main.py             # FastAPI server entry point
├── miktos-workflows/        # AI workflow templates
├── docs/                    # Development documentation
└── tools/                   # Development tools and scripts
```

## 🎯 Current Development Status

### **Phase 1: Foundation (Active)**
- [x] **Project Structure**: Complete workspace setup
- [x] **Desktop App**: Tauri project with React frontend
- [x] **AI Bridge**: Python FastAPI server architecture
- [ ] **ComfyUI Integration**: Embedding ComfyUI (In Progress)
- [ ] **Blender Connector**: Basic 3D software integration
- [ ] **Basic AI Features**: Text-to-texture generation

### **Next Steps**
1. **Complete ComfyUI Integration**: Embed ComfyUI in desktop app
2. **Basic Texture Generation**: Implement SDXL workflow
3. **Blender Integration**: Real-time texture application
4. **Natural Language Processing**: Add LLM command parsing

## 🔧 Development Workflow

### **Daily Development**
1. **Start AI Bridge**: `cd miktos-ai-bridge && python main.py`
2. **Start Desktop App**: `cd miktos-desktop && npm run tauri:dev`
3. **Test Integration**: Verify AI Bridge connection in app
4. **Commit Changes**: Regular commits to respective GitHub repositories

### **Testing**
- **Desktop App**: Hot reload with Vite
- **AI Bridge**: FastAPI auto-reload on code changes
- **Integration**: Test AI commands through desktop interface

## 📚 Key Files to Know

### **Desktop App**
- `src/App.tsx` - Main React application
- `src/components/` - React components
- `src-tauri/src/main.rs` - Tauri backend
- `src-tauri/tauri.conf.json` - Tauri configuration

### **AI Bridge**
- `main.py` - FastAPI server entry point
- `src/core/bridge.py` - Main AI bridge logic
- `src/comfyui/` - ComfyUI integration
- `src/connectors/` - 3D software connectors

## 🚨 Common Issues

### **Desktop App Won't Start**
- Check if Tauri CLI is installed: `npm install -g @tauri-apps/cli`
- Verify Rust is installed: `rustc --version`
- Try `npm install` in miktos-desktop folder

### **AI Bridge Connection Failed**
- Check if Python virtual environment is activated
- Verify all dependencies installed: `pip install -r requirements.txt`
- Check if port 8000 is available

### **ComfyUI Integration Issues**
- ComfyUI integration is still in development
- Check logs in terminal for specific error messages
- Refer to ComfyUI documentation for troubleshooting

## 📞 Getting Help

- **GitHub Issues**: Report bugs and feature requests
- **Project Hub**: https://github.com/Miktos-Universe/miktos-project-hub
- **Development Docs**: Check `/docs` folder for detailed documentation

## 🎉 Success Criteria

**You're ready to develop when:**
- [x] Desktop app opens without errors
- [x] AI Bridge shows "connected" status
- [x] Can type commands in AI input bar
- [ ] Basic texture generation works
- [ ] Can apply textures to Blender objects

---

**Happy coding! You're now ready to build the future of AI-powered 3D creativity!** 🚀