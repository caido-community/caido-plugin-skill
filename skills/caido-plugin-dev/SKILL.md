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

Caido uses HTTPQL for filtering requests throughout the application. Common query examples:
- `ext:json` - Filter by file extension
- `status:200` - Filter by status code
- `method:POST` - Filter by HTTP method
- `ext:php AND status:200` - Combine filters with boolean operators

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

Plugins can combine frontend and backend components in a single package.

## Quick Start: Creating a New Plugin

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
      assets: ['assets/**/*']         // Glob patterns for assets
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

Frontend plugins create UI components, custom pages, commands, and context menus using Vue.js and the Caido frontend SDK.

**SDK Reference:** https://developer.caido.io/reference/sdks/frontend/

### Basic Frontend Plugin Structure

```typescript
import type { Caido } from '@caido/sdk-frontend';

// Define backend API types (if connecting to backend)
type API = {
  myBackendFunction: (param: string) => Promise<string>;
};

// Define backend event types
type Events = {
  onDataUpdate: (data: any) => void;
};

export function init(caido: Caido<API, Events>) {
  // Plugin initialization
}
```

### UI Components

The frontend SDK provides styled components that match Caido's design system:

**Button:**
```typescript
const button = caido.ui.button({
  variant: 'primary',  // 'primary' | 'secondary' | 'tertiary'
  label: 'Click Me',
  size: 'medium',      // 'small' | 'medium' | 'large'
  leadingIcon: 'fas fa-check',  // FontAwesome class
  trailingIcon: 'fas fa-arrow-right'
});
```

**Card:**
```typescript
const card = caido.ui.card({
  header: 'Card Title',
  body: '<div>Card content</div>',
  footer: '<button>Action</button>'
});
```

**Well:**
```typescript
const well = caido.ui.well({
  header: 'Well Title',
  body: '<div>Content</div>',
  footer: '<div>Footer</div>'
});
```

**HTTP Editors:**
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
caido.navigation.addPage('/my-plugin', {
  body: card  // Your UI component
});
```

**Navigation:**
```typescript
// Navigate programmatically
caido.navigation.goTo('/my-plugin');

// Listen for page changes
caido.navigation.onPageChange((path) => {
  console.log('Navigated to:', path);
});
```

### Command System

Register commands that appear in the command palette and can be bound to shortcuts or menu items:

```typescript
caido.commands.register('my-plugin:do-something', {
  name: 'Do Something',
  run: () => {
    console.log('Command executed!');
  },
  when: () => true  // Conditional availability
});
```

**Command Palette:**
```typescript
caido.commandPalette.register('my-plugin:do-something');
```

**Keyboard Shortcuts:**
```typescript
caido.shortcuts.register('my-plugin:do-something', {
  key: 'Ctrl+Shift+D',
  when: () => true
});
```

### Context Menus

Add commands to context menus in various Caido views:

```typescript
// Request context menu
caido.menu.registerItem({
  type: 'Request',
  commandId: 'my-plugin:process-request',
  leadingIcon: 'fas fa-cog'
});

// Response context menu
caido.menu.registerItem({
  type: 'Response',
  commandId: 'my-plugin:process-response'
});

// Request row context menu (in tables)
caido.menu.registerItem({
  type: 'RequestRow',
  commandId: 'my-plugin:analyze-request'
});

