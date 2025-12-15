# Requirements Document - Sprint 6

---

## Frontend Requirements

### FR-6.1: Project Setup
- React 18+ with TypeScript
- Vite or Create React App
- React Router v6
- Tailwind CSS or CSS Modules

### FR-6.2: Pages
- `/` - Home page
- `/explore` - Browse locations
- `/location/:id` - Location detail
- `/rankings` - Rankings page (placeholder)
- `/compare` - Compare page (placeholder)

### FR-6.3: Components
- Header with navigation
- Search input
- Location card
- Pagination
- Loading spinner
- Error message

### FR-6.4: API Integration
- API client service
- Type definitions for responses
- Error handling
- Loading states

---

## Component Specifications

### LocationCard
```typescript
interface LocationCardProps {
  id: number;
  name: string;
  state: string;
  population?: number;
  medianIncome?: number;
  onClick: () => void;
}
```

### SearchBar
```typescript
interface SearchBarProps {
  onSearch: (query: string) => void;
  placeholder?: string;
}
```

---

## API Types

```typescript
interface Location {
  id: number;
  fips_code: string;
  name: string;
  state_name: string;
  latitude?: number;
  longitude?: number;
  demographics?: Demographics;
  employment?: Employment;
  climate?: Climate;
}

interface PaginatedResponse<T> {
  data: T[];
  total: number;
  page: number;
  per_page: number;
  pages: number;
}
```

---

*Next: Read `03-user-stories.md`*
