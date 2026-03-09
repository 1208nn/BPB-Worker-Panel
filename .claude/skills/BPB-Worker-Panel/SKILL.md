# BPB-Worker-Panel Development Patterns

> Auto-generated skill from repository analysis

## Overview

BPB-Worker-Panel is a TypeScript-based application that provides a web panel interface for managing proxy protocols. The codebase includes implementations for multiple proxy protocols (Clash, Sing-box, Xray) with WebSocket support, and maintains bilingual documentation in English and Farsi. The project follows a modular architecture with separate cores for different protocol handlers and a web-based management panel.

## Coding Conventions

### File Naming
- Use camelCase for file names
- Example: `protocolHandler.ts`, `webSocketUtils.ts`

### Import Style
- Mixed import patterns are used throughout the codebase
- Prefer explicit imports for better tree-shaking

```typescript
// Named imports for utilities
import { handleWebSocket, parseConfig } from './common';

// Default imports for main modules
import ConfigHandler from '../cores/clash/config';
```

### Export Style
- Use named exports consistently
- Avoid default exports except for main module entry points

```typescript
// Preferred approach
export const createVlessConfig = () => { ... };
export const parseVlessUrl = () => { ... };

// Instead of
export default { createVlessConfig, parseVlessUrl };
```

## Workflows

### Release Version Bump
**Trigger:** When someone wants to publish a new release
**Command:** `/release`

1. Update `RELEASE.md` with detailed changelog including:
   - New features added
   - Bug fixes implemented
   - Breaking changes (if any)
2. Increment version number in `package.json` following semantic versioning
3. Update version display in `src/assets/panel/index.html`
4. Ensure all version references are synchronized across files

```json
// package.json
{
  "version": "2.3.5"
}
```

```html
<!-- src/assets/panel/index.html -->
<div class="version">v2.3.5</div>
```

### Multilingual Documentation Update
**Trigger:** When someone wants to update documentation
**Command:** `/docs-update`

1. Create or update English documentation in `docs/en/docs/`
2. Translate and update corresponding Farsi documentation in `docs/fa/docs/`
3. Update main `README.md` with English content
4. Update `README_fa.md` with Farsi translation
5. Ensure documentation structure matches between languages

```markdown
<!-- docs/en/docs/configuration/basic.md -->
# Basic Configuration
This section covers basic configuration options...

<!-- docs/fa/docs/configuration/basic.md -->
# پیکربندی اولیه  
این بخش گزینه‌های پیکربندی اولیه را پوشش می‌دهد...
```

### Core Protocol Refactoring
**Trigger:** When someone wants to improve or restructure core protocol handling
**Command:** `/refactor-protocols`

1. Identify protocol handlers that need updates in `src/cores/`
2. Update type definitions in `src/types/*.d.ts` to match new structure
3. Refactor Clash protocol handlers in `src/cores/clash/`
4. Update Sing-box implementations in `src/cores/sing-box/`
5. Modify Xray protocol handlers in `src/cores/xray/`
6. Ensure all protocol variants maintain consistent interfaces

```typescript
// src/types/protocol.d.ts
interface ProtocolConfig {
  type: 'vless' | 'trojan' | 'vmess';
  transport: 'ws' | 'grpc' | 'tcp';
  security: 'tls' | 'none';
}

// src/cores/clash/vless.ts
export const generateVlessConfig = (config: ProtocolConfig) => {
  // Implementation
};
```

### WebSocket Protocol Improvements
**Trigger:** When someone wants to improve WebSocket protocol handling
**Command:** `/update-websocket`

1. Update shared utilities in `src/protocols/websocket/common.ts`
2. Enhance VLESS WebSocket implementation in `src/protocols/websocket/vless.ts`
3. Improve Trojan WebSocket handling in `src/protocols/websocket/trojan.ts`
4. Test WebSocket connections across all supported protocols

```typescript
// src/protocols/websocket/common.ts
export const createWebSocketHeaders = (host: string) => {
  return {
    'Host': host,
    'Upgrade': 'websocket',
    'Connection': 'Upgrade'
  };
};
```

### Panel Feature Addition
**Trigger:** When someone wants to add a new feature to the panel
**Command:** `/add-panel-feature`

1. Add HTML structure for new feature in `src/assets/panel/index.html`
2. Implement JavaScript functionality in `src/assets/panel/script.js`
3. Create or update handlers in `src/common/handlers.ts`
4. Document the new feature in `RELEASE.md`

```html
<!-- src/assets/panel/index.html -->
<div class="feature-section">
  <h3>New Feature</h3>
  <button id="newFeatureBtn">Activate Feature</button>
</div>
```

```javascript
// src/assets/panel/script.js
document.getElementById('newFeatureBtn').addEventListener('click', () => {
  // Feature implementation
});
```

### Configuration Documentation Sync
**Trigger:** When someone wants to document configuration changes
**Command:** `/update-config-docs`

1. Update English configuration docs in `docs/en/docs/configuration/`
2. Update Farsi configuration docs in `docs/fa/docs/configuration/`
3. Add changelog entry to `RELEASE.md`
4. Update or add screenshot images in `docs/assets/images/` if UI changes are involved

```markdown
<!-- docs/en/docs/configuration/advanced.md -->
## Advanced Settings

### Protocol Configuration
Configure your preferred protocol settings:

- **VLESS**: Recommended for better performance
- **Trojan**: Good compatibility with firewalls
```

## Testing Patterns

The project uses a standard testing approach with files matching the `*.test.*` pattern. When adding new features:

1. Create test files alongside implementation files
2. Use descriptive test names that explain the expected behavior
3. Test both success and error scenarios
4. Include integration tests for protocol implementations

```typescript
// protocolHandler.test.ts
describe('Protocol Handler', () => {
  it('should generate valid VLESS configuration', () => {
    // Test implementation
  });

  it('should handle invalid input gracefully', () => {
    // Error handling test
  });
});
```

## Commands

| Command | Purpose |
|---------|---------|
| `/release` | Bump version and prepare release across all files |
| `/docs-update` | Update documentation in both English and Farsi |
| `/refactor-protocols` | Refactor protocol implementations and types |
| `/update-websocket` | Improve WebSocket protocol handling |
| `/add-panel-feature` | Add new functionality to web panel |
| `/update-config-docs` | Sync configuration documentation with releases |