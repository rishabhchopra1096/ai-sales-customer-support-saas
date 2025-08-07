# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Core Development Workflow
- `pnpm dev` - Start all applications in development mode (runs turbo dev)
- `pnpm build` - Build all applications and packages (runs turbo build)  
- `pnpm lint` - Lint all packages (runs turbo lint)
- `pnpm format` - Format all TypeScript, TSX, and Markdown files with Prettier

### Individual App Commands
**Web App (Main Dashboard - Port 3000):**
- `cd apps/web && pnpm dev` - Start web app development server
- `cd apps/web && pnpm typecheck` - Type check the web application
- `cd apps/web && pnpm lint:fix` - Auto-fix linting issues

**Widget App (Customer Widget - Port 3001):**  
- `cd apps/widget && pnpm dev` - Start widget app with turbopack
- `cd apps/widget && pnpm typecheck` - Type check the widget application
- `cd apps/widget && pnpm lint:fix` - Auto-fix linting issues

## Architecture Overview

This is a **Next.js 15 monorepo** built with **Turbo** that creates an AI-powered customer support platform with multiple applications:

### Applications Structure
1. **Web App (`apps/web`)** - Main dashboard application for organizations
   - Authentication via Clerk
   - Organization management
   - Conversation management and monitoring
   - File uploads and knowledge base management
   - Plugin integrations (Vapi for voice AI)
   - Billing and customization settings

2. **Widget App (`apps/widget`)** - Embeddable customer support widget
   - Multiple interaction modes: chat, voice, contact forms
   - Session-based authentication
   - Real-time conversation interface
   - Vapi integration for voice interactions

### Core Packages
- **Backend (`packages/backend`)** - Convex backend with AI agents
- **UI (`packages/ui`)** - Shared shadcn/ui components library
- **Math (`packages/math`)** - Shared utility functions
- **ESLint Config** - Shared linting configuration
- **TypeScript Config** - Shared TypeScript configurations

### Backend Architecture (Convex)
The backend uses **Convex** as a real-time database and backend platform with:

- **AI Agent System**: GPT-4o-mini powered support agent with RAG capabilities
- **Database Schema**: Organizations, conversations, contact sessions, plugins
- **Real-time Updates**: Automatic syncing between dashboard and widget
- **Plugin System**: Extensible integrations (currently supports Vapi)

### Key Technology Stack
- **Frontend**: Next.js 15, React 19, TypeScript, Tailwind CSS
- **Backend**: Convex (real-time database + functions)
- **AI**: OpenAI GPT-4o-mini, Convex Agent framework, RAG system
- **Authentication**: Clerk (web app), session-based (widget)  
- **State Management**: Jotai atoms
- **UI Components**: shadcn/ui with custom extensions
- **Voice AI**: Vapi integration
- **Monorepo**: Turbo, pnpm workspaces

### Module Organization Pattern
Both applications follow a consistent module structure:
```
modules/
  [feature]/
    ui/
      components/  # Reusable UI components
      layouts/     # Page layouts
      views/       # Page-level views
      screens/     # Screen components (widget)
    atoms.ts       # Jotai state atoms
    constants.ts   # Feature constants
    hooks/         # Custom React hooks
```

### Development Workflow
1. The monorepo uses **pnpm** for package management and **Turbo** for build orchestration
2. Shared components live in `packages/ui` and are imported as `@workspace/ui`
3. Backend functions in `packages/backend/convex` are deployed to Convex
4. Both apps share the same backend but serve different user types
5. The widget is designed to be embedded in external websites

### Key Integration Points
- **Convex Real-time**: Both apps subscribe to the same Convex backend
- **Shared UI**: Common components reduce duplication
- **Plugin Architecture**: Extensible third-party service integrations
- **AI Agent**: Handles customer inquiries with knowledge base search
- **Session Management**: Widget uses temporary sessions, web app uses persistent auth

## Component Imports
Use workspace protocol for shared packages:
- `@workspace/ui/components/[component]` - UI components
- `@workspace/backend` - Convex functions and schema
- `@workspace/math` - Utility functions

## Adding shadcn/ui Components
Run from repository root to add to the shared UI package:
```bash
pnpm dlx shadcn@latest add [component] -c apps/web
```
Components are placed in `packages/ui/src/components` and can be imported by both applications.