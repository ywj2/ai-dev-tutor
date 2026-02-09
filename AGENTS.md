# AI Development Agent Guidelines

This file contains build/lint/test commands and code style guidelines for agentic coding agents working in this repository.

## Project Overview

This is a Next.js 16 application with TypeScript, Tailwind CSS v4, and ESLint configuration. The project follows modern React patterns with App Router.

## Build, Lint, and Test Commands

### Development
```bash
# Start development server
npm run dev

# Build for production
npm run build

# Start production server
npm run start

# Run linter
npm run lint
```

### Testing
This project currently does not have a testing framework configured. When adding tests:
- Use Jest or Vitest for unit testing
- Use React Testing Library for component testing
- Add test scripts to package.json as needed

### Code Style Guidelines

#### TypeScript Configuration
- Target: ES2017
- Strict mode enabled
- Module resolution: bundler
- JSX: react-jsx
- Path alias: @/* maps to ./*

#### Import Organization
```typescript
// 1. React/Next.js imports
import { useState, useEffect } from 'react';
import Image from 'next/image';

// 2. Third-party libraries
import { Button } from '@/components/ui/button';

// 3. Local imports (use @ alias)
import { getPosts } from '@/lib/api';
import { Layout } from '@/components/layout';
```

#### Component Structure
```typescript
// 1. Imports
import React from 'react';

// 2. Types/Interfaces (if needed)
interface ComponentProps {
  title: string;
  description?: string;
}

// 3. Component definition
export default function ComponentName({
  title,
  description = 'Default description'
}: ComponentProps) {
  // 4. Hooks (useState, useEffect, etc.)
  const [state, setState] = useState(null);
  
  // 5. Event handlers
  const handleClick = () => {
    setState(!state);
  };
  
  // 6. Render
  return (
    <div className="container">
      <h1>{title}</h1>
      {description && <p>{description}</p>}
    </div>
  );
}

// 7. Prop types (if not using TypeScript)
ComponentName.propTypes = {
  title: PropTypes.string.isRequired,
  description: PropTypes.string
};
```

#### Naming Conventions
- **Components**: PascalCase (e.g., `UserProfile`, `DataTable`)
- **Functions/Variables**: camelCase (e.g., `getUserData`, `isLoading`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `API_BASE_URL`, `MAX_RETRY_ATTEMPTS`)
- **Files**: PascalCase for components (e.g., `UserProfile.tsx`), camelCase for utilities (e.g., `apiClient.ts`)
- **CSS Classes**: Tailwind utility classes or BEM methodology for custom styles

#### Error Handling
```typescript
// Use try-catch for async operations
try {
  const data = await fetchUserData();
  setData(data);
} catch (error) {
  console.error('Failed to fetch user data:', error);
  setError('Unable to load user data');
}

// Use proper error boundaries for React components
```

#### State Management
- Use React hooks (`useState`, `useEffect`, `useContext`) for local state
- Consider Context API for global state
- Avoid direct mutations; use immutable updates
- Prefer derived state over redundant state

#### Styling Guidelines
- Use Tailwind CSS utility classes
- Leverage dark mode support (`dark:` prefix)
- Use CSS variables for theming
- Follow responsive design patterns (`sm:`, `md:`, `lg:` prefixes)

#### File Organization
```
src/
├── components/     # Reusable UI components
│   ├── ui/         # Basic UI elements
│   └── features/   # Feature-specific components
├── lib/           # Utilities and helper functions
├── types/         # TypeScript type definitions
├── hooks/         # Custom React hooks
├── pages/         # Page components (Next.js App Router)
└── styles/        # Global styles and themes
```

#### ESLint Configuration
- Uses Next.js ESLint configuration
- Extends core web vitals and TypeScript rules
- Ignores `.next/`, `out/`, `build/`, and `next-env.d.ts`

#### TypeScript Best Practices
- Use explicit return types for functions
- Prefer interfaces over types for object shapes
- Use generic types where appropriate
- Enable strict mode for type safety

#### Performance Guidelines
- Use React.memo for expensive components
- Implement proper loading states
- Optimize images with Next.js Image component
- Use dynamic imports for code splitting

#### Security Considerations
- Use `rel="noopener noreferrer"` for external links
- Sanitize user inputs
- Use environment variables for sensitive data
- Implement proper authentication and authorization

## Development Workflow

1. Always run `npm run lint` before committing
2. Use TypeScript for type safety
3. Follow the established component patterns
4. Test responsive design across breakpoints
5. Ensure dark mode compatibility
6. Use semantic HTML elements
7. Optimize for Core Web Vitals

## Common Patterns

### API Calls
```typescript
const fetchUsers = async (): Promise<User[]> => {
  try {
    const response = await fetch('/api/users');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch users:', error);
    throw error;
  }
};
```

### Form Handling
```typescript
const [formData, setFormData] = useState({
  name: '',
  email: ''
});

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  const { name, value } = e.target;
  setFormData(prev => ({
    ...prev,
    [name]: value
  }));
};
```

### Loading States
```typescript
const [isLoading, setIsLoading] = useState(false);
const [data, setData] = useState<Data | null>(null);

const loadData = async () => {
  setIsLoading(true);
  try {
    const result = await fetchData();
    setData(result);
  } finally {
    setIsLoading(false);
  }
};
```

## Tools and Extensions

- Use VS Code with TypeScript and ESLint extensions
- Install Tailwind CSS IntelliSense for better autocomplete
- Use Prettier for code formatting (if configured)
- Consider using Next.js DevTools for debugging