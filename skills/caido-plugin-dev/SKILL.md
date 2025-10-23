---
name: caido-plugin-dev
description: Comprehensive skill for developing Caido plugins (frontend, backend, or both). Use this skill when creating Caido security testing tool plugins, extending Caido functionality, or building custom automation for web application security auditing within the Caido platform.
---

# Caido Plugin Development

## Overview

Caido is a lightweight web security auditing toolkit designed for penetration testing and vulnerability assessment. This skill provides comprehensive guidance for developing Caido plugins using the official SDK ecosystem.

**Key Resources:**
- Main Docs: https://docs.caido.io/llms.txt
- Developer Docs: https://developer.caido.io/
- LLM-Optimized Docs: https://developer.caido.io/llms.txt
- Community GitHub: https://github.com/caido-community

## About Caido

Caido is a modern web security auditing platform similar to Burp Suite, designed for penetration testers and security researchers. It operates on a client-server architecture and can run locally or on remote instances (VPS, Docker, cloud).

### Core Architecture

- **Client-Server Model:** Supports both desktop application and CLI deployment
- **Technology Stack:** Vue.js frontend, Rust backend, GraphQL API
- **Data Storage:** SQLite database for persistent project data
- **Proxy System:** HTTP/HTTPS traffic interception with dynamic certificate generation

### Main Features and Tabs

Understanding Caido's core features is essential for plugin development, as plugins often extend or integrate with these existing capabilities:

**Proxy & Traffic Management:**
- **Intercept:** Real-time request/response interception and modification - users can pause, inspect, and modify HTTP traffic before it's sent or received
- **HTTP History:** Complete log of all proxied traffic with filtering and search capabilities
- **Search:** Advanced searching across all captured traffic using HTTPQL query language
- **Sitemap:** Visual tree representation of discovered URLs and application structure

**Testing and Automation:**
- **Replay (Repeater):** Manual request crafting and modification tool - send and resend individual HTTP requests with custom modifications, similar to Burp's Repeater
- **Automate:** Fuzzing and automated testing capabilities - run payloads against endpoints, brute-force attacks, parameter testing
- **Workflows:** Multi-step automation sequences that can be passive (automatic analysis) or active (user-triggered)
- **Match & Replace:** Automatic request/response modification rules that apply to all traffic

**Security Analysis:**
- **Findings:** Centralized vulnerability management - document and track security issues discovered during testing
- **Assistant:** AI-powered feature that suggests attack vectors and generates proof-of-concept exploits
- **Scopes:** Define which domains/subdomains are in-scope for testing to focus analysis

**Data Management:**
- **Projects:** Organize work by project with persistent storage and backup/restore
- **Filters:** Create saved filters using HTTPQL to categorize and segment traffic
- **Environment Variables:** Store and manage configuration values that can be used across workflows and plugins
- **Export:** JSON/CSV export capabilities for reporting and data analysis

### HTTPQL Query Language

HTTPQL is the query language we use in Caido to let you filtering requests and responses.

Caido uses HTTPQL for filtering requests throughout the application. Common query examples:
- `req.ext.eq:".js"` - Filter by file extension
- `resp.code.eq:200` - Filter by status code
- `req.method.eq:"GET"` - Filter by HTTP method
- `req.ext.eq:".php" AND resp.code.eq:200` - Combine filters with boolean operators

Understanding HTTPQL is important for plugins that query or filter traffic.

### WebSocket Support

Caido supports WebSocket traffic inspection and manipulation, allowing real-time analysis of WebSocket streams.

### Plugin Integration Points

Plugins can extend virtually any part of Caido:
- Add custom pages to the navigation
- Inject UI components into existing pages via slots
- Register commands in the command palette
- Add context menu items to requests/responses
- Process traffic automatically via intercept callbacks
- Create custom automation workflows
- Generate security findings programmatically
- Access and query the project database
- Integrate with external tools and APIs

**Reference:** See https://docs.caido.io/llms.txt for complete Caido platform documentation

## Plugin Types

Caido supports three types of plugins, each with its own SDK:

1. **Frontend Plugins** (`@caido/sdk-frontend`) - UI components, custom pages, commands, context menus
2. **Backend Plugins** (`@caido/sdk-backend`) - Server-side logic, HTTP operations, data processing, findings
3. **Workflow Plugins** (`@caido/sdk-workflow`) - JavaScript nodes for automation sequences

Plugins can combine frontend and backend components in a single package. In most cases, we're going to focus on frontend and backend plugins.

## Quick Start: Creating a New Plugin

**IMPORTANT:** Always start a new Caido plugin project using the official starter CLI:

```bash
pnpm create @caido-community/plugin
```

This is the standard and recommended way to initialize any new Caido plugin project.

### Prerequisites

