# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a minimalist, zero-dependency text editor that runs entirely in the browser. All content is stored in the URL hash using deflate compression. The application is a Progressive Web App (PWA) with offline support.

## Architecture

### Single-File Design
The entire application is contained in two standalone HTML files:
- `index.html` - Main editor interface (755 lines)
- `qr.html` - QR code generator for sharing URLs

Both files are completely self-contained with inline CSS and JavaScript - no build process, no external dependencies, no package.json.

### Key Components

**Editor System (`Editor` function in index.html:477-753)**
- Custom contenteditable implementation with plaintext-only mode
- Built-in syntax highlighting for Markdown via the `parseMarkdown` function (index.html:370-441)
- Custom undo/redo system that maintains cursor position (history stored in memory)
- Debounced highlighting (30ms) and auto-save (500ms)

**Data Flow**
1. User types → `input` event → debounced `save()` → compress → update URL hash + localStorage
2. URL changes → `hashchange` event → `load()` → decompress → update editor content
3. Style attributes on `<article>` are serialized into the hash using `\x00` as separator

**Compression/Storage**
- Uses browser's native `CompressionStream`/`DecompressionStream` with 'deflate-raw'
- Base64url encoding via `Uint8Array.toBase64()` and `Uint8Array.fromBase64()`
- Format: `#<base64url_compressed_data>` or `#<base64url_compressed_content\x00style>`
- Dual storage: URL hash (shareable) + localStorage (persistence)

**Markdown Parser (`parseMarkdown` in index.html:370-441)**
- Regex-based matcher system, not a full parser
- Applies CSS classes (e.g., `md-h1`, `md-bold`, `md-url`) to spans
- Handles: headers (h1-h6), code blocks, inline code, bold, italic, strikethrough, URLs
- No actual HTML rendering - visual styling only

**Editor State Management**
The `Editor` function maintains:
- Cursor position via custom `save()`/`restore()` methods that traverse the DOM tree
- History stack (max 10,000 entries) with HTML snapshots and cursor positions
- Event listeners for keyboard shortcuts (Cmd/Ctrl+Z for undo, Cmd/Ctrl+Shift+Z for redo)

**Service Worker (`sw.js`)**
- Simple cache-first strategy
- Cache name includes date for versioning: `doodleboard-2026-01-04`
- Caches both `/` and `/qr` routes
- Auto-cleanup of old caches on activation

### Important Implementation Details

**Keyboard Shortcuts**
- Uses `event.code` (not `event.key`) for consistency across layouts (see index.html:254, 724-729)
- Cmd+S / Ctrl+S: Download HTML file
- Cmd+Z / Ctrl+Z: Undo
- Cmd+Shift+Z / Ctrl+Shift+Z: Redo

**Title Extraction**
- If document starts with `# Title`, that becomes the page title
- Regex: `/^\n*#(.+)\n/` (index.html:303)

**Download Feature (index.html:335-368)**
- Clones entire DOM, removes scripts and `.noprint` elements
- Uses File System Access API (`showSaveFilePicker`) if available
- Falls back to blob download with `<a>` tag

**UI Elements**
- Floating action button (FAB) with ripple effect
- Menu with: New document, QR code generator, GitHub link
- QR link dynamically updated with current hash (index.html:451)

## Development Commands

This project has no build system or dependencies. To work with it:

**Local Development**
```bash
# Serve locally (any static server works)
python3 -m http.server 8000
# or
npx serve
```

**Testing**
- Open in browser and test manually
- Test compression/decompression in browser console
- Verify PWA installation and offline functionality

**Updating Service Worker**
When making changes, update the cache version in `sw.js`:
```javascript
const CACHE_NAME = 'doodleboard-YYYY-MM-DD'
```

## Code Style

- Pure vanilla JavaScript (ES6+)
- No semicolons (ASI - Automatic Semicolon Insertion)
- contenteditable="plaintext-only" for security (prevents HTML injection)
- Event delegation and debouncing patterns throughout
- Browser APIs used: CompressionStream, File System Access API, Service Worker, Web App Manifest

## Key Constraints

- Must remain dependency-free and self-contained
- All functionality in inline scripts (no external JS files)
- PWA-compatible (manifest.json, service worker, offline support)
- Mobile-responsive with proper viewport and touch handling
- Dark mode support via `prefers-color-scheme`
