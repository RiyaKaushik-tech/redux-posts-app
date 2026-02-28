# Redux Posts App (CRUD + Favorites)
A polished, offline-first posts manager built with React, Redux Toolkit, and a modern Tailwind UI stack.

## Overview
**Redux Posts App** is a single-page CRUD application for creating, editing, deleting, and favoriting posts. Posts are stored in a Redux state slice and persisted to the browser via `redux-persist`, enabling a smooth offline-friendly experience without a backend.

The UI is built for a modern feel (animated gradients, responsive layout) and uses a lightweight component approach with **shadcn/ui-style** primitives (Radix UI + utility helpers) alongside Tailwind CSS.

---

## Links
- **Repository:** https://github.com/RiyaKaushik-tech/redux-posts-app
- **Live Demo:** [_Not found in repository_ (add link here when deployed)](https://redux-posts-app-lake.vercel.app/)

---

## Key Features
- **Create / Edit / Delete posts** with title, description, and optional image
- **Favorites workflow**: toggle favorite status and view favorites on a dedicated page
- **Client-side image uploads** using `FileReader` (stores as Base64 Data URL in state)
- **Persisted state** via `redux-persist` (posts survive reloads)
- **Route-based navigation** using React Router:
  - Home (`/`)
  - About (`/about`)
  - Create (`/create`)
  - Edit (`/edit/:id`)
  - Favorites (`/fav`)
- **Delete confirmation dialog** (Radix Alert Dialog via shadcn-style wrapper)
- **Motion-enhanced UI** with Framer Motion animations on post cards
- **Responsive navbar** with a mobile menu toggle


---

## Technical Architecture Overview
This project follows a straightforward SPA architecture:

- **UI Layer (React components/pages)** renders views and captures user input.
- **Routing Layer (React Router)** maps routes to page components.
- **State Layer (Redux Toolkit)** owns the posts domain state and actions.
- **Persistence Layer (redux-persist)** rehydrates the Redux store from `localStorage`.
- **UI Primitives (shadcn-style)** provide composable components built on Radix + Tailwind utilities.
- **Build Tooling (Vite)** provides fast local development and optimized production builds.

### Data Model (Post)
Posts are stored in Redux as objects shaped approximately like:

| Field | Type | Notes |
|------|------|------|
| `id` | number | Created via `Date.now()` |
| `title` | string | Required |
| `description` | string | Required |
| `date` | string | `new Date().toLocaleDateString()` |
| `image` | string | Base64 Data URL or fallback URL |
| `fav` | boolean | Managed by `toggleFav` reducer |

---

## Tech Stack

### Frontend
- **React 19**
- **React Router DOM 7** (client-side routing)
- **Framer Motion** (animations)

### Backend
- **None** (client-only app)

### State Management
- **Redux Toolkit**
- **React Redux**
- **redux-persist** (localStorage persistence)

### APIs
- **None currently** (no active external API integration detected)

### Authentication
- **None**

### Styling
- **Tailwind CSS (via `@tailwindcss/vite`)**
- **tw-animate-css** (animation utilities)
- **shadcn/ui-style components** (Radix UI primitives + Tailwind patterns)
- Some Bootstrap CSS is included via CDN in `index.html`, though Tailwind appears to be the primary styling system.

### Tooling
- **Vite 7**
- **ESLint 9** (React Hooks + React Refresh rules)
- **JSConfig path aliases** (`@/*` → `src/*`)

---

## Folder Structure
> Generated from repository inspection. Some listings may be incomplete due to search result limits in code browsing.

```text
redux-posts-app/
├── index.html
├── vite.config.js
├── package.json
├── eslint.config.js
├── jsconfig.json
├── components.json
├── public/
│   └── vite.svg
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── App.css
    ├── index.css
    ├── lib/
    │   └── utils.js
    ├── reduxConcepts/
    │   ├── store.js
    │   └── slice.js
    ├── pages/
    │   ├── home.jsx
    │   ├── create.jsx
    │   ├── edit.jsx
    │   ├── fav.jsx
    │   └── about.jsx
    ├── component/
    │   ├── Navbar.jsx
    │   ├── Footer.jsx
    │   ├── postCard.jsx
    │   ├── confirmation.jsx
    │   └── loading.jsx
    └── components/
        └── ui/
            ├── button.jsx
            ├── input.jsx
            ├── textarea.jsx
            ├── card.jsx
            ├── alert.jsx
            ├── alert-dialog.jsx
            └── dropdown-menu.jsx
```

To browse more files via GitHub search UI:
- https://github.com/RiyaKaushik-tech/redux-posts-app/search?q=path%3Asrc&type=code

---

## Installation & Setup

### Prerequisites
- Node.js (recommended: latest LTS)

### Install
```bash
git clone https://github.com/RiyaKaushik-tech/redux-posts-app.git
cd redux-posts-app
npm install
```

### Run locally
```bash
npm run dev
```

### Production build
```bash
npm run build
npm run preview
```

---

## Environment Variables
No `.env` usage or `import.meta.env` / `process.env` references were detected in the codebase.  
If you add runtime configuration later, consider using Vite-style variables (e.g., `VITE_API_BASE_URL`).

---

## Usage Guide

### Create a post
1. Go to **Create** (`/create`)
2. Enter **Title** and **Description**
3. Optionally upload an image (stored in-app as a Base64 Data URL)
4. Submit to add the post and return to Home

### Edit a post
- From Home, click **Edit** on a post card → updates title/description/image while preserving the `id` and `fav` flag.

### Delete a post
- Click **Delete** → confirm in the modal dialog → post is removed from Redux state (and persisted storage).

### Favorite a post
- Click **Fav** on a post card
- View favorites on **Favorites page** (`/fav`)

---

## Engineering Highlights

### Redux slice design (single domain reducer)
The `slice.js` reducer cleanly encapsulates core domain actions:
- `add`: appends a post, defaulting `fav` to `false`
- `remove`: filters by `id`
- `edit`: updates mutable fields while intentionally preserving `id` and `fav`
- `toggleFav`: flips favorite status by `id`

### State persistence with safe middleware configuration
`redux-persist` integrates through a persisted reducer and `PersistGate`.  
The store middleware config disables serializable checks for known persistence actions to avoid false positives:

- `FLUSH`, `REHYDRATE`, `PAUSE`, `PERSIST`, `PURGE`, `REGISTER`

### UX patterns implemented
- **Route-level loading skeleton** pattern (a short 500ms loader is shown on multiple pages/components)
- **Modal preview** for long descriptions and images inside `PostCard`
- **Confirmation dialog** for destructive actions (delete) using Radix Alert Dialog

---

## Performance / Optimization Notes
- **Persistence** minimizes unnecessary re-entry and improves perceived performance by keeping state across reloads.
- **Vite** provides fast dev-server startup and efficient production builds.
- Images are stored as Base64 in state; this improves portability (no server dependency) but can increase `localStorage` usage and slow down persistence for large images.

---

## Security Considerations
- No authentication or backend calls are present.
- External links opened via the post popup use:
  - `target="_blank"` and `rel="noopener noreferrer"` (good tabnabbing protection)
- If you later add API calls or auth, avoid storing tokens in `localStorage` and validate/sanitize user-provided URLs.

---

## Future Improvements
- Add automated tests (unit tests for reducers, component tests for pages)
- Replace `alert()` validations with inline form errors and accessible feedback
- Add image size/type validation and optional compression before persisting
- Introduce a real backend (optional) with sync, pagination, and search
- Reduce repeated loading logic by extracting a reusable page-level `Suspense/Loader` wrapper
- Remove unused dependencies (`next`, `axios`, etc.) or implement the intended integrations

---

## Author
**Riya Kaushik**

- Email: `riyakaushik6410@email.com`
- GitHub: https://github.com/riya1807pro
- LinkedIn: https://www.linkedin.com/in/riyakaushik-webdev
