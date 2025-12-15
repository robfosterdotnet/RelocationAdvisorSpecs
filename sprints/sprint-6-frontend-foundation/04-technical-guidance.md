# Technical Guidance - Sprint 6

**From:** Sarah Chen (Tech Lead)
**Subject:** React Frontend Foundation Design Specifications

---

## Overview

This document provides design specifications for the React frontend. You'll need to research the libraries and write the code yourself.

---

## Project Setup

### Create React Project with Vite

Research Vite (https://vitejs.dev/) and create a new React + TypeScript project.

**Required packages to install:**
- `react-router-dom` - Client-side routing
- `axios` - HTTP client for API calls
- `tailwindcss` - Utility-first CSS framework
- `postcss` and `autoprefixer` - Required for Tailwind

Research: Vite documentation, Tailwind CSS installation guide

---

## Project Structure

Design your frontend with this organization:

```
frontend/src/
├── components/
│   ├── common/          # Reusable UI components
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   ├── Input.tsx
│   │   └── Loading.tsx
│   └── layout/          # Page layout components
│       ├── Header.tsx
│       └── Layout.tsx
├── pages/               # Page-level components
│   ├── Home.tsx
│   ├── Explore.tsx
│   ├── LocationDetail.tsx
│   ├── Rankings.tsx
│   └── Compare.tsx
├── services/            # API communication
│   └── api.ts
├── hooks/               # Custom React hooks
│   ├── useLocations.ts
│   └── useLocation.ts
├── types/               # TypeScript type definitions
│   └── index.ts
├── App.tsx              # Main app with routing
└── main.tsx             # Entry point
```

---

## TypeScript Types

### types/index.ts Requirements

Define TypeScript interfaces matching your API responses:

#### `Location`
```
id: number
fips_code: string
name: string
state_name: string
state_fips: string
latitude?: number
longitude?: number
demographics?: Demographics
employment?: Employment
climate?: Climate
```

#### `Demographics`
```
median_household_income?: number
median_home_value?: number
median_gross_rent?: number
population?: number
bachelors_degree_pct?: number
data_year?: number
```

#### `Employment`
```
unemployment_rate?: number
median_hourly_wage?: number
job_growth_rate?: number
labor_force?: number
data_year?: number
```

#### `Climate`
```
avg_temp_annual?: number
avg_temp_summer_high?: number
avg_temp_winter_low?: number
precipitation_annual?: number
snowfall_annual?: number
sunny_days?: number
```

#### `PaginatedResponse<T>`
Generic type for paginated API responses:
```
data: T[]
total: number
page: number
per_page: number
pages: number
```

Research: TypeScript generics, optional properties

---

## API Service Design

### services/api.ts Requirements

Create an axios instance with:
- Base URL from environment variable (`VITE_API_URL`)
- Default headers for JSON content
- Fallback to localhost:8000 for development

#### `locationService` Object

Create an object with these methods:

| Method | Parameters | Returns | API Endpoint |
|--------|------------|---------|--------------|
| getAll | page?, per_page?, state? | PaginatedResponse<Location> | GET /api/v1/locations |
| getById | id | Location | GET /api/v1/locations/{id} |
| getByFips | fips_code | Location | GET /api/v1/locations/fips/{fips} |
| search | query | PaginatedResponse<Location> | GET /api/v1/locations/search?q={query} |

Research: Axios configuration, async/await with axios

---

## Custom Hooks Design

### Why Custom Hooks?

Custom hooks encapsulate data fetching logic, making components cleaner.

### hooks/useLocations.ts Requirements

Create a hook that:
- Accepts parameters: `{ page?: number, state?: string }`
- Manages state for: data, loading, error
- Fetches data when parameters change
- Returns `{ data, loading, error }`

**State Management:**
| State | Type | Initial Value |
|-------|------|---------------|
| data | PaginatedResponse<Location> or null | null |
| loading | boolean | true |
| error | Error or null | null |

**Effect Logic:**
1. Set loading to true
2. Call API service
3. On success: set data
4. On error: set error
5. Finally: set loading to false

Research: `useState`, `useEffect`, dependency arrays

### hooks/useLocation.ts Requirements

Similar hook for fetching a single location by ID.

---

## Component Design

### components/common/ Requirements

Create reusable components:

#### `Button.tsx`
Props:
- `children` - Button text
- `onClick` - Click handler
- `variant` - "primary" | "secondary" | "danger"
- `disabled` - boolean
- `loading` - boolean (shows spinner)

#### `Card.tsx`
Props:
- `children` - Card content
- `onClick` - Optional click handler
- `className` - Additional CSS classes

#### `Loading.tsx`
A loading spinner component.
Research: CSS animations, spinner patterns

#### `Input.tsx`
Props:
- `value` - Current value
- `onChange` - Change handler
- `placeholder` - Placeholder text
- `type` - "text" | "number" | etc.

---

### components/layout/ Requirements

#### `Header.tsx`
Navigation header with:
- Logo/title
- Navigation links (Home, Explore, Rankings, Compare)
- Use React Router's `Link` component

#### `Layout.tsx`
Wrapper component that:
- Renders Header
- Renders `children` (page content)
- Applies consistent page padding/margins

---

## Page Components

### pages/Home.tsx Requirements

Landing page with:
- Hero section with app title and description
- Call-to-action buttons (Explore, Find Rankings)
- Brief feature highlights

### pages/Explore.tsx Requirements

Location browsing page with:
- State filter dropdown
- Grid of LocationCard components
- Pagination controls (Previous/Next buttons)
- Shows current page and total pages

**State to Manage:**
- `page` - Current page number
- `state` - Selected state filter

**Features:**
- Reset to page 1 when state filter changes
- Disable Previous on page 1
- Disable Next on last page

### pages/LocationDetail.tsx Requirements

Single location detail page with:
- Get location ID from URL parameters (`useParams`)
- Display all location data
- Show demographics, employment, climate in sections
- Handle loading and error states
- Back button to return to explore

Research: `useParams` from react-router-dom

### pages/Rankings.tsx (placeholder for Sprint 7)

Basic structure for now, full implementation next sprint.

### pages/Compare.tsx (placeholder for Sprint 7)

Basic structure for now, full implementation next sprint.

---

## Routing Setup

### App.tsx Requirements

Set up React Router with these routes:

| Path | Component | Description |
|------|-----------|-------------|
| `/` | Home | Landing page |
| `/explore` | Explore | Browse locations |
| `/location/:id` | LocationDetail | Single location |
| `/rankings` | Rankings | Find rankings |
| `/compare` | Compare | Compare locations |

**Structure:**
1. Wrap app in `BrowserRouter`
2. Use `Layout` component to wrap all routes
3. Define routes with `Routes` and `Route`

Research: react-router-dom v6 documentation

---

## Styling with Tailwind

### Common Patterns to Use

**Layout:**
- `container mx-auto` - Centered container
- `flex`, `grid` - Flexbox and grid layouts
- `gap-4` - Spacing between items

**Spacing:**
- `p-4`, `m-4` - Padding and margin
- `mb-4` - Margin bottom

**Typography:**
- `text-xl`, `text-2xl` - Font sizes
- `font-bold` - Bold text
- `text-gray-600` - Gray text color

**Components:**
- `border rounded-lg shadow` - Card styling
- `hover:shadow-md` - Hover effects
- `cursor-pointer` - Clickable indicator

Research: Tailwind CSS documentation, utility classes

---

## Environment Variables

### .env File

Create `.env` in frontend directory:
```
VITE_API_URL=http://localhost:8000
```

**Accessing in Code:**
```
import.meta.env.VITE_API_URL
```

Note: Vite requires `VITE_` prefix for client-side env vars.

---

## Testing the Frontend

### Manual Testing Checklist

1. Home page loads without errors
2. Navigation links work
3. Explore page fetches and displays locations
4. Pagination works correctly
5. State filter filters results
6. Location detail page shows full data
7. Loading states display properly
8. Error states display properly

### Browser DevTools

Use browser developer tools to:
- Check Network tab for API calls
- Check Console for errors
- Inspect component rendering

---

## Research Topics

1. **React Hooks** - useState, useEffect, useParams
2. **React Router v6** - BrowserRouter, Routes, Route, Link
3. **TypeScript with React** - Props typing, generics
4. **Axios** - HTTP client configuration
5. **Tailwind CSS** - Utility classes, responsive design
6. **Vite** - Build tool configuration

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Does the project structure follow the design?
2. Are TypeScript types properly defined?
3. Do custom hooks manage state correctly?
4. Does routing work for all pages?
5. Is the UI styled consistently?
6. Do API calls work correctly?
7. Are loading/error states handled?

---

*Next: Read `05-developer-tasks.md`*
