# Claude Code - Complete Codebase Explanation

> **Language**: Hinglish (Hindi + English Mix)

---

## Table of Contents
1. [Project Kya Hai?](#project-kya-hai)
2. [Folder Structure](#folder-structure)
3. [Main Entry Point (`main.tsx`)](#main-entry-point)
4. [Tools System](#tools-system)
5. [State Management](#state-management)
6. [Services Layer](#services-layer)
7. [Bootstrap & Context](#bootstrap--context)
8. [Utils (Utilities)](#utils-utilities)
9. [Commands System](#commands-system)
10. [Plugins System](#plugins-system)
11. [MCP (Model Context Protocol)](#mcp-model-context-protocol)
12. [Hooks System](#hooks-system)
13. [UI Components (Ink/React)](#ui-components)
14. [How It All Works Together](#how-it-all-works-together)

---

## Project Kya Hai?

Ye **Claude Code** hai - Anthropic ka official CLI tool jo AI-powered coding assistant ke roop mein kaam karta hai. Isme:

- **AI Agent** hai jo code likhta, padhta, aur edit karta hai
- **Terminal UI** hai jo React + Ink se bana hai
- **Tools System** hai jaise Bash, File Read/Write, Web Search
- **MCP Integration** hai jo external tools connect karne deta hai
- **Permission System** hai jo security ke liye zaroori hai

---

## Folder Structure

```
src/
├── main.tsx              # App ka entry point - sabse pehle ye chalta hai
├── context.ts            # System aur User context load karta hai
├── tools.ts              # Sabhi tools ka registry
├── commands.ts           # CLI commands define karta hai
├── state/                # App state management (React Context/Store)
├── tools/                # Individual tools (Bash, FileRead, etc.)
├── services/             # Backend services (API, MCP, Analytics)
├── utils/                # Helper functions (300+ files)
├── bootstrap/            # App initialization state
├── components/           # React UI components
├── ink/                  # Terminal UI rendering (Ink library)
├── hooks/                # React hooks
├── plugins/              # Plugin system
├── skills/               # Skill system (specialized abilities)
├── migrations/           # Data migration scripts
├── types/                # TypeScript type definitions
├── constants/            # App constants
├── entrypoints/          # Different app entry points
├── server/               # Server/remote session handling
└── remote/               # Remote session management
```

---

## Main Entry Point

**File**: `main.tsx`

Ye file sabse pehle execute hoti hai. Iska kaam:

### 1. Startup Optimization
```typescript
// Side effects jo imports se pehle chalte hain
profileCheckpoint('main_tsx_entry');
startMdmRawRead();      // MDM settings parallel mein load
startKeychainPrefetch(); // Keychain reads parallel mein
```

### 2. Debug Detection
```typescript
function isBeingDebugged() {
  // Agar koi debugger attached hai toh exit kar do
  // Security ke liye - debugging mode mein run nahi hone dete
}
```

### 3. Main Function
```typescript
export async function main() {
  // Step 1: Windows security
  process.env.NoDefaultCurrentDirectoryInExePath = '1';
  
  // Step 2: Warning handler initialize
  initializeWarningHandler();
  
  // Step 3: CLI arguments parse karo
  // Step 4: Client type determine karo (cli, sdk, vscode, etc.)
  // Step 5: Settings load karo
  // Step 6: Run function call karo
}
```

### 4. Run Function
```typescript
async function run() {
  // Commander.js se CLI commands setup
  // preAction hook: initialization before any command
  // - init() call hota hai
  // - Migrations run hote hain
  // - Remote settings load hote hain
}
```

### 5. Deferred Prefetches
```typescript
export function startDeferredPrefetches() {
  // Ye sab background mein hota hai:
  // - User context load
  // - Git status fetch
  // - Analytics initialize
  // - Model capabilities refresh
}
```

---

## Tools System

**File**: `tools.ts`

Tools wo hain jo AI agent ke pass available hote hain. Har tool ek specific kaam karta hai.

### Available Tools:

| Tool | Kaam |
|------|------|
| `BashTool` | Terminal commands run karta hai |
| `FileReadTool` | Files padhta hai |
| `FileEditTool` | Files edit karta hai |
| `FileWriteTool` | Nayi files banata hai |
| `GlobTool` | Files search karta hai pattern se |
| `GrepTool` | Content search karta hai |
| `WebFetchTool` | URLs se data fetch karta hai |
| `WebSearchTool` | Internet pe search karta hai |
| `AgentTool` | Sub-agents spawn karta hai |
| `TodoWriteTool` | Tasks/TODOs manage karta hai |
| `BashTool` | Commands run karta hai |
| `NotebookEditTool` | Jupyter notebooks edit karta hai |
| `LSPTool` | Language Server Protocol integration |
| `MCPTool` | External MCP servers ke tools |
| `REPLTool` | Code execution environment |
| `PowerShellTool` | PowerShell commands (Windows) |

### Tool Registration:
```typescript
export function getAllBaseTools(): Tools {
  return [
    AgentTool,
    BashTool,
    FileReadTool,
    FileEditTool,
    // ... aur bhi tools
  ]
}
```

### Tool Filtering:
```typescript
export function getTools(permissionContext: ToolPermissionContext): Tools {
  // Permission context ke basis pe tools filter hote hain
  // Agar koi tool deny list mein hai toh wo nahi aayega
  return filterToolsByDenyRules(tools, permissionContext)
}
```

### Tool Pool Assembly:
```typescript
export function assembleToolPool(permissionContext, mcpTools): Tools {
  // Built-in tools + MCP tools combine karta hai
  // Deduplication karta hai (built-in tools priority)
  // Cache stability ke liye sort karta hai
}
```

---

## State Management

**Folder**: `state/`

### AppState Store
```typescript
// state/AppStateStore.ts
export type AppState = {
  sessionId: string;
  messages: Message[];
  tools: Tool[];
  mcp: McpState;
  // ... aur bhi state properties
}
```

### Store Structure:
```typescript
// state/store.ts
export function createStore() {
  // React Context based store
  // State updates ke liye actions
  // Subscribers for state changes
}
```

### State Changes:
```typescript
// state/onChangeAppState.ts
export function onChangeAppState(prev: AppState, next: AppState) {
  // Jab bhi state change hoti hai ye call hota hai
  // UI re-render hoti hai
  // Side effects handle hote hain
}
```

---

## Services Layer

**Folder**: `services/`

Services backend operations handle karti hain.

### API Service
```typescript
// services/api/
// - bootstrap.js: Initial data fetch
// - filesApi.js: File upload/download
// - referral.js: Referral system
```

### MCP Service
```typescript
// services/mcp/
// - client.js: MCP client connection
// - config.js: MCP configuration
// - officialRegistry.js: Official MCP servers
```

### Analytics Service
```typescript
// services/analytics/
// - growthbook.js: Feature flags
// - index.js: Event logging
// - config.js: Analytics settings
```

### Settings Service
```typescript
// services/settingsSync/
// - Local settings upload
// - Remote settings download
```

### Policy Limits
```typescript
// services/policyLimits/
// - Enterprise policy enforcement
// - Rate limiting
```

---

## Bootstrap & Context

**File**: `bootstrap/state.ts`, `context.ts`

### Bootstrap State
```typescript
// Ye sab app startup pe set hota hai:
setIsInteractive(true/false);    // Interactive mode?
setClientType('cli');            // Kaunsa client
setOriginalCwd(process.cwd());   // Original directory
setSessionSource('cli');         // Session source
```

### Context System
```typescript
// context.ts
export const getSystemContext = memoize(async () => {
  // Git status fetch karta hai
  // Current branch, recent commits
  // Ye conversation ke start pe cache hota hai
});

export const getUserContext = memoize(async () => {
  // Claude.md files padhta hai
  // Current date add karta hai
  // User-specific context
});
```

---

## Utils (Utilities)

**Folder**: `utils/` (300+ files)

Helpers jo har jagah use hote hain.

### Key Utils:

| File | Purpose |
|------|---------|
| `git.ts` | Git operations (branch, status, commits) |
| `config.ts` | Global config read/write |
| `auth.ts` | Authentication (OAuth, API keys) |
| `permissions/` | Permission checking system |
| `model/` | AI model selection & capabilities |
| `errors.ts` | Error handling utilities |
| `json.ts` | JSON parsing |
| `path.ts` | Path manipulation |
| `shell.ts` | Shell command execution |
| `sessionStorage.ts` | Session save/load |
| `teleport.ts` | Remote session teleport |
| `claudemd.ts` | Claude.md file handling |
| `gracefulShutdown.ts` | Clean app exit |

---

## Commands System

**File**: `commands.ts`, `commands/`

CLI commands jo user terminal se run kar sakta hai.

### Main Commands:
```
claude                    # Interactive session start
claude -p "prompt"        # Non-interactive (print mode)
claude --continue         # Last session continue
claude --resume <id>      # Specific session resume
claude mcp                # MCP server management
claude plugin             # Plugin management
claude auth               # Authentication
claude doctor             # Diagnostics
claude ssh <host>         # Remote SSH session
```

### Command Registration:
```typescript
program
  .name('claude')
  .description('Claude Code - AI coding assistant')
  .argument('[prompt]', 'Your prompt')
  .option('-d, --debug', 'Debug mode')
  .option('-p, --print', 'Non-interactive mode')
  .option('--model <model>', 'AI model to use')
  // ... aur bhi options
```

---

## Plugins System

**Folder**: `plugins/`, `utils/plugins/`

Plugins app ki functionality extend karte hain.

### Plugin Loading:
```typescript
// utils/plugins/pluginLoader.ts
// - Bundled plugins load hote hain
// - User-installed plugins load hote hain
// - Plugin cache use hota hai
```

### Plugin Types:
- **Bundled Plugins**: App ke saath aate hain
- **User Plugins**: User install karta hai
- **Inline Plugins**: `--plugin-dir` se load

---

## MCP (Model Context Protocol)

**Folder**: `services/mcp/`

MCP external tools aur servers connect karne ka protocol hai.

### MCP Features:
```typescript
// MCP servers connect karke tools add kar sakte ho
// Examples:
// - GitHub MCP server
// - Filesystem MCP server  
// - Database MCP server
// - Custom MCP servers
```

### MCP Configuration:
```typescript
// MCP servers config file se ya CLI se add hote hain
claude mcp add <name> <command>
claude mcp list
claude mcp remove <name>
```

---

## Hooks System

**Folder**: `utils/hooks/`

Hooks events pe custom actions run karne dete hain.

### Hook Events:
- `sessionStart`: Session start hone pe
- `preToolCall`: Tool call se pehle
- `postToolCall`: Tool call ke baad
- `notification`: Notifications ke time

---

## UI Components

**Folder**: `components/`, `ink/`

Terminal UI React + Ink se bana hai.

### Key Components:
```typescript
// REPL.tsx - Main interactive prompt
// MessageDisplay.tsx - Messages show karta hai
// ToolUse.tsx - Tool calls display
// StatusLine.tsx - Status bar
// PermissionDialog.tsx - Permission requests
```

### Ink Usage:
```typescript
// Ink React components terminal pe render hoti hain
// Text, Box, Color components use hote hain
// Keyboard input handle hoti hai
```

---

## How It All Works Together

### Application Flow:

```
1. main.tsx execute hota hai
   ↓
2. CLI arguments parse hote hain
   ↓
3. Settings load hote hain
   ↓
4. init() call hota hai
   ↓
5. Migrations run hote hain
   ↓
6. Tools initialize hote hain
   ↓
7. MCP servers connect hote hain
   ↓
8. REPL (interactive prompt) start hota hai
   ↓
9. User input aata hai
   ↓
10. AI model process karta hai
   ↓
11. Tools call hote hain (agar zaroorat ho)
   ↓
12. Response user ko milta hai
   ↓
13. Loop continues...
```

### Data Flow:

```
User Input
    ↓
[Message System]
    ↓
[AI Model API Call]
    ↓
[Tool Selection]
    ↓
[Tool Execution]
    ↓
[Result Processing]
    ↓
[Response Generation]
    ↓
[UI Display]
```

### Security Layers:

```
1. Permission System - Tools ko allow/deny
2. Sandbox - Commands isolated environment mein
3. Trust Dialog - User se confirmation
4. Deny Rules - Specific tools block
5. Policy Limits - Enterprise restrictions
```

---

## Key Patterns Used

### 1. Memoization
```typescript
const getSystemContext = memoize(async () => {
  // Expensive operation ko cache karta hai
});
```

### 2. Lazy Loading
```typescript
const getTool = () => require('./tools/Tool.js');
// Sirf tab load hota hai jab zaroorat ho
```

### 3. Feature Flags
```typescript
if (feature('FEATURE_NAME')) {
  // Feature enabled hai toh ye code run
}
```

### 4. Dead Code Elimination
```typescript
const Tool = feature('TOOL_ENABLED') 
  ? require('./Tool.js') 
  : null;
// Build time pe unused code remove ho jata hai
```

### 5. Parallel Execution
```typescript
await Promise.all([
  getIsGit(),
  getWorktreeCount(),
  getGhAuthStatus(),
]);
// Multiple async operations parallel mein
```

---

## Environment Variables

Important environment variables:

| Variable | Purpose |
|----------|---------|
| `CLAUDE_CODE_USE_BEDROCK` | AWS Bedrock use karo |
| `CLAUDE_CODE_USE_VERTEX` | Google Vertex use karo |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | Terminal title disable |
| `CLAUDE_CODE_SIMPLE` | Simple mode (kam tools) |
| `USER_TYPE` | User type (ant = Anthropic employee) |
| `NODE_ENV` | Environment (test/production) |

---

## Summary

**Claude Code** ek powerful AI-powered CLI tool hai jo:

1. **TypeScript/React** se bana hai
2. **Ink** library se terminal UI render karta hai
3. **Commander.js** se CLI commands handle karta hai
4. **Tools System** ke through AI ko capabilities deta hai
5. **MCP Protocol** ke through extensibility deta hai
6. **Permission System** ke through security maintain karta hai
7. **State Management** ke through consistent behavior deta hai

Har component modular hai aur specific responsibility handle karta hai. Ye architecture scalable aur maintainable hai.