// Settings menu
caido.menu.registerItem({
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
const data = await caido.storage.get<MyStorage>();

// Set data
await caido.storage.set<MyStorage>({
  notes: [...],
  settings: { enabled: true }
});

// Listen for changes
caido.storage.onChange<MyStorage>((newData) => {
  console.log('Storage updated:', newData);
});
```

**Example:** See https://developer.caido.io/tutorials/notebook for a complete storage example

### Backend Communication

Call backend functions and subscribe to events:

```typescript
// Call backend function
const result = await caido.backend.myBackendFunction('param');

// Subscribe to backend events
caido.backend.onDataUpdate((data) => {
  console.log('Received from backend:', data);
});
```

### Environment Variables

Access Caido environment variables:

```typescript
const apiKey = await caido.environment.getVar('API_KEY');
```

### Page-Specific SDKs

Caido provides specialized SDKs for extending specific pages:

**HTTP History:**
```typescript
caido.httpHistory.query({
  filter: 'ext:json',  // HTTPQL query
  scope: 'in-scope',
  order: { by: 'time', order: 'desc' }
});
```

**Search:**
```typescript
caido.search.query('ext:php AND status:200');
```

**Replay:**
```typescript
const session = await caido.replay.createSession({
  name: 'My Session',
  requests: [requestId1, requestId2]
});
```

**Findings:**
```typescript
const finding = await caido.findings.create({
  title: 'XSS Vulnerability',
  description: 'Reflected XSS found',
  reporter: 'My Plugin',
  request: requestId
});
```

**Automate, Intercept, Sitemap:** See SDK docs for specialized methods

### UI Slots

Extend existing Caido pages with custom components in predefined slots:

```typescript
// Replay toolbar slots
caido.ui.addSlot(ReplaySlot.SessionToolbarPrimary, button);
caido.ui.addSlot(ReplaySlot.Topbar, customComponent);

// Footer slots
caido.ui.addSlot(FooterSlot.FooterSlotPrimary, component);
```

### Dialogs and Toasts

**Dialogs:**
```typescript
caido.window.showDialog({
  title: 'My Dialog',
  content: '<div>Dialog content</div>',
  modal: true,
  draggable: true
});
```

**Toasts:**
```typescript
caido.window.showToast({
  message: 'Operation successful',
  variant: 'success',  // 'success' | 'error' | 'warning' | 'info'
  duration: 3000
});
```

### Active Editor Access

Get content from the currently focused editor:

```typescript
const editor = caido.window.getActiveEditor();
if (editor) {
  const content = editor.getContent();
  const selection = editor.getSelection();
}
```

**Complete Frontend SDK Reference:** https://developer.caido.io/reference/sdks/frontend/

## Backend Plugin Development

Backend plugins handle server-side logic, HTTP operations, data processing, findings generation, and more. They run in a QuickJS runtime with Node.js-compatible APIs.

**SDK Reference:** https://developer.caido.io/reference/sdks/backend/

### Basic Backend Plugin Structure

```typescript
import type { SDK } from '@caido/sdk-backend';

// Define API functions exposed to frontend
type API = {
  greet: typeof greet;
  processData: typeof processData;
};

// Define events sent to frontend
type Events = {
  dataUpdated: (data: any) => void;
  progress: (percent: number) => void;
};

function greet(sdk: SDK<API, Events>, name: string): string {
  return `Hello, ${name}!`;
}

function processData(sdk: SDK<API, Events>, data: string): Promise<string> {
  // Send progress event to frontend
  sdk.api.send('progress', 50);

  // Process data...
  const result = data.toUpperCase();

  sdk.api.send('dataUpdated', result);
  return Promise.resolve(result);
}

export function init(sdk: SDK<API, Events>) {
  // Register API functions
  sdk.api.register('greet', greet);
  sdk.api.register('processData', processData);

  // Setup event handlers, workflows, etc.
  sdk.console.log('Backend plugin initialized');
}
```

### RPC and Frontend Communication

**Register Backend Functions:**
```typescript
sdk.api.register('myFunction', (sdk, param1, param2) => {
  // Function implementation
  return result;
});
```

**Send Events to Frontend:**
```typescript
sdk.api.send('eventName', eventData);
```

### HTTP Request Operations

**Query Saved Requests:**
```typescript
const results = await sdk.requests.query({
  filter: 'ext:json',  // HTTPQL query
  order: { by: 'time', order: 'desc' },
  limit: 100,
  offset: 0
});
```

**Send HTTP Requests:**
```typescript
import { RequestSpec } from '@caido/sdk-backend';

const spec = new RequestSpec('https://example.com');
spec.setMethod('POST');
spec.setHeader('Content-Type', 'application/json');
spec.setBody(JSON.stringify({ key: 'value' }));

const response = await sdk.requests.send({
  request: spec,
  options: {
    timeout: 5000,
    saveInProject: true
  }
});

// Access response
const bodyText = response.getBody()?.toText();
const statusCode = response.getCode();
const headers = response.getHeaders();
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

### Security Findings

Create and manage security findings:

```typescript
const finding = await sdk.findings.create({
  title: 'SQL Injection',
  description: 'SQL injection vulnerability detected in login form',
  reporter: 'my-plugin',
  request: requestId,
  dedupeKey: 'sqli-login-form'  // Prevents duplicates
});

// Get findings
const findings = await sdk.findings.get({ request: requestId });

// Check if finding exists
const exists = await sdk.findings.exists({ dedupeKey: 'sqli-login-form' });
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

### Environment Variables

**Get Variables:**
```typescript
const apiKey = await sdk.env.getVar('API_KEY');
const allVars = await sdk.env.getVars();
```

**Set Variables:**
```typescript
await sdk.env.setVar({
  key: 'MY_VAR',
  value: 'my-value',
  scope: 'global',     // 'global' or 'project'
  secret: false        // true for encryption
});
```

### Project and Scope

**Current Project:**
```typescript
const project = sdk.meta.project();
const projectName = project?.name;
const projectPath = project?.path;
```

**Scope Management:**
```typescript
const allowlist = await sdk.scopes.getAllowlist();
const denylist = await sdk.scopes.getDenylist();
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

### Runtime Information

```typescript
const version = sdk.meta.runtime().version;  // Caido version
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
