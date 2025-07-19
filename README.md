# Torch CLI (`@theaiinc/torch`)

**Torch** is a powerful, standalone command-line interface for our AI coding agent. It allows developers to interact with the agent for various development tasks, from generating code to performing deep research. The CLI can also be run in an "MCP (Model Context Protocol) Mode," allowing external clients, such as a custom chat UI, to interact with the agent programmatically.

## 🚀 Features

### Core Capabilities

- **AI-Powered Code Generation**: Generate, modify, and refactor code using state-of-the-art LLMs
- **Interactive Development**: Work with the agent in real-time through natural language
- **File System Operations**: Read, write, and manage files with intelligent context awareness
- **Shell Command Execution**: Execute shell commands with safety checks and workspace constraints
- **Git Integration**: Automatic version control with intelligent commit messages and branch management
- **Multi-Platform Support**: Pre-compiled binaries for Windows x64, macOS ARM64 (Apple Silicon), and Linux x64

### Advanced Features

- **Virtual File System**: In-memory file representation with `.torchignore` support
- **Isolated Workspaces**: Each task gets its own isolated workspace for safety
- **Environment Configuration**: `.torchenv` files for project-specific settings
- **Semantic Context**: Intelligent code analysis and context retrieval
- **Memory Persistence**: Session-based memory for continuous conversations
- **MCP Server Mode**: HTTP server for programmatic access via chat UIs

## 📦 Installation

You can install the CLI globally via npm:

```bash
npm install -g @theaiinc/torch
```

## 🎯 Quick Start

### Basic Usage

```bash
# See all available commands and options
torch --help

# Start the agent with a simple instruction
torch --instruction "Create a Python script that prints 'Hello, World!'"

# Use a script file for complex tasks
torch --script ./my_complex_task.txt
```

### Interactive Mode

```bash
# Start interactive mode with your preferred LLM
torch --provider google --model gemini-pro --interactive
```

## 🔧 Configuration

### Environment Variables (`.torchenv`)

Create a `.torchenv` file in your project root to configure Torch behavior:

```bash
# .torchenv
# --- Debugging and Agent Behavior ---
TORCH_DEBUG=True
TORCH_MAX_ITERATIONS=50
TORCH_SAVE_MEMORY=True

# --- Caching ---
TORCH_USE_EMBEDDING_CACHE=True

# --- LLM Configuration ---
TORCH_PROVIDER="openai"
TORCH_MODEL="gpt-4-turbo"
OPENAI_API_KEY="sk-..."

# Alternative providers:
# Google Gemini
TORCH_PROVIDER="google"
TORCH_MODEL="gemini-2.5-flash-preview-05-20"
GOOGLE_GEMINI_API_KEY="your-google-api-key"

# Anthropic
TORCH_PROVIDER="anthropic"
TORCH_MODEL="claude-3-sonnet-20240229"
ANTHROPIC_API_KEY="sk-ant-..."

# Ollama (local)
TORCH_PROVIDER="ollama"
TORCH_MODEL="codellama"
TORCH_BASE_URL="http://localhost:11434"
```

### File Ignoring (`.torchignore`)

Create a `.torchignore` file to exclude files and directories from Torch's virtual file system:

```bash
# .torchignore
# Dependencies
node_modules/
venv/
__pycache__/

# Build artifacts
dist/
build/
*.egg-info/

# Logs and temp files
*.log
*.tmp
.DS_Store

# Large files
*.bin
*.model
*.weights

# Sensitive files
.env
*.key
*.pem
```

## 🏗️ Virtual Workspace System

### Isolated Task Workspaces

Each task initiated via MCP gets its own isolated workspace within the agent's data directory. This provides:

- **Safety**: Commands are constrained to the workspace
- **Isolation**: Tasks don't interfere with each other
- **Cleanup**: Automatic cleanup of temporary files
- **Version Control**: Each workspace can have its own Git repository

### Working with Existing Repositories

To work on an existing repository:

1. **Specify the path in your instruction**:

   ```bash
   torch --instruction "Work on the project at /path/to/my/repo"
   ```

2. **Clone into workspace**:
   ```bash
   torch --instruction "Clone https://github.com/user/repo and work on it"
   ```

### Virtual File System

Torch uses an in-memory virtual file system that:

- Loads all non-ignored files into memory
- Respects `.torchignore` patterns
- Provides fast read operations
- Updates both memory and disk on writes
- Automatically rescans when file structure changes

## 🔌 MCP Mode for UI Integration

### Starting MCP Server