- Node.js 18+ or 20+
- pnpm package manager

### Initialize a New Plugin

Use the official starter CLI to scaffold a new plugin:

```bash
pnpm create @caido-community/plugin
```

This creates a template with:
- `caido.config.ts` - Plugin configuration
- `manifest.json` - Auto-generated metadata
- Frontend and/or backend starter code
- Build configuration

**Reference:** https://developer.caido.io/guides/ (Getting Started section)

### Install Dependencies

```bash
pnpm install
```

### Build the Plugin

```bash
pnpm build
```

This produces `dist/plugin_package.zip` ready for installation in Caido.

## Project Structure

A typical Caido plugin follows this structure:

```
my-plugin/
├── caido.config.ts          # Plugin configuration
├── manifest.json            # Auto-generated from config
├── package.json
├── pnpm-workspace.yaml      # If using monorepo
├── packages/
│   ├── frontend/            # Frontend plugin code
│   │   ├── src/
│   │   │   ├── index.ts     # Entry point
│   │   │   └── types.ts     # TypeScript types
│   │   └── style.css        # Optional styles
│   └── backend/             # Backend plugin code
│       ├── src/
│       │   └── index.ts     # Entry point
│       └── assets/          # Backend-accessible files
└── dist/                    # Build output
```

- `packages/backend` → Backend plugin code - handles server-side logic, data processing, and API endpoints
- `packages/frontend` → Frontend plugin code - handles UI components, user interactions, and calls to backend using `sdk.backend.functionName`

**Example:** See https://github.com/caido-community/plugin-demo for official demo structure

## Configuration Files

### caido.config.ts

The primary configuration file that defines your plugin package. Uses `defineConfig` from `@caido-community/dev`.

**Reference:** https://developer.caido.io/reference/config

**Common Configuration:**

```typescript
import { defineConfig } from '@caido-community/dev';

export default defineConfig({
  id: 'my-plugin',                    // Unique ID: lowercase, hyphens/underscores
  name: 'My Plugin',                  // Display name
  description: 'Plugin description',
  version: '1.0.0',                   // Semantic versioning
  author: {
    name: 'Your Name',
    email: 'you@example.com',         // Optional
    url: 'https://yoursite.com'       // Optional
  },
  plugins: [
    // Frontend plugin config
    {
      kind: 'frontend',
      id: 'my-plugin-frontend',
      name: 'My Plugin Frontend',
      root: './packages/frontend',
      backend: {
        id: 'my-plugin-backend'       // Link to backend plugin
      }
    },
    // Backend plugin config
    {
      kind: 'backend',
      id: 'my-plugin-backend',
      name: 'My Plugin Backend',
      root: './packages/backend',
      assets: ['assets/**/*']         // (optional) Glob patterns for assets
    }
  ]
});
```

### manifest.json

Auto-generated from `caido.config.ts` during build. Defines plugin metadata and installation structure.

**Reference:** https://developer.caido.io/reference/manifest

**Key Fields:**
- `id` - Unique identifier matching `^[a-z][a-z0-9]+(?:[_-][a-z0-9]+)*$`
- `version` - MAJOR.MINOR.PATCH format
- `plugins` - Array of frontend/backend/workflow definitions
- `author` - Creator information (name, email, url)
- `links` - Sponsor/funding URLs

**Note:** Manually edit only if not using `caido.config.ts`

## Frontend Plugin Development

Frontend plugins create UI components, custom pages, commands, and context menus using Vue.js and the Caido frontend SDK. The Caido Frontend SDK is used for creating UI components, pages, and handling user interactions in Caido plugins.

**SDK Reference:** https://developer.caido.io/reference/sdks/frontend/

### Entry Point

Frontend plugins are initialized via `packages/frontend/src/index.ts`:

### Basic Frontend Plugin Structure

```typescript
import { Caido } from "@caido/sdk-frontend";
import { API, BackendEvents } from "backend";

// Define SDK type with backend API
export type FrontendSDK = Caido<API, BackendEvents>;

// Plugin initialization
export const init = (sdk: FrontendSDK) => {
  // Create pages and UI
  createPage(sdk);

  // Register sidebar items
  sdk.sidebar.registerItem("My Plugin", "/my-plugin-page", {
    icon: "fas fa-rocket"
  });

  // Register commands
  sdk.commands.register("my-command", {
    name: "My Custom Command",
    run: () => sdk.backend.myCustomFunction("Hello"),
  });
};
```

### UI Components

The frontend SDK provides styled components that match Caido's design system:

### HTTP Editors

```typescript
const requestEditor = caido.ui.httpRequestEditor({
  id: 'my-request-editor',
  options: { readOnly: false }
});

const responseEditor = caido.ui.httpResponseEditor({
  id: 'my-response-editor',
  options: { readOnly: true }
});
```

