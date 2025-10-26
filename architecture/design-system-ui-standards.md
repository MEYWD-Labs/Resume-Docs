# Design System & UI Standards

## Overview

The design system establishes a cohesive visual language and component architecture across all Resume Builder SaaS interfaces, ensuring consistency, accessibility, and optimal performance across web, mobile, and admin platforms.

## Design Tokens Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                Design Token Hierarchy                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Foundation Layer                                            │
│  ────────────────                                           │
│  ├─ Colors (Primitive Values)                               │
│  ├─ Typography (Font Families & Scales)                     │
│  ├─ Spacing (Base Unit System)                              │
│  └─ Motion (Timing & Easing)                                │
│                                                              │
│  Semantic Layer                                             │
│  ──────────────                                            │
│  ├─ Brand Colors (Primary, Secondary, Accent)               │
│  ├─ System Colors (Success, Warning, Error, Info)           │
│  ├─ Text Styles (Headings, Body, Captions)                 │
│  └─ Interactive States (Hover, Active, Focus, Disabled)     │
│                                                              │
│  Component Layer                                            │
│  ───────────────                                           │
│  ├─ Button Variants                                         │
│  ├─ Form Controls                                           │
│  ├─ Card Styles                                             │
│  └─ Navigation Patterns                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Color System

**Brand Colors**:
```css
--color-primary-50: #EFF6FF;
--color-primary-100: #DBEAFE;
--color-primary-200: #BFDBFE;
--color-primary-300: #93C5FD;
--color-primary-400: #60A5FA;
--color-primary-500: #3B82F6;
--color-primary-600: #2563EB; /* Main Brand Blue */
--color-primary-700: #1D4ED8;
--color-primary-800: #1E40AF;
--color-primary-900: #1E3A8A;

--color-secondary-50: #F8FAFC;
--color-secondary-100: #F1F5F9;
--color-secondary-200: #E2E8F0;
--color-secondary-300: #CBD5E1;
--color-secondary-400: #94A3B8;
--color-secondary-500: #64748B; /* Slate Gray */
--color-secondary-600: #475569;
--color-secondary-700: #334155;
--color-secondary-800: #1E293B;
--color-secondary-900: #0F172A;
```

**System Colors**:
```css
--color-success: #10B981;
--color-warning: #F59E0B;
--color-error: #EF4444;
--color-info: #3B82F6;

--color-background-primary: #FFFFFF;
--color-background-secondary: #F8FAFC;
--color-background-tertiary: #F1F5F9;
```

**Dark Mode Support**:
```css
[data-theme="dark"] {
  --color-background-primary: #0F172A;
  --color-background-secondary: #1E293B;
  --color-background-tertiary: #334155;
  --color-text-primary: #F1F5F9;
  --color-text-secondary: #CBD5E1;
}
```

## Typography System

**Font Stack**:
```css
--font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI',
             Roboto, 'Helvetica Neue', Arial, sans-serif;
--font-mono: 'JetBrains Mono', 'Courier New', monospace;
```

**Type Scale**:
```css
--text-xs: 0.75rem;    /* 12px */
--text-sm: 0.875rem;   /* 14px */
--text-base: 1rem;     /* 16px */
--text-lg: 1.125rem;   /* 18px */
--text-xl: 1.25rem;    /* 20px */
--text-2xl: 1.5rem;    /* 24px */
--text-3xl: 1.875rem;  /* 30px */
--text-4xl: 2.25rem;   /* 36px */
--text-5xl: 3rem;      /* 48px */
```

**Line Heights**:
```css
--leading-none: 1;
--leading-tight: 1.25;
--leading-snug: 1.375;
--leading-normal: 1.5;
--leading-relaxed: 1.625;
--leading-loose: 2;
```

**Font Weights**:
```css
--font-light: 300;
--font-regular: 400;
--font-medium: 500;
--font-semibold: 600;
--font-bold: 700;
```

## Spacing System

**Base Unit**: 4px grid system

