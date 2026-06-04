# DevLifecycle Manager

A technical project-management tool for tracking the maturity of software projects and their features through a defined lifecycle. Each feature moves through states (Backlog, Creating, Fix/Polish, Expanding, Stable), and features can be nested into sub-features to arbitrary depth. Projects capture scope, goal, repository URL, and a description, and the whole board is persisted locally in the browser. The app is built with React and TypeScript and supports drag-and-drop reordering, JSON import/export, and AI-assisted project breakdown (via Google Gemini) for turning a free-text project description into a structured set of features.

## Features

- Track multiple projects, each with a title, repo URL, description, scope, and goal
- Lifecycle states per feature: `BACKLOG`, `CREATING`, `FIX/POLISH`, `EXPANDING`, `STABLE`, shown with color-coded badges
- Nested sub-features (recursive tree) with per-feature notes and attached context files
- Aggregate statistics that roll up feature counts (total, stable, creating, polishing, backlog) across the tree
- Drag-and-drop reordering of features (powered by `@dnd-kit`)
- Create-project modal and a feature detail modal for editing
- Local persistence via `localStorage` (no backend required)
- JSON import/export of project data, with migration handling for older saved formats
- AI-assisted project analysis: the Create Project modal sends a free-text description to a Gemini-backed service (`analyzeProjectRequirements`) to auto-generate title, scope, goal, and an initial feature list; the feature detail modal can attach context files for Gemini

> Note: the AI integration is wired through the UI and expects a `services/geminiService.ts` module and a `GEMINI_API_KEY`. That service module is not currently included in this repository, so the AI-assisted flows require providing it (and adding the `@google/genai` dependency) before they will run.

## Tech Stack

- React 19 + TypeScript
- Vite 6
- `@dnd-kit/core`, `@dnd-kit/sortable`, `@dnd-kit/utilities` — drag-and-drop
- `lucide-react` — icons
- `uuid` — id generation

## Getting Started

**Prerequisites:** Node.js

1. Install dependencies:
   ```bash
   npm install
   ```
2. (Optional, for AI-assisted breakdown) Set the `GEMINI_API_KEY` in a `.env.local` file:
   ```
   GEMINI_API_KEY=your_key_here
   ```
3. Start the dev server:
   ```bash
   npm run dev
   ```

Other scripts:

- `npm run build` — production build
- `npm run preview` — preview the production build

## Project Structure

```
App.tsx                          # Root component: board, stats, drag-and-drop, import/export
index.tsx / index.html           # Entry point
types.ts                         # Project, Feature, LifecycleState, ContextFile types
services/
  storage.ts                     # localStorage save/load of projects
components/
  CreateProjectModal.tsx         # New-project dialog
  FeatureDetailModal.tsx         # Feature edit dialog
  StatusBadge.tsx                # Lifecycle-state badge
import_template.json             # Template for JSON import
vite.config.ts                   # Vite config
```