### Registering Custom Pages

Add custom pages to Caido's navigation:

```typescript
sdk.navigation.addPage("/my_plugin", {
  body: root
});
```

**Navigation:**
```typescript
// Navigate programmatically
sdk.navigation.goTo('/my-plugin');

// Listen for page changes
sdk.navigation.onPageChange((path) => {
  console.log('Navigated to:', path);
});
```

### SDK Type Definitions

#### For plugins WITHOUT backend, this is fine:
```typescript
export type FrontendSDK = Caido<Record<string, never>, Record<string, never>>;
```

#### For plugins WITH backend:
```typescript
import { Caido } from "@caido/sdk-frontend";
import { API, BackendEvents } from "backend";

export type FrontendSDK = Caido<API, BackendEvents>;
```

### Command Pattern

Commands provide a unified way to register actions that can be triggered from:
- Command palette (Ctrl/Cmd+Shift+P)
- Context menus (right-click)
- UI buttons
- Keyboard shortcuts

Commands is a frontend-only concept.

```typescript
// Define command IDs as constants
const Commands = {
  processData: "my-plugin.process-data",
  exportResults: "my-plugin.export-results",
} as const;

// Register commands
sdk.commands.register(Commands.processData, {
  name: "Process Data",
  run: async () => {
    const result = await sdk.backend.processData();
    sdk.window.showToast(`Processed ${result.count} items`, {
      variant: "success"
    });
  },
  group: "My Plugin",
});

// Add to command palette
sdk.commandPalette.register(Commands.processData);

// Add to context menus
sdk.menu.registerItem({
  type: "Request",
  commandId: Commands.processData,
  leadingIcon: "fas fa-cog",
});
```

### Context Menus

Add commands to context menus in various Caido views:

```typescript
// Request context menu
sdk.menu.registerItem({
  type: 'Request',
  commandId: 'my-plugin:process-request',
  leadingIcon: 'fas fa-cog'
});

// Response context menu
sdk.menu.registerItem({
  type: 'Response',
  commandId: 'my-plugin:process-response'
});

// Request row context menu (in tables)
sdk.menu.registerItem({
  type: 'RequestRow',
  commandId: 'my-plugin:analyze-request'
});

// Settings menu
sdk.menu.registerItem({
  type: 'Settings',
  commandId: 'my-plugin:open-settings'
});
```

### Data Persistence (Frontend Storage)

Store plugin configuration and data:

```typescript
// Define storage type
type MyStorage = {
  notes: Array<{
    content: string;
    timestamp: string;
  }>;
  settings: {
    enabled: boolean;
  };
};

// Get data
const data = await sdk.storage.get<MyStorage>();

// Set data
await sdk.storage.set<MyStorage>({
  notes: [...],
  settings: { enabled: true }
});

// Listen for changes
sdk.storage.onChange<MyStorage>((newData) => {
  console.log('Storage updated:', newData);
});
```

**Example:** See https://developer.caido.io/tutorials/notebook for a complete storage example

### Backend Communication

Call backend functions and subscribe to events:

```typescript
// Call backend function
const result = await sdk.backend.myBackendFunction('param');

// Subscribe to backend events
sdk.backend.onEvent("my_plugin:request_received", (data) => {
  console.log(data)
});
```

### Error Handling

When calling backend APIs from the frontend, handle Result types gracefully:

```typescript
// Frontend usage - no try/catch needed
const handleProcess = async () => {
  const result = await sdk.backend.processData(inputValue);

  if (result.kind === "Error") {
    sdk.window.showToast(result.error, { variant: "error" });
    return;
  }

  // Handle successful result
  const data = result.value;
  sdk.window.showToast("Processing completed!", { variant: "success" });
};
```


### Environment Variables

Access Caido environment variables:

```typescript
const apiKey = await sdk.env.getVar('API_KEY');
```

Types:

```typescript
/**
 * Utilities to interact with the environment.
 * @category Environment
 */
export type EnvironmentSDK = {
    /**
     * Get the value of an environment variable.
     * @param name The name of the environment variable.
     * @returns The value of the environment variable.
     */
    getVar: (name: string) => string | undefined;
    /**
     * Get all environment variables available in the global environment and the selected environment.
     * @returns All environment variables.
     */
    getVars: () => EnvironmentVariable[];
};

export type EnvironmentVariable = {
    /**
     * The name of the environment variable.
     */
    name: string;
    /**
     * The value of the environment variable.
     */
    value: string;
    /**
     * Whether the environment variable is a secret.
     */
    isSecret: boolean;
};

```

### Page-Specific SDKs

Caido provides specialized SDKs for extending specific pages:

**HTTP History:**
```typescript
sdk.httpHistory.setQuery("resp.raw.cont:\"hello\"")
```

**Search:**
```typescript
sdk.search.setQuery("resp.raw.cont:\"hello\"")
```

**Replay:**
```typescript
const session = await sdk.replay.createSession({
  name: 'My Session',
  requests: [requestId1, requestId2]
});

/**
  * Create a session.
  * @param sessionId The ID of the request to add.
  * @param collectionId The ID of the collection to add the request.
  */
createSession: (source: RequestSource, collectionId?: ID) => Promise<void>;

/**
 * @category Replay
 *
 * @remarks
 * This type is a discriminated union with two possible shapes:
 * - A raw request, containing the raw HTTP request string and connection information.
 * - A reference to an existing request ID.
 *
 * @example
 * // Using a raw request
 * const source: RequestSource = {
 *   type: "Raw",
 *   raw: "GET /api/data HTTP/1.1",
 *   connectionInfo: { ... }
 * };
 * // Using an ID
 * const source: RequestSource = {
 *   type: "ID",
 *   id: "request-123"
 * };
 */

export type RequestSource = {
    type: "Raw";
    raw: string;
    connectionInfo: SendRequestOptions["connectionInfo"];
} | {
    type: "ID";
    id: string;
};

/**
 * The connection information to use for the request.
 */
connectionInfo: {
    /**
     * The host to use for the request.
     */
    host: string;
    /**
     * The port to use for the request.
     */
    port: number;
    /**
     * Whether the request is TLS.
     */
    isTLS: boolean;
    /**
     * The SNI to use for the request.
     * If not provided, the SNI will be inferred from the host.
     */
    SNI?: string;
};
```

**Findings:**
```typescript
const finding = await sdk.findings.createFinding(requestId, {
  title: 'XSS Vulnerability',
  description: 'Reflected XSS found',
  reporter: 'My Plugin'
});
```

**Automate, Intercept, Sitemap:** See SDK docs for specialized methods

### Toasts

**Toasts:**
```typescript
sdk.window.showToast("Hello", {
  variant: 'success',  // "success" | "error" | "warning" | "info";
  duration: 3000
});
```

### Active Editor Access

Get content from the currently focused editor:

```typescript
const editor = sdk.window.getActiveEditor();
if (editor) {
  const content = editor.getContent();
  const selection = editor.getSelection();
}
```

**Complete Frontend SDK Reference:** https://developer.caido.io/reference/sdks/frontend/

## Backend Plugin Development

Backend plugins handle server-side logic, HTTP operations, data processing, findings generation, and more. They run in a QuickJS runtime with Node.js-compatible APIs.

**SDK Reference:** https://developer.caido.io/reference/sdks/backend/

### Entry Point

Backend plugins are initialized via `packages/backend/src/index.ts`:

### Basic Backend Plugin Structure

```typescript
  import { SDK, DefineAPI } from "caido:plugin";

  // Define your API functions
  function myCustomFunction(sdk: SDK, param: string) {
    sdk.console.log(`Called with: ${param}`);
    return `Processed: ${param}`;
  }

  // Export the API type definition
  export type API = DefineAPI<{
    myCustomFunction: typeof myCustomFunction;
  }>;

  // Plugin initialization
  export function init(sdk: SDK<API>) {
    // Register API endpoints
    sdk.api.register("myCustomFunction", myCustomFunction);
  }
```

### SDK Type Definitions

#### Backend SDK with events:
```typescript
  import { DefineEvents, SDK } from "caido:plugin";

  export type BackendEvents = DefineEvents<{
    "data-updated": { message: string };
    "status-changed": { status: "active" | "inactive" };
  }>;

  export type CaidoBackendSDK = SDK<never, BackendEvents>;
```

### Best Practices

When building API endpoints in the backend and calling them from the frontend, use Result types to handle errors gracefully without throwing exceptions:

```typescript
  // Define the Result type
  export type Result<T> =
    | { kind: "Error"; error: string }
    | { kind: "Ok"; value: T };

  // Backend API function returning Result
  function processData(sdk: SDK, input: string): Result<ProcessedData> {
    try {
      // Your processing logic here
      const processed = doSomeProcessing(input);
      return { kind: "Ok", value: processed };
    } catch (error) {
      return { kind: "Error", error: error.message };
    }
  }

  // Frontend usage - no try/catch needed
  const handleProcess = async () => {
    const result = await sdk.backend.processData(inputValue);

    if (result.kind === "Error") {
      sdk.window.showToast(result.error, { variant: "error" });
      return;
    }

    // Handle successful result
    const data = result.value;
    sdk.window.showToast("Processing completed!", { variant: "success" });
  };
```


#### Registering Multiple API Endpoints