```css
--space-0: 0;
--space-1: 0.25rem;   /* 4px */
--space-2: 0.5rem;    /* 8px */
--space-3: 0.75rem;   /* 12px */
--space-4: 1rem;      /* 16px */
--space-5: 1.25rem;   /* 20px */
--space-6: 1.5rem;    /* 24px */
--space-8: 2rem;      /* 32px */
--space-10: 2.5rem;   /* 40px */
--space-12: 3rem;     /* 48px */
--space-16: 4rem;     /* 64px */
--space-20: 5rem;     /* 80px */
--space-24: 6rem;     /* 96px */
```

## Responsive Breakpoints

```css
--breakpoint-xs: 320px;
--breakpoint-sm: 640px;
--breakpoint-md: 768px;
--breakpoint-lg: 1024px;
--breakpoint-xl: 1280px;
--breakpoint-2xl: 1536px;
```

**Responsive Strategy**:
```
Mobile First Approach:
├─ Base styles: 320px - 640px
├─ Tablet enhancement: 640px - 1024px
├─ Desktop optimization: 1024px - 1280px
└─ Large screen polish: 1280px+
```

## Component Library Standards

### Component Architecture

```typescript
// Component Structure Example
interface ComponentProps {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  state?: 'default' | 'hover' | 'active' | 'focus' | 'disabled';
  theme?: 'light' | 'dark' | 'auto';
}
```

### Core Component Set

**Buttons**:
```
Primary Button   → Main CTAs, form submissions
Secondary Button → Secondary actions
Outline Button   → Tertiary actions
Ghost Button     → Minimal emphasis actions
Icon Button      → Compact actions with icons only
```

**Form Controls**:
```
Text Input       → Single-line text entry
Text Area        → Multi-line text entry
Select           → Dropdown selections
Checkbox         → Multiple selections
Radio            → Single selection from group
Switch           → Binary on/off states
Date Picker      → Date selection with calendar
File Upload      → Document and image uploads
```

**Navigation**:
```
Header           → Top navigation bar
Sidebar          → Side navigation panel
Tab Navigation   → Horizontal tab switching
Breadcrumbs      → Location pathway
Pagination       → Multi-page navigation
```

**Feedback**:
```
Alert            → System messages
Toast            → Temporary notifications
Modal            → Overlaying dialogs
Tooltip          → Contextual help
Progress Bar     → Loading and progress states
Skeleton        → Content loading placeholders
```

## Accessibility Requirements

**WCAG 2.1 AA Compliance**:
```
Color Contrast:
├─ Normal text: 4.5:1 minimum
├─ Large text: 3:1 minimum
├─ Interactive elements: 3:1 minimum
└─ Focus indicators: 3:1 minimum

Keyboard Navigation:
├─ All interactive elements keyboard accessible
├─ Visible focus indicators
├─ Logical tab order
├─ Skip links for navigation
└─ Keyboard shortcuts documentation

Screen Reader Support:
├─ Semantic HTML structure
├─ ARIA labels and descriptions
├─ Live regions for dynamic content
├─ Alternative text for images
└─ Proper heading hierarchy
```

## Performance Budgets

**Web Application**:
```
Initial Load:
├─ HTML: < 10 KB
├─ CSS: < 50 KB
├─ JavaScript: < 200 KB
├─ Fonts: < 100 KB
└─ Total: < 400 KB

Core Web Vitals:
├─ LCP (Largest Contentful Paint): < 2.5s
├─ FID (First Input Delay): < 100ms
├─ CLS (Cumulative Layout Shift): < 0.1
├─ FCP (First Contentful Paint): < 1.5s
└─ TTI (Time to Interactive): < 3.5s
```

**Mobile Application**:
```
App Size:
├─ iOS: < 50 MB
├─ Android: < 30 MB
└─ OTA Updates: < 5 MB

Launch Performance:
├─ Cold start: < 2s
├─ Warm start: < 1s
├─ Hot start: < 500ms
└─ First meaningful paint: < 1.5s
```

