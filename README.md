# Syntax AI - Mastra Agent Platform

    A powerful AI agent platform built with [Mastra](https://mastra.ai), featuring Agent-to-Agent (A2A) communication capabilities and the TelexAssistant development agent.

## 🚀 Quick Start

### Prerequisites

- Node.js >= 20.9.0
- pnpm (recommended) or npm

### Installation

```bash
# Install dependencies
pnpm install

# Start development server
pnpm run dev
```

The server will start at `http://localhost:4111`

## 📁 Project Structure

```
syntax-ai/
├── src/
│   └── mastra/
│       ├── agents/
│       │   └── telexAssistant-agent.ts    # AI development assistant
│       ├── routes/
│       │   └── a2a.route.ts               # A2A protocol endpoint
│       ├── tools/
│       │   ├── getRepoData.tool.ts        # GitHub repo fetcher
│       │   ├── summarizeRepo.tool.ts      # Repo summarizer
│       │   └── weather-tool.ts            # Weather tool
│       ├── workflows/
│       │   └── githubSummarizer.workflow.ts
│       └── index.ts                        # Mastra configuration
├── test-a2a-route.js                       # Node.js test suite
├── test-a2a-curl.sh                        # Bash/curl tests
└── A2A-TESTING.md                          # Detailed testing guide
```

## 🤖 Available Agents

### TelexAssistant

An elite AI development assistant specialized in:

- Code generation (TypeScript/JavaScript)
- Code refactoring and optimization
- Code review and best practices
- Technical problem solving

**Model:** Google Gemini 2.5 Flash  
**Memory:** Persistent (LibSQL)

## 🔌 API Endpoints

### A2A Agent Communication

```
POST /a2a/agent/:agentId
```

Interact with agents using the Agent-to-Agent (A2A) protocol.

**Example Request:**

```bash
curl -X POST http://localhost:4111/a2a/agent/TelexAssistant \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "1",
    "method": "generate",
    "params": {
      "messages": [
        {
          "role": "user",
          "parts": [
            {
              "kind": "text",
              "text": "Write a TypeScript function to add two numbers"
            }
          ]
        }
      ]
    }
  }'
```

**Response Format:**

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "result": {
    "id": "task-uuid",
    "contextId": "context-uuid",
    "status": {
      "state": "completed",
      "timestamp": "2025-11-03T...",
      "message": {
        "messageId": "msg-uuid",
        "role": "agent",
        "parts": [
          {
            "kind": "text",
            "text": "Generated code..."
          }
        ],
        "kind": "message"
      }
    },
    "artifacts": [...],
    "history": [...],
    "kind": "task"
  }
}
```

## 🧪 Testing

### Run All Tests

```bash
# Node.js test suite (recommended)
node test-a2a-route.js

# Bash/curl tests
bash test-a2a-curl.sh
```

### Manual Testing

See [A2A-TESTING.md](./A2A-TESTING.md) for detailed testing instructions and examples.

## 📝 Scripts

```bash
# Development
pnpm run dev          # Start dev server with hot reload

# Production
pnpm run build        # Build for production
pnpm run start        # Start production server

# Testing
pnpm run test         # Run tests
```

## 🛠️ Configuration

### Environment Variables

Create a `.env` file in the root directory:

```env
# Optional: Configure AI model providers
GOOGLE_API_KEY=your_google_api_key
OPENAI_API_KEY=your_openai_api_key
ANTHROPIC_API_KEY=your_anthropic_api_key
```

### Storage

By default, the system uses in-memory storage. To persist data:

Edit `src/mastra/index.ts`:

```typescript
storage: new LibSQLStore({
  url: "file:./mastra.db", // Change from ":memory:"
}),
```

## 🏗️ Adding New Agents

1. Create agent file in `src/mastra/agents/`:

```typescript
import { Agent } from "@mastra/core/agent";

export const MyAgent = new Agent({
  name: "MyAgent",
  instructions: "Your agent instructions...",
  model: "google/gemini-2.5-flash",
});
```

2. Register in `src/mastra/index.ts`:

```typescript
export const mastra = new Mastra({
  agents: {
    TelexAssistantAgent,
    MyAgent, // Add your agent
  },
  // ...
});
```

3. Access via API:

```bash
POST /a2a/agent/MyAgent
```

## 🔧 Adding Tools

Create tools in `src/mastra/tools/`:

```typescript
import { createTool } from "@mastra/core/tools";
import { z } from "zod";

export const myTool = createTool({
  id: "my-tool",
  description: "Tool description",
  inputSchema: z.object({
    param: z.string(),
  }),
  outputSchema: z.object({
    result: z.string(),
  }),
  execute: async ({ context }) => {
    return { result: "..." };
  },
});
```

## 📚 Resources

- [Mastra Documentation](https://docs.mastra.ai)
- [A2A Protocol](https://github.com/a2a-js)
- [API Testing Guide](./A2A-TESTING.md)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📄 License

ISC

## 🆘 Troubleshooting

### Server won't start

- Check if port 4111 is available
- Verify Node.js version (>= 20.9.0)
- Clear cache: `rm -rf node_modules && pnpm install`

### Agent not found error

- Ensure agent is registered in `src/mastra/index.ts`
- Check agent name matches exactly (case-sensitive)

### Memory issues

- Switch to file-based storage in production
- Monitor memory usage with persistent storage

## 📧 Support

For issues and questions, please open an issue on GitHub.