```typescript
  // Define multiple API functions
  function getData(sdk: SDK, id: string): Result<Data> {
    // Implementation
  }

  function saveData(sdk: SDK, data: Data): Result<void> {
    // Implementation
  }

  function deleteData(sdk: SDK, id: string): Result<boolean> {
    // Implementation
  }

  // Export Caido Backend API
  export type API = DefineAPI<{
    getData: typeof getData;
    saveData: typeof saveData;
    deleteData: typeof deleteData;
  }>;

  // Register all endpoints
  export function init(sdk: SDK<API>) {
    sdk.api.register("getData", getData);
    sdk.api.register("saveData", saveData);
    sdk.api.register("deleteData", deleteData);
  }
```


### RPC and Frontend Communication
**Send Events to Frontend:**
```typescript
sdk.api.send('eventName', eventData);
```

### Working with Requests and Responses

**Creating and Sending Requests**

```typescript
import { RequestSpec } from "caido:utils";
import { type Request, type Response } from "caido:utils";

// Create a new request
const spec = new RequestSpec("https://api.example.com/data");
spec.setMethod("POST");
spec.setHeader("Content-Type", "application/json");
spec.setBody(JSON.stringify({ key: "value" }));

// Send the request
const result = await sdk.requests.send(spec);
if (result.response) {
  const statusCode = result.response.getCode();
  const responseBody = result.response.getBody()?.toText();
}
```


**Get Saved Requests by ID:**
```typescript
const request = await sdk.requests.get(requestId);
```

**Check Request Scope:**
```typescript
const inScope = await sdk.requests.inScope(requestUrl);
```

**Test HTTPQL Filters:**
```typescript
const matches = await sdk.requests.matches(request, 'ext:php AND status:200');
```

### Request/Response Data Models

**Request:**
```typescript
const method = request.getMethod();
const url = request.getUrl();
const path = request.getPath();
const query = request.getQuery();
const headers = request.getHeaders();
const body = request.getBody();

// Convert body
const bodyText = body?.toText();      // Returns string (unprintable → �)
const bodyJson = body?.toJson();      // Parse as JSON
const bodyRaw = body?.toRaw();        // Get raw bytes

// Get specific header (case-insensitive)
const contentType = request.getHeader('content-type');

// Convert to mutable spec
const spec = request.toSpec();
spec.setHeader('X-Custom', 'value');
```

**Response:**
```typescript
const code = response.getCode();
const statusLine = response.getStatusLine();
const headers = response.getHeaders();
const body = response.getBody();
```

### Intercept Callbacks

Intercept and modify requests/responses in real-time:

```typescript
sdk.events.onInterceptRequest(async (sdk, request) => {
  // Modify request
  const spec = request.toSpec();
  spec.setHeader('X-Intercepted', 'true');
  return spec;
});

sdk.events.onInterceptResponse(async (sdk, response) => {
  // Modify response
  const spec = response.toSpec();
  // Modify as needed
  return spec;
});
```


### Important Caido SDK Types

```typescript
export type Request = {
  getId(): ID;
  getHost(): string;
  getPort(): number;
  getTls(): boolean;
  getMethod(): string;
  getPath(): string;
  getQuery(): string;
  getUrl(): string;
  getHeaders(): Record<string, Array<string>>;
  getHeader(name: string): Array<string> | undefined;
  getBody(): Body | undefined;
  getRaw(): RequestRaw;
  getCreatedAt(): Date;
  toSpec(): RequestSpec;
  toSpecRaw(): RequestSpecRaw;
};

export type Response = {
  getId(): ID;
  getCode(): number;
  getHeaders(): Record<string, Array<string>>;
  getHeader(name: string): Array<string> | undefined;
  getBody(): Body | undefined;
  getRaw(): ResponseRaw;
  getRoundtripTime(): number;
  getCreatedAt(): Date;
};
```

For Body and Raw you can use methods like `getBody()?.toText()` to extract text content.

These types can be imported by:
```
import { type Request, type Response } from "caido:utils";
```

### Data Storage

**SQLite Database:**
```typescript
const db = sdk.meta.db();

// Execute queries with connection pooling
await db.execute('CREATE TABLE IF NOT EXISTS notes (id INTEGER PRIMARY KEY, content TEXT)');
const rows = await db.query('SELECT * FROM notes');
```

**Plugin Data Directory:**
```typescript
const dataPath = sdk.meta.path();  // Plugin-specific writable directory
const assetsPath = sdk.meta.assetsPath();  // Read-only assets from package
```

### Project and Scope

**Current Project:**
```typescript
const project = sdk.projects.getCurrent();
const projectName = project?.getName();
const projectPath = project?.getPath();
```

**Scope Management:**
```typescript
const scopes = await sdk.scope.getAll();

  export type Scope = {
    /**
     * The unique Caido {@link ID} of the scope.
     */
    readonly id: ID;
    /**
     * The name of the scope.
     */
    readonly name: string;
    /**
     * The allowlist of the scope.
     */
    readonly allowlist: string[];
    /**
     * The denylist of the scope.
     */
    readonly denylist: string[];
  };

  /**
   * The SDK for the Scope service.
   * @category Scope
   */
  export type ScopeSDK = {
    /**
     * Get all the scopes.
     * @returns An array of {@link Scope}
     */
    getAll(): Promise<Scope[]>;
  };
```

### GraphQL API

Execute GraphQL queries against Caido's internal API:

```typescript
const result = await sdk.graphql.execute(`
  query {
    requests(limit: 10) {
      nodes {
        id
        method
        url
      }
    }
  }
`);
```

### Process Management

Spawn external processes (e.g., security tools):

```typescript
import { spawn } from 'child_process';

const proc = spawn('nmap', ['-sV', 'target.com']);

proc.stdout.on('data', (data) => {
  sdk.console.log(data.toString());
});

proc.on('close', (code) => {
  sdk.console.log(`Process exited with code ${code}`);
});
```

### Event Handlers

**Project Change:**
```typescript
sdk.events.onProjectChange((sdk, project) => {
  sdk.console.log('Project changed:', project?.name);
});
```

### Console Logging

```typescript
sdk.console.log('Info message');
sdk.console.debug('Debug message');
sdk.console.warn('Warning message');
sdk.console.error('Error message');
```

**Complete Backend SDK Reference:** https://developer.caido.io/reference/sdks/backend/

## Backend Runtime Modules

Backend plugins run in a QuickJS environment with Node.js-compatible modules:

**Available Modules:** https://developer.caido.io/reference/modules/

**Core Modules:**
- `fs`, `fs/promise` - File system operations
- `net` - Networking and sockets
- `caido:http` - Custom fetch implementation
- `process` - Process information
- `child_process` - Spawn subprocesses
- `crypto` - Cryptographic operations
- `buffer` - Binary data handling
- `stream` - Streaming APIs
- `path` - Path operations
- `os` - Operating system info
- `sqlite` - Database access
- `url` - URL utilities

**Global Modules** (no import needed):
- `console`, `timers`, `abort`, `dom-events`, `globals`

**Note:** Some modules use `caido:` prefix (e.g., `caido:http`)

## Workflow SDK

For JavaScript nodes within Caido's workflow system.

**SDK Reference:** https://developer.caido.io/reference/sdks/workflow/

**Key Capabilities:**
- Query and send HTTP requests
- Create security findings
- Execute GraphQL queries
- Access environment variables
- Manage scope configurations
- Work with workflow inputs/outputs

**Workflow Plugins in manifest.json:**
```json
{
  "kind": "workflow",
  "id": "my-workflow",
  "definition": "workflow-definition.json"
}
```

**Community Workflows:** https://github.com/caido-community/workflows

## Development Workflow

### Hot Reload with Devtools

For rapid iteration without rebuilding:

1. **Install Devtools** from the Caido Community Store

2. **Run Watch Mode:**
```bash
pnpm watch
```

3. **Connect to Dev Server** in Devtools within Caido

Changes are automatically reloaded in real-time.

**Reference:** https://developer.caido.io/guides/ (Development Workflow section)

### Building and Testing

**Build:**
```bash
pnpm build
```

**Output:** `dist/plugin_package.zip`

**Install in Caido:**
1. Open Caido
2. Navigate to Plugins
3. Click "Install from file"
4. Select `plugin_package.zip`

### Pre-Commit Checks

**IMPORTANT:** Always run these commands before committing your code:

```bash
pnpm lint
pnpm typecheck
```

These commands ensure code quality and catch type errors before they make it into version control. Make it a habit to run both commands before every commit.

### Debugging

Use `sdk.console` methods for logging:
- Backend: Logs appear in Caido's backend console
- Frontend: Logs appear in browser DevTools

## Distribution

### Publishing to Community Store

1. **Setup Repository:** Follow guidelines at https://developer.caido.io/guides/distribution/repository

2. **Submit Plugin:** Follow process at https://developer.caido.io/guides/distribution/store

3. **Plugin Requirements:**
   - Valid `manifest.json` with all required fields
   - Proper versioning (semantic versioning)
   - Author information
   - Optional: Sponsor link

### Plugin Signing

Caido supports plugin signing for security. Refer to SDK documentation for signing requirements.

## Popular Plugin Examples

Learn from existing community plugins to understand implementation patterns:

### General Reference Plugins

- **plugin-demo** (Official Demo): https://github.com/caido-community/plugin-demo
  - Demonstrates frontend architecture with Vue.js and TypeScript
  - Monorepo structure with pnpm workspaces
  - PrimeVue component integration
  - **Use when:** Building frontend-heavy plugins with UI components

