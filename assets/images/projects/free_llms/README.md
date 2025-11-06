# Free LLMs 🤖

<div align="center">
  <img src="public/logo.png" alt="Free LLMs Logo" width="200"/>
  
  **A Locally Deployed Web Interface for Free LLM Interactions via Ollama**
  
  [![GitHub](https://img.shields.io/badge/GitHub-rexmirak-blue)](https://github.com/rexmirak)
  [![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
  [![Next.js](https://img.shields.io/badge/Next.js-14-black)](https://nextjs.org/)
  [![TypeScript](https://img.shields.io/badge/TypeScript-5.6-blue)](https://www.typescriptlang.org/)
</div>

---

## 📖 Overview

Free LLMs is a modern, production-ready web application that provides a beautiful interface for interacting with local Large Language Models through [Ollama](https://ollama.com). Run powerful AI models completely locally on your laptop with full control over your data and conversations.

**🎯 Perfect for**: Developers, researchers, privacy advocates, and anyone who wants full control over their AI interactions without cloud dependencies or API costs.

### ✨ Key Features

#### 🎨 Interface & Design
- **WhatsApp-Inspired Layout** - Familiar 1/3-2/3 split with chat list and conversation view
- **Dark Mode** - Beautiful dark theme with customized grey assistant bubbles
- **Responsive Design** - Clean, modern interface built with Tailwind CSS
- **Real-time Streaming** - See responses as they're generated with Server-Sent Events

#### AI Capabilities
- **Multiple Model Support** - Use different models for different conversations
- **Model Variant Selector** - Choose from different sizes (1B, 3B, 8B, 70B, etc.) with RAM requirements
- **DeepSeek Reasoning Display** - Real-time thinking process shown in collapsible sections
- **Advanced Hyperparameters** - Fine-tune temperature, top-p, top-k, and max tokens per chat
- **System Messages** - Customize AI behavior with custom system prompts
- **Streaming Responses** - Real-time token-by-token response generation

#### 💾 Data Management
- **Local Storage** - All chats saved locally in JSON format (zero cloud dependencies)
- **Chat Persistence** - Automatic saving with message history
- **Unread Indicators** - Track new messages in background chats
- **Export to Markdown** - Download conversations with formatting preserved
- **Search Functionality** - Search across chats and within messages

#### 📥 Model Management
- **Download Models** - Built-in model downloader with Ollama integration
- **Progress Tracking** - Real-time download progress with percentage and status
- **Cancel Downloads** - Ability to cancel in-progress downloads
- **Model Library** - Browse and install models from Ollama library
- **Variant Selection** - Interactive modal showing model sizes and hardware requirements

#### � Monitoring & Analytics
- **Usage Dashboard** - Track total chats, active conversations, and message counts
- **Model Statistics** - See which models you use most frequently
- **Chat Analytics** - Monitor your AI interaction patterns

---

## 🖼️ Screenshots

### Dashboard
Track your usage with beautiful statistics cards showing total chats, active conversations, models used, and message counts.

### Chats Interface
WhatsApp-inspired 1/3 - 2/3 split layout with chat list on the left and conversation on the right.

### Settings
Download new models, configure default hyperparameters, and customize your experience.

---

## 🚀 Quick Start

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v18 or higher) - [Download here](https://nodejs.org/)
- **npm** (comes with Node.js)
- **Ollama** (optional - the setup script can install it for you)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/rexmirak/free-llms.git
   cd free-llms
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Run the setup script**
   ```bash
   npm run setup
   ```
   
   This script will:
   - Check if Ollama is installed (and offer to install it)
   - Start Ollama if it's not running
   - Download the default model (llama3.2)
   - Create necessary data directories
   - Initialize default settings

4. **Start the development server**
   ```bash
   npm run dev
   ```

5. **Open your browser**
   
   Navigate to [http://localhost:3000](http://localhost:3000)

---

## 📚 Usage Guide

### Creating a New Chat

1. Click the **"New Chat"** button in the Chats page
2. Fill in the chat details:
   - **Chat Name**: Give your conversation a memorable name
   - **Model**: Select from your installed models
   - **System Message**: Define the AI's behavior (optional)
   - **Hyperparameters**: Fine-tune the model's responses
3. Click **"Create Chat"** and start conversing!

### Understanding the Interface

#### Chat List Panel (Left 1/3)
- **Search Bar**: Quickly find chats by name
- **Unread Indicators**: Blue dot shows chats with new messages
- **Model Tags**: See which model each chat uses
- **Last Updated**: Timestamp of the last message

#### Chat Window (Right 2/3)
- **Message Display**: User messages in blue, assistant in light grey
- **Thinking Process**: DeepSeek-R1 models show reasoning in collapsible 🧠 sections
- **Search**: Find specific messages within the conversation
- **Export**: Download chat as Markdown file
- **Settings**: Quick access to chat configuration

### DeepSeek Reasoning Models

When using reasoning models like **DeepSeek-R1**, you'll see their thinking process in real-time:

1. **🧠 Thinking Process Section**: Appears above the response in a collapsible panel
2. **Real-time Streaming**: Watch the model's reasoning unfold as it thinks
3. **Toggle Visibility**: Click the header to expand/collapse the thinking process
4. **Preserved in History**: Reasoning is saved with the message for later review
5. **Final Answer**: Clean answer appears below the thinking section

This transparency helps you understand how the model arrives at its conclusions, making it perfect for:
- **Learning**: See how AI reasons through problems
- **Debugging**: Understand why a model gave a specific answer
- **Trust**: Verify the reasoning behind recommendations

### Managing Models

#### Downloading Models

1. Go to **Settings** page
2. Scroll to **Model Management**
3. Enter a model name:
   - **Base name only** (e.g., `llama3.2`) → Opens variant selector
   - **With tag** (e.g., `llama3.2:3b`) → Downloads directly
4. Select variant from the modal (shows size and RAM requirements)
5. Click **Download** and watch the progress bar
6. Cancel anytime with the **Cancel** button

#### Model Variants

Popular models come in different sizes:
- **llama3.2**: 1B (1.3GB RAM), 3B (2GB RAM)
- **llama3.1**: 8B (4.7GB RAM), 70B (40GB RAM), 405B (231GB RAM)
- **qwen2.5**: 0.5B-72B (various sizes)
- **deepseek-r1**: 1.5B-70B (reasoning models)

The interface shows hardware requirements to help you choose!

Visit [Ollama Library](https://ollama.com/library) for all available models.

### Hyperparameters Explained

Fine-tune how your AI responds with these controls:

- **Temperature** (0-2): Controls randomness
  - Lower (0.1-0.5) = More focused and deterministic
  - Higher (0.8-2.0) = More creative and diverse
  - Default: 0.7

- **Top P** (0-1): Nucleus sampling
  - Controls diversity by limiting token selection
  - Lower = More focused, Higher = More diverse
  - Default: 0.9

- **Top K** (1-100): Limits vocabulary
  - Only considers the K most likely next tokens
  - Lower = More focused, Higher = More variety
  - Default: 40

- **Max Tokens** (100-8192): Maximum response length
  - Controls how long responses can be
  - 1 token ≈ 4 characters
  - Default: 2048

💡 **Tip**: Each chat can have different hyperparameters! Use high temperature for creative writing and low temperature for factual queries.

### Advanced Features

#### Background Chat Notifications

- Send a message in Chat A
- Switch to Chat B while Chat A is still generating
- Chat A automatically gets an unread indicator (blue dot)
- Response saves correctly to Chat A in the background

#### Markdown Export

Export includes:
- Chat metadata (name, model, system message)
- All messages with timestamps
- Formatted code blocks and markdown
- Reasoning content from DeepSeek models

#### Search Functionality

- **Chat List Search**: Filter chats by name in real-time
- **Message Search**: Find specific text within a conversation
- **Case Insensitive**: Matches regardless of capitalization

---

## 🏗️ Project Structure

```
free-llms/
├── app/                      # Next.js app directory (App Router)
│   ├── api/                 # API routes (Server-Side Events)
│   │   ├── chats/          # Chat CRUD operations
│   │   ├── ollama/         
│   │   │   ├── chat/       # Streaming chat with reasoning
│   │   │   ├── pull/       # Model download with progress
│   │   │   └── tags/       # List installed models
│   │   └── settings/       # Settings management
│   ├── chats/              # Chats page with split layout
│   ├── settings/           # Settings with model download
│   ├── layout.tsx          # Root layout with ThemeProvider
│   ├── page.tsx            # Dashboard with statistics
│   └── globals.css         # Global styles + Tailwind
├── components/              # React components
│   ├── ChatList.tsx        # Chat list with search & unread
│   ├── ChatWindow.tsx      # Chat UI with reasoning display
│   ├── NewChatModal.tsx    # New chat with model selection
│   ├── ChatSettingsModal.tsx # Edit chat settings
│   ├── Navbar.tsx          # Navigation with dark mode toggle
│   └── ThemeProvider.tsx   # Theme context provider
├── lib/                     # Utilities & types
│   ├── ollama.ts           # Ollama API client (SSE streaming)
│   ├── storage.ts          # File system operations
│   ├── types.ts            # TypeScript interfaces
│   ├── utils.ts            # Helper functions
│   └── constants.ts        # App constants
├── data/                    # Local data storage (gitignored)
│   ├── chats/              # Individual chat JSON files
│   └── settings.json       # App settings
├── scripts/
│   └── setup.sh            # Automated setup script
├── public/
│   └── logo.png            # App logo
├── tailwind.config.ts       # Tailwind configuration
├── tsconfig.json           # TypeScript configuration
└── package.json            # Dependencies & scripts
```

### Key Technical Decisions

- **Server-Sent Events (SSE)**: Used for streaming responses from Ollama
- **File-based Storage**: JSON files for simplicity and portability
- **AbortController**: Enables model download cancellation
- **React Refs**: Manage chat switching without race conditions
- **Markdown Support**: ReactMarkdown with remark-gfm for rich formatting
- **Theme Context**: useContext for dark mode across components

---

## ⚙️ Configuration

### Default Settings

Default settings are stored in `data/settings.json`:

```json
{
  "defaultModel": "llama3.2",
  "defaultHyperparameters": {
    "temperature": 0.7,
    "top_p": 0.9,
    "top_k": 40,
    "max_tokens": 2048
  },
  "defaultSystemMessage": "You are a helpful assistant...",
  "installedModels": ["llama3.2"],
  "theme": "light"
}
```

### Chat Storage

Each chat is stored as a separate JSON file in `data/chats/`:

```json
{
  "id": "chat-1730505600000-abc123",
  "name": "My Research Chat",
  "model": "llama3.2",
  "systemMessage": "You are a helpful research assistant.",
  "hyperparameters": {
    "temperature": 0.7,
    "top_p": 0.9,
    "top_k": 40,
    "max_tokens": 2048
  },
  "messages": [
    {
      "id": "msg-1730505601000",
      "role": "user",
      "content": "Explain quantum computing",
      "timestamp": "2025-11-01T12:00:01.000Z"
    },
    {
      "id": "msg-1730505615000",
      "role": "assistant",
      "content": "Quantum computing uses quantum bits...",
      "reasoning_content": "Let me think about this systematically...",
      "timestamp": "2025-11-01T12:00:15.000Z"
    }
  ],
  "createdAt": "2025-11-01T12:00:00.000Z",
  "updatedAt": "2025-11-01T12:00:15.000Z",
  "unread": false
}
```

**Note**: `reasoning_content` field is populated by DeepSeek-R1 and similar reasoning models.

---

## 🔧 Troubleshooting

### Ollama Not Running

**Error**: "Ollama is not running" or connection refused

**Solution**:
```bash
# Start Ollama service
ollama serve

# Or on macOS/Windows, ensure Ollama app is running
```

### Model Not Found

**Error**: "Model not found" when creating chat

**Solution**:
```bash
# Download the model first
ollama pull llama3.2

# Or use the Settings page Model Manager
```

### Download Errors

**Error**: "Model download failed" or "Error: invalid model name"

**Solutions**:
1. Verify model name at [Ollama Library](https://ollama.com/library)
2. Check internet connection
3. Ensure Ollama is running (`ollama list` should work)
4. Check Ollama version: `ollama --version` (needs v0.1.0+)
5. Try with explicit tag: `qwen3-vl:4b` instead of `qwen3-vl`

### Port Already in Use

**Error**: "Port 3000 is already in use"

**Solution**:
```bash
# Kill existing process
lsof -ti:3000 | xargs kill -9

# Or use a different port
PORT=3001 npm run dev
```

### Chat Not Saving

**Issue**: Chats disappear after refresh

**Solutions**:
1. Check that `data/chats/` directory exists
2. Ensure you have write permissions:
   ```bash
   chmod -R 755 data/
   ```
3. Check browser console for errors
4. Verify API route is working: http://localhost:3000/api/chats

### Reasoning Not Showing

**Issue**: Using DeepSeek but no thinking process appears

**Solutions**:
1. Ensure you're using a DeepSeek-R1 model (not regular DeepSeek):
   ```bash
   ollama pull deepseek-r1:1.5b
   # or
   ollama pull deepseek-r1:8b
   ```
2. The thinking only appears when the model uses reasoning (not all queries trigger it)
3. Check browser console for any SSE connection errors
4. Verify Ollama version supports the "thinking" field (v0.5.0+)

### Multiple Dev Servers

**Issue**: Multiple Next.js instances running

**Solution**:
```bash
# Kill all node processes
killall node

# Clean rebuild
rm -rf .next
npm run dev
```

---

## 🛠️ Development

### Running in Development Mode

```bash
npm run dev
```

The app will be available at http://localhost:3000

### Building for Production

```bash
npm run build
npm run start
```

The production build optimizes:
- Minified JavaScript and CSS
- Optimized images and assets
- Server-side rendering for fast initial load
- Static page generation where possible

**Performance Tips**:
- Use environment variables for configuration
- Enable compression in your web server
- Consider using a reverse proxy (nginx, Apache)
- Monitor memory usage for long-running chats

### Linting

```bash
npm run lint
```

### Technical Implementation Details

#### Streaming Architecture

The app uses **Server-Sent Events (SSE)** for real-time streaming:

1. **Client** sends message to `/api/ollama/chat`
2. **API Route** creates a `ReadableStream` with SSE format
3. **Ollama Client** (`lib/ollama.ts`) connects to Ollama API
4. **Stream Parsing**: Extracts `content` and `reasoning_content` from JSON chunks
5. **Client UI** updates in real-time with both response and thinking

```typescript
// SSE Stream Format
data: {"chunk": "Hello", "reasoning": "User asked a greeting..."}\n\n
data: {"chunk": " world!", "reasoning": "Completing the response..."}\n\n
data: {"done": true}\n\n
```

#### Chat State Management

Uses **React Refs** to prevent race conditions during chat switching:

```typescript
const selectedChatIdRef = useRef<string | null>(null);

// Only update UI if still viewing the same chat
if (selectedChatIdRef.current === currentChatId) {
  setCurrentResponse(fullResponse);
}
```

This ensures:
- Messages go to the correct chat
- Unread indicators work properly
- No ghost messages appear in wrong chats

#### Model Download Cancellation

Uses **AbortController** for graceful cancellation:

```typescript
const abortController = new AbortController();

// Cancel download
abortController.abort();

// API route detects and stops streaming
```

#### Dark Mode Implementation

Context-based theme with CSS variables:

```typescript
// ThemeProvider.tsx
const [theme, setTheme] = useState<'light' | 'dark'>('light');

// Tailwind classes adapt automatically
className="bg-gray-100 dark:bg-gray-700"
```

---

## 🌟 Roadmap

### ✅ Implemented Features

#### Core Functionality
- [x] WhatsApp-inspired chat interface with split layout
- [x] Multiple chat management with create/delete/update
- [x] Real-time streaming responses via Server-Sent Events
- [x] Multiple model support per chat
- [x] Local JSON-based storage system
- [x] Dark mode with theme persistence

#### Model Management
- [x] Model download with progress tracking
- [x] Model variant selector with hardware requirements
- [x] Download cancellation with AbortController
- [x] Installed model detection and listing
- [x] Model size and RAM requirement display

#### Chat Features
- [x] Unread message indicators for background chats
- [x] Background chat handling (responses save to correct chat)
- [x] Message search within conversations
- [x] Chat search/filter in sidebar
- [x] Markdown rendering with code highlighting
- [x] Export conversations to Markdown
- [x] Hyperparameter customization per chat
- [x] Custom system messages per chat
- [x] DeepSeek-R1 reasoning/thinking process display
- [x] Collapsible thinking sections with real-time streaming

#### UI/UX
- [x] Loading indicators and streaming cursor
- [x] Toast notifications for errors
- [x] Model variant selection modal
- [x] Settings page with configuration options
- [x] Usage dashboard with statistics
- [x] Responsive chat bubbles
- [x] Timestamp display on messages

### 🚀 Next Up (Priority Features)

#### 📁 File Handling (High Priority)
- [ ] Document upload (PDF, TXT, MD, DOCX)
- [ ] File context injection into prompts
- [ ] RAG (Retrieval Augmented Generation) integration
- [ ] Multi-file chat contexts
- [ ] File preview in chat
- [ ] Document chunking and embedding

#### 🎨 Image Generation (High Priority)
- [ ] Stable Diffusion integration via Ollama
- [ ] Text-to-image generation in chat
- [ ] Image preview in chat bubbles
- [ ] Image download and export
- [ ] Prompt templates for image generation
- [ ] Image history gallery

#### 💎 UI/UX Enhancements (Medium Priority)
- [ ] Mobile responsive design
- [ ] Drag-and-drop file upload
- [ ] Chat folders/categories
- [ ] Conversation branching
- [ ] Message editing and regeneration
- [ ] Keyboard shortcuts
- [ ] Split-screen model comparison
- [ ] Chat templates library
- [ ] Custom color themes
- [ ] Font size adjustments

#### 📤 Export & Tools (Medium Priority)
- [ ] Export to PDF with styling
- [ ] Export to HTML
- [ ] Bulk chat export
- [ ] Import chat history
- [ ] Web search integration (DuckDuckGo, SearXNG)
- [ ] URL content fetching
- [ ] Code execution sandbox
- [ ] LaTeX/Math rendering improvements

#### 🔮 Future Enhancements (Low Priority)
- [ ] Voice input support (Web Speech API)
- [ ] Text-to-speech for responses
- [ ] Multi-language interface (i18n)
- [ ] Cloud backup option (optional, encrypted)
- [ ] Collaborative chats (local network)
- [ ] Plugin system for extensions
- [ ] API key management for cloud models
- [ ] Conversation analytics and insights
- [ ] Automated chat summarization

---

## 🤝 Contributing

**We welcome contributions from the community!** Whether you're a developer, designer, or AI enthusiast, there are many ways to help make Free LLMs better.

### 🎯 Priority Areas

We're especially looking for help with:

1. **📁 File Handling & RAG**
   - Document parsing (PDF, DOCX, TXT)
   - Vector embeddings for semantic search
   - Document chunking strategies
   - File upload UI/UX

2. **🎨 Image Generation**
   - Stable Diffusion integration via Ollama
   - Image generation UI components
   - Prompt engineering templates
   - Gallery and history views

3. **💎 UI/UX Improvements**
   - Mobile responsive design
   - Accessibility enhancements
   - Animation and transitions
   - Component library expansion

4. **🔧 Web Tools & Integrations**
   - Web search integration (DuckDuckGo API)
   - URL content fetching and parsing
   - Code sandbox execution
   - Browser extension development

### 🚀 How to Contribute

1. **Fork the repository**
   ```bash
   git clone https://github.com/rexmirak/free-llms.git
   cd free-llms
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```

3. **Make your changes**
   - Follow the existing code style
   - Add comments for complex logic
   - Test your changes thoroughly

4. **Commit your changes**
   ```bash
   git commit -m 'Add amazing feature'
   ```

5. **Push to your fork**
   ```bash
   git push origin feature/amazing-feature
   ```

6. **Open a Pull Request**
   - Describe your changes clearly
   - Reference any related issues
   - Include screenshots for UI changes

### 📋 Contribution Guidelines

- **Code Style**: Follow TypeScript best practices, use ESLint
- **Commits**: Write clear, descriptive commit messages
- **Testing**: Test your changes with multiple models
- **Documentation**: Update README if adding features
- **Issues**: Check existing issues before creating new ones

### 💡 Good First Issues

New to the project? Look for issues tagged with:
- `good first issue` - Easy entry points
- `help wanted` - Features needing implementation
- `bug` - Known issues to fix
- `documentation` - Docs improvements

### 🎨 Design Contributions

Designers welcome! We need help with:
- UI/UX mockups for new features
- Icon design and illustrations
- Color scheme refinements
- Animation concepts

### 📖 Documentation

Help improve our docs:
- Tutorial videos or blog posts
- API documentation
- Code comments
- Translation to other languages

### 💬 Community

- **GitHub Issues**: [Report bugs or request features](https://github.com/rexmirak/free-llms/issues)
- **Discussions**: Share ideas and ask questions
- **Pull Requests**: Contribute code directly

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 💬 Support

Need help or have questions?

- **GitHub Issues**: [Create an issue](https://github.com/rexmirak/free-llms/issues)
- **GitHub**: [@rexmirak](https://github.com/rexmirak)

---

## 🙏 Acknowledgments

- [Ollama](https://ollama.com) - For making local LLMs accessible and easy to deploy
- [DeepSeek](https://deepseek.com) - For pioneering reasoning models with transparent thinking
- [Next.js](https://nextjs.org/) - The React framework powering this app
- [Tailwind CSS](https://tailwindcss.com/) - For the beautiful, utility-first styling
- [Lucide Icons](https://lucide.dev/) - For the comprehensive icon set
- [ReactMarkdown](https://github.com/remarkjs/react-markdown) - For rendering markdown in chats
- [remark-gfm](https://github.com/remarkjs/remark-gfm) - For GitHub-flavored markdown support

### Special Thanks

To the open-source AI community for making powerful language models available to everyone, locally and privately.

---

<div align="center">
  Made with ❤️ by <a href="https://github.com/rexmirak">@rexmirak</a>
  
  ⭐ Star this repo if you find it useful!
</div>