## Motion & Animation

**Animation Principles**:
```css
/* Timing Functions */
--ease-in: cubic-bezier(0.4, 0, 1, 1);
--ease-out: cubic-bezier(0, 0, 0.2, 1);
--ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
--ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55);

/* Duration Scale */
--duration-75: 75ms;
--duration-100: 100ms;
--duration-150: 150ms;
--duration-200: 200ms;
--duration-300: 300ms;
--duration-500: 500ms;
--duration-700: 700ms;
--duration-1000: 1000ms;
```

**Animation Guidelines**:
- Micro-interactions: 100-200ms
- Page transitions: 200-300ms
- Complex animations: 300-500ms
- Respect prefers-reduced-motion settings

## Theme Architecture

```typescript
interface ThemeConfig {
  mode: 'light' | 'dark' | 'system';
  colors: ColorPalette;
  typography: TypographyScale;
  spacing: SpacingScale;
  breakpoints: Breakpoints;
  components: ComponentTokens;
}

// Theme Provider Implementation
const ThemeContext = {
  theme: ThemeConfig,
  toggleTheme: () => void,
  setTheme: (theme: ThemeMode) => void,
  customizeTheme: (overrides: Partial<ThemeConfig>) => void
};
```

## Cross-Platform Design Consistency

```
Platform-Specific Adaptations:
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│  Web (Next.js)                                              │
│  ├─ Tailwind CSS with custom config                         │
│  ├─ CSS-in-JS for dynamic styles                           │
│  └─ Server-side rendering optimizations                     │
│                                                              │
│  Mobile (React Native)                                      │
│  ├─ StyleSheet API with theme provider                      │
│  ├─ Platform-specific components (iOS/Android)              │
│  └─ Native gesture handlers                                 │
│                                                              │
│  Shared Design Tokens                                       │
│  ├─ JSON token definitions                                  │
│  ├─ Build-time token transformation                         │
│  └─ Runtime theme switching                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Icon System

**Icon Library Requirements**:
- Format: SVG with consistent viewBox (24x24)
- Variants: Outline (default), Solid, Duotone
- Sizes: 16px, 20px, 24px (default), 32px, 48px
- Accessibility: Decorative vs. Informative icons

```typescript
interface IconProps {
  name: string;
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  variant?: 'outline' | 'solid' | 'duotone';
  color?: string;
  className?: string;
  ariaLabel?: string; // For informative icons
  decorative?: boolean; // For decorative icons
}
```

## Data Visualization Standards

**Chart Types**:
- Line charts: Trends over time
- Bar charts: Comparisons
- Pie/Donut charts: Part-to-whole relationships
- Area charts: Cumulative values
- Scatter plots: Correlations

**Chart Color Palette**:
```css
--chart-1: #2563EB;
--chart-2: #10B981;
--chart-3: #F59E0B;
--chart-4: #8B5CF6;
--chart-5: #EC4899;
--chart-6: #14B8A6;
--chart-7: #F97316;
--chart-8: #6366F1;
```

## Internationalization (i18n) Support

**Text Direction**:
- LTR (Left-to-Right): Default
- RTL (Right-to-Left): Arabic, Hebrew support

**Locale-Specific Formatting**:
- Date formats: ISO 8601 with locale display
- Number formats: Decimal and thousand separators
- Currency: Multiple currency support
- Time zones: User timezone detection

## Design System Governance

**Version Control**:
```
Design Token Updates:
├─ Major: Breaking changes (yearly)
├─ Minor: New tokens/components (quarterly)
└─ Patch: Bug fixes (as needed)

Component Library Releases:
├─ Stable: Production-ready components
├─ Beta: Testing new patterns
└─ Experimental: Proof of concepts
```

**Documentation Requirements**:
- Component usage guidelines
- Accessibility notes
- Performance considerations
- Platform-specific variations
- Code examples and demos