- **Notebook** (Tutorial Example): https://developer.caido.io/tutorials/notebook
  - Note-taking plugin with persistent storage
  - Command palette integration
  - Context menu registration
  - Storage synchronization patterns
  - **Use when:** Implementing data persistence and UI integration

### Specialized Security Plugins
Utilize these as references for adding specific features, so you will know how to implement similar functionality.

- **quickssrf** (31 stars): https://github.com/caido-community/quickssrf
  - SSRF vulnerability detection
  - Integration with external APIs (Interactsh)
  - Monorepo TypeScript/Vue architecture
  - **Use when:** Building vulnerability detection plugins or integrating external security services

- **scanner** (28 stars): https://github.com/caido-community/scanner
  - Vulnerability scanning capabilities
  - **Use when:** Implementing active security scanning features

- **shift** (30 stars): https://github.com/caido-community/shift
  - AI integration into Caido
  - **Use when:** Adding AI/LLM capabilities to plugins

- **authmatrix** (6 stars): https://github.com/caido-community/authmatrix
  - Authorization testing across multiple users
  - **Use when:** Building multi-user authentication/authorization testing tools

### Workflow and Automation

- **workflows** (82 stars): https://github.com/caido-community/workflows
  - Community-created automation workflows
  - **Use when:** Creating workflow plugins or JavaScript workflow nodes

### Development Tools

- **devtools**: https://github.com/caido-community/devtools
  - Hot-reloading during development
  - **Use when:** Setting up development environment

- **create-plugin**: https://github.com/caido-community/create-plugin
  - CLI for initializing plugins
  - **Use when:** Starting new plugin projects

## Common Patterns and Best Practices

### Plugin Architecture Patterns

1. **Frontend-Only Plugins:** UI enhancements, visual tools, reporting interfaces
2. **Backend-Only Plugins:** HTTP processing, external tool integration, background tasks
3. **Full-Stack Plugins:** Complex features requiring both UI and server-side logic

### TypeScript Type Safety

Always define types for backend API and events:

```typescript
// Frontend
type API = {
  functionName: (param: ParamType) => Promise<ReturnType>;
};

type Events = {
  eventName: (data: EventDataType) => void;
};

const caido: Caido<API, Events>;
```

### Error Handling

```typescript
try {
  const result = await sdk.requests.send({ request: spec });
  // Process result
} catch (error) {
  sdk.console.error('Request failed:', error);
  sdk.api.send('error', { message: error.message });
}
```

### Performance Considerations

- Use pagination for large request queries
- Implement timeouts for HTTP requests
- Debounce UI events that trigger backend calls
- Cache frequently accessed data in storage

### Security Best Practices

- Validate all user input
- Use `secret: true` for sensitive environment variables
- Implement proper error handling without leaking sensitive data
- Use deduplication keys for findings to avoid duplicates
- Follow principle of least privilege for external integrations

## Troubleshooting

### Common Issues

**Plugin Not Loading:**
- Check manifest.json syntax
- Verify plugin ID format (lowercase, hyphens/underscores only)
- Ensure version follows semantic versioning
- Check console for errors

**Hot Reload Not Working:**
- Verify Devtools is installed
- Ensure `pnpm watch` is running
- Check connection URL in Devtools matches watch server

**Backend/Frontend Communication Failing:**
- Verify API function registration matches frontend calls
- Check type definitions match between frontend and backend
- Ensure backend plugin ID is correctly referenced in frontend config

**Build Failures:**
- Run `pnpm install` to ensure dependencies are up-to-date
- Check TypeScript errors with `pnpm tsc`
- Verify caido.config.ts syntax

### Getting Help

- **Developer Docs:** https://developer.caido.io/
- **Discord Community:** Active support and collaboration
- **GitHub Issues:** Plugin-specific issues on respective repos
- **LLM Docs:** https://developer.caido.io/llms.txt for AI assistance

## Quick Reference Links

**Official Documentation:**
- Developer Portal: https://developer.caido.io/
- Main Docs: https://docs.caido.io/
- LLM-Optimized Docs: https://developer.caido.io/llms.txt

**SDK References (Always Check for Latest APIs):**
- Frontend SDK: https://developer.caido.io/reference/sdks/frontend/
- Backend SDK: https://developer.caido.io/reference/sdks/backend/
- Workflow SDK: https://developer.caido.io/reference/sdks/workflow/
- Runtime Modules: https://developer.caido.io/reference/modules/

**Configuration Files:**
- caido.config.ts: https://developer.caido.io/reference/config
- manifest.json: https://developer.caido.io/reference/manifest
- plugin_packages.json: https://developer.caido.io/reference/plugin_packages.md