```bash
# Start the agent in MCP mode
torch --mcp --mcp-host 127.0.0.1 --mcp-port 7878

# With specific LLM configuration
GOOGLE_API_KEY="AIza..." torch --mcp --provider google --model gemini-pro
```

### MCP Configuration

- **Authentication**: Currently no API key required (manage access at network level)
- **Workspace Isolation**: Each task gets its own workspace directory
- **Environment Inheritance**: MCP server processes inherit environment variables
- **File Permissions**: Agent's file system permissions apply to all operations

### Example: Chat UI Integration

1. **User in Chat UI**: "Create a new Python Flask project named 'my-api'."
2. **Chat UI sends to MCP**:
   ```json
   {
     "instruction": "Create a new Python Flask project named 'my-api'. Initialize a git repository, create a basic app.py with a hello world route, and a requirements.txt with Flask.",
     "agent_config": { "provider": "openai", "model": "gpt-4-turbo" }
   }
   ```
3. **Agent creates isolated workspace and executes**:
   - `mkdir my-api`
   - `cd my-api`
   - `git init`
   - Creates `app.py` and `requirements.txt`
   - Optionally commits changes if auto-git is enabled

## 🛠️ Available Parameters

### LLM Provider Settings

- `--provider`: LLM provider (`openai`, `azure`, `anthropic`, `ollama`, `local`, `mock`, `google`)
- `--model`: Specific model name (e.g., `gpt-4-turbo-preview`)
- `--temperature`: Generation temperature (0.0-1.0)
- `--api-key`: API key for the provider
- `--base-url`: Custom API base URL
- `--debug`: Enable debug mode

### Agent Settings

- `--memory-type`: Memory type (`buffer`, `summary`, `none`)
- `--session-id`: Session ID for memory persistence
- `--load-memory`: Path to memory file to load
- `--save-memory`: Save memory to file on exit
- `--max-iterations`: Maximum agent iterations (default: 25)
- `--headless`: Run without interactive prompts

### Task & Context Settings

- `-i`, `--instruction`, `--task`, `-p`, `--prompt`: Single instruction
- `--script`: Path to script file with detailed instructions
- `--source-dir`: Project source directory (default: current directory)
- `--image-file`, `--image-path`: Image file for vision tasks
- `--use-semantic-context`: Enable semantic context retrieval
- `--context-db-path`: Path to semantic context database
- `--deep-research`: Enable deep research capabilities
- `--search-api-key`: API key for search providers

### Git Integration

- `--use-git`: Enable Git integration
- `--auto-commit`: Automatically commit changes made by the agent

### MCP Settings

- `--mcp`: Run in MCP server mode
- `--mcp-host`: MCP server host
- `--mcp-port`: MCP server port
- `--mcp-worker`: Run in MCP worker mode
- `--mcp-master-url`: MCP master URL
- `--job-id`: Job ID for current task

### Reporting

- `--generate-report`: Generate ZIP report of agent's work
- `--report-path`: Path for generated report
- `--send-report-email`: Email address to send report to

## 🔒 Security Features

### Command Safety

- **Dangerous Command Detection**: Blocks potentially harmful commands
- **Workspace Constraints**: Commands can only run within the workspace
- **Path Validation**: Prevents directory traversal attacks
- **Timeout Protection**: Automatic timeout for long-running commands

### File System Safety

- **Virtual File System**: Isolated file operations
- **Ignore Patterns**: `.torchignore` prevents access to sensitive files
- **Permission Checks**: Respects file system permissions
- **Backup Creation**: Automatic backups before major changes

## 📁 File Operations

### Reading Files

```bash
# Read entire file
torch --instruction "Read the main.py file and explain what it does"

# Read specific lines
torch --instruction "Show me lines 10-20 of config.py"
```

### Writing Files

```bash
# Create new file
torch --instruction "Create a new file called utils.py with helper functions"

# Modify existing file
torch --instruction "Add error handling to the login function in auth.py"
```

### File Search

```bash
# Search by pattern
torch --instruction "Find all Python files that contain 'database'"

# Search by content
torch --instruction "Find files that import pandas"
```

## 🔄 Git Integration

### Automatic Git Operations

- **Auto-commit**: Automatically commit changes after successful operations
- **Branch Management**: Create task-specific branches
- **Commit Messages**: Generate intelligent commit messages based on changes
- **Conflict Resolution**: Handle merge conflicts automatically

### Manual Git Control

```bash
# Start a new task branch
torch --instruction "Create a new branch for the authentication feature"

# Commit current changes
torch --instruction "Commit the current changes with a descriptive message"

# View git status
torch --instruction "Show me the current git status and recent commits"
```

## 🧠 Memory and Context

### Session Memory

- **Buffer Memory**: Stores recent interactions in memory
- **Summary Memory**: Summarizes long conversations
- **Persistent Memory**: Save and load memory between sessions
- **Context Awareness**: Maintains context across multiple operations

### Semantic Context

- **Code Analysis**: Intelligent analysis of project structure
- **Context Retrieval**: Find relevant code based on queries
- **Embedding Cache**: Cache embeddings for faster retrieval
- **Deep Research**: Search external sources for information

## 🐳 Docker Support

### Running in Docker

```bash
# Build and run in Docker container
docker build -t torch .
docker run -it torch --instruction "Your task here"
```

### Docker Environment Detection

Torch automatically detects when running in Docker and:

- Adjusts file paths accordingly
- Uses appropriate working directories
- Handles container-specific constraints
- Manages environment variables properly

## 📊 Monitoring and Logging

### Log Files

- **Primary Log**: JSONL format with detailed agent actions
- **Stdout/Stderr**: Capture of command outputs
- **Error Logs**: Detailed error information
- **Progress Tracking**: Real-time progress updates

### Debug Mode

```bash
# Enable debug mode for verbose output
torch --debug --instruction "Your task here"
```

## 🔧 Troubleshooting

### Common Issues

**Command Blocked by Security**

```
Error: Command blocked for security reasons
```

- Check if command is within workspace
- Verify command is not in dangerous commands list
- Ensure proper file permissions

**File Not Found**

```
Error: File not found in virtual file system
```

- Check if file is ignored by `.torchignore`
- Verify file exists in physical file system
- Try rescanning filesystem with `rescan_filesystem` tool

**LLM API Errors**

```
Error: Failed to connect to LLM provider
```

- Verify API key is correct
- Check network connectivity
- Ensure provider and model are compatible

**Binary Download Issues**

If you encounter problems with binary downloads or platform compatibility:

- **Binary download fails**: Check your internet connection and verify the release exists
- **Platform not supported**: Ensure you're using a supported platform (Windows x64, macOS ARM64, Linux x64)
- **Permission denied**: The binary should be automatically made executable; try `chmod +x /path/to/torch`
- **Version mismatch**: Try reinstalling: `npm uninstall -g @theaiinc/torch && npm install -g @theaiinc/torch`

**Report Issues**

For all Torch CLI issues (binary problems, features, bugs, installation, configuration, etc.), please report them in the [torch-binaries repository](https://github.com/theaiinc/torch-binaries/issues).

### Getting Help

```bash
# Show detailed help
torch --help

# Show specific parameter help
torch --help --provider

# Enable debug mode for troubleshooting
torch --debug --instruction "Your task"
```

## 🌐 Compatible Platforms

The CLI is distributed with pre-built binaries for:

- **Windows**: x64
- **macOS**: ARM64 (Apple Silicon - M1/M2/M3)
- **Linux**: x64 (Ubuntu 22.04+)

### Platform-Specific Packages

The torch CLI is distributed as platform-specific npm packages:

- `@theaiinc/torch-win32-x64` - Windows x64
- `@theaiinc/torch-darwin-arm64` - macOS ARM64 (Apple Silicon)
- `@theaiinc/torch-linux-x64` - Linux x64

The main `@theaiinc/torch` package automatically selects the appropriate platform-specific binary during installation.

## 📚 Examples

### Web Development

```bash
# Create a React app
torch --instruction "Create a new React application with TypeScript, including routing and state management"

# Build a REST API
torch --instruction "Create a FastAPI backend with user authentication and database models"
```

### Data Science

```bash
# Data analysis script
torch --instruction "Create a Python script to analyze this CSV file and generate visualizations"

# Machine learning pipeline
torch --instruction "Build a machine learning pipeline for classification using scikit-learn"
```

### DevOps

```bash
# Docker setup
torch --instruction "Create a Dockerfile and docker-compose.yml for this application"

# CI/CD pipeline
torch --instruction "Set up GitHub Actions for automated testing and deployment"
```

## 🤝 Contributing

For development and contribution guidelines, please see the main project repository.

## 📄 License

© The AI Inc 2025. All rights reserved.

This software is available for free use with basic features. Additional features and capabilities are available through subscription plans. Please contact us for more information about licensing and subscription options.

For commercial use or advanced features, please visit our website or contact us directly.