**Guides:**
- Getting Started: https://developer.caido.io/guides/
- Repository Setup: https://developer.caido.io/guides/distribution/repository
- Store Submission: https://developer.caido.io/guides/distribution/store

**Community Resources:**
- Community GitHub: https://github.com/caido-community
- Popular Plugins: quickssrf, scanner, shift, authmatrix, workflows
- Example Plugins: plugin-demo, Notebook tutorial

**Development Tools:**
- Starter CLI: `pnpm create @caido-community/plugin`
- Devtools: https://github.com/caido-community/devtools

---

**Note:** Caido's plugin ecosystem is actively developed. Always reference the official documentation links above for the most up-to-date API information, as SDKs and capabilities may evolve over time.

## TypeScript Guidelines

- Use TypeScript for all files.
- NEVER use `any` type.
- Use `undefined` over `null`.
- Try to keep things in one function unless composable or reusable.
- Prefer single word variable names where possible.
- DO NOT do unnecessary destructuring of variables.
- AVOID `else` statements where possible.
- AVOID `try` / `catch` where possible.
- AVOID using interfaces where possible.

## UI Style Guidelines

### PrimeVue

- Prefer to use PrimeVue compontents where possible
- A custom PrimeVue theme is configured with dark mode as default, handling most color-related styles for us.

### General Theme

- Dark Mode is default — all UI elements follow a dark, low-contrast background with light text for high readability.
- For text / background colors, prefer to use `...-surface-...` f.e. `border-surface-700`.
- Caido uses `bg-surface-800` as the main app background, `bg-surface-700` is the background used for `Card` component.
- Follow minimalistic color palette. AVOID using too much colors where possible.

### Layout & Components

- Often use PrimeVue `Splitter` and `SplitterPanel` for vertical or horizontal layout.
- Prefer to use `Card` PrimeVue components a lot, if needed add `h-full` to them via `pt` params.
- Remember to use `<template #content>` and other named slots (like `#header`, `#footer`) in Card and other PrimeVue components that support them.


Example:

```
<Card
  class="h-full"
  :pt="{
    body: { class: 'h-full p-0' },
    content: { class: 'h-full flex flex-col' },
  }"
>
  <template #content>
     ...
  </template #content>
</Card>
```

### Enviroment

- Keep in mind that we are building a plugin that's inside a Caido web app, we can modify frontend by adding sidebar pages using Caido Frontend SDK.
- Our plugin content is rendered within a dedicated window/panel that Caido provides for our sidebar page.
- The plugin UI should integrate seamlessly with Caido's existing interface and theming.

### Data Representation

- Prefer `DataTable` component for displaying structured data:
  * Caido often uses `stripedRows`, use it where possible
  * Actions column at the end (e.g. “Install” buttons).
- Empty states use friendly, minimal messages with icons.

### Icons

- Always use `fas fa-[...]` for icons. We don't support any other icon libraries.

# Caido SDK API Validation

## Valid API Usage

Always use documented Caido SDK APIs directly without runtime checks or validation.

- Good
  ```typescript
  sdk.window.showToast("Message", { variant: "error" });
  sdk.backend.myFunction();
  sdk.commands.register("my-command", { name: "Command", run: () => {} });
  ```

- Bad (NEVER do this)
  ```typescript
  if ("showToast" in sdk.window && typeof sdk.window.showToast === "function") {
    sdk.window.showToast("Message");
  }
  ```

## Rules

1. Never use runtime API existence checks (`typeof`, `in` operator)
2. Never assume undocumented SDK methods exist
3. If unsure about an API, ask for clarification rather than guessing

The SDK is typed and guaranteed to have the documented methods available.

# Linter

We have a built-in ESLint linter configured at the root folder. After making any significant change, always run the linter with `pnpm lint` and fix all potential issues.

## Most common mistakes that lead to linter errors

### Lint Rule: Unexpected nullable string value in conditional. Please handle nullish or empty cases explicitly

To prevent this, when comparing strings, instead of writing:

```
if (!str) {}
```

do this:

```
if (str !== undefined) {}
```

# Components

1. Use Primevue components for all UI components.
2. Use `<script setup lang="ts">` for all components.

## Structure

Use the following layout for every component to keep imports and growth consistent:

```text
ComponentName/
  ├─ index.ts           # Re-export (single public entry)
  ├─ Container.vue      # Main component implementation
  ├─ useForm.ts         # Optional: composable when logic grows (e.g. forms)
  └─ DependentComponent.vue
```

ComponentName/index.ts
```ts
export { default as ComponentName } from "./Container.vue";
```

When a child piece becomes complex or needs its own hook, use the same pattern as the parent:

```text
ComponentName/
  └─ DependentComponent/
       ├─ index.ts
       └─ Container.vue
```
