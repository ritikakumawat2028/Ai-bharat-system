# Design Document - AI for Bharat Platform

## 1. System Architecture

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Client Layer                          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  React SPA (Vite + TypeScript + Tailwind CSS)       │  │
│  │  - Component-based UI                                 │  │
│  │  - Client-side routing (React Router)                │  │
│  │  - Context API for state management                  │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    API Layer (Future)                        │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  RESTful API / GraphQL                               │  │
│  │  - Authentication endpoints                           │  │
│  │  - User management                                    │  │
│  │  - AI chat integration                                │  │
│  │  - Scheme data API                                    │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   Service Layer (Future)                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │   Auth   │  │   User   │  │    AI    │  │  Scheme  │  │
│  │ Service  │  │ Service  │  │ Service  │  │ Service  │  │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   Data Layer (Future)                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                 │
│  │ Database │  │  Cache   │  │  Storage │                 │
│  │(MongoDB/ │  │ (Redis)  │  │  (S3)    │                 │
│  │PostgreSQL)│  │          │  │          │                 │
│  └──────────┘  └──────────┘  └──────────┘                 │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              External Services (Future)                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                 │
│  │  Gemini  │  │  Speech  │  │   Gov    │                 │
│  │   API    │  │   API    │  │  Scheme  │                 │
│  │          │  │          │  │   APIs   │                 │
│  └──────────┘  └──────────┘  └──────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 Current Implementation (Frontend-Only)

```
┌─────────────────────────────────────────────────────────────┐
│                    React Application                         │
│                                                               │
│  ┌────────────────────────────────────────────────────┐    │
│  │              App Component (Root)                   │    │
│  │  - RouterProvider                                   │    │
│  │  - Toaster (notifications)                          │    │
│  └────────────────────────────────────────────────────┘    │
│                          │                                   │
│  ┌────────────────────────────────────────────────────┐    │
│  │           Context Providers                         │    │
│  │  - AuthContext (user authentication)                │    │
│  │  - LanguageContext (i18n)                           │    │
│  │  - UserDataContext (user preferences)               │    │
│  └────────────────────────────────────────────────────┘    │
│                          │                                   │
│  ┌────────────────────────────────────────────────────┐    │
│  │              Routing Layer                          │    │
│  │  - Root (layout wrapper)                            │    │
│  │  - Public routes (Landing, Login, Signup)           │    │
│  │  - Protected routes (Dashboard, Chat, etc.)         │    │
│  └────────────────────────────────────────────────────┘    │
│                          │                                   │
│  ┌────────────────────────────────────────────────────┐    │
│  │              Page Components                        │    │
│  │  - LandingPage                                      │    │
│  │  - Dashboard                                        │    │
│  │  - AIChat                                           │    │
│  │  - StudentSupport                                   │    │
│  │  - GovernmentSchemes                                │    │
│  │  - CareerGuidance                                   │    │
│  │  - ProfilePage                                      │    │
│  └────────────────────────────────────────────────────┘    │
│                          │                                   │
│  ┌────────────────────────────────────────────────────┐    │
│  │          Shared Components                          │    │
│  │  - Header (navigation)                              │    │
│  │  - MobileMenu                                       │    │
│  │  - Modals (SchemeDetail, EligibilityResult)         │    │
│  │  - UI Components (shadcn/ui)                        │    │
│  └────────────────────────────────────────────────────┘    │
│                          │                                   │
│  ┌────────────────────────────────────────────────────┐    │
│  │          Data & Utilities                           │    │
│  │  - governmentSchemes.ts (static data)               │    │
│  │  - aiResponses.ts (mock AI logic)                   │    │
│  │  - utils (helper functions)                         │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Design Patterns & Principles

### 2.1 Architectural Patterns

#### Component-Based Architecture
- Modular, reusable components
- Single Responsibility Principle
- Composition over inheritance
- Props-based data flow

#### Context Pattern
- Global state management without prop drilling
- Separate contexts for different concerns (Auth, Language, UserData)
- Provider pattern for dependency injection

#### Container/Presentational Pattern
- Page components as containers (business logic)
- UI components as presentational (pure rendering)
- Clear separation of concerns

### 2.2 Design Principles

#### DRY (Don't Repeat Yourself)
- Reusable UI components (shadcn/ui)
- Shared utilities and hooks
- Centralized styling with Tailwind

#### SOLID Principles
- Single Responsibility: Each component has one purpose
- Open/Closed: Components extensible via props
- Liskov Substitution: Component interfaces consistent
- Interface Segregation: Minimal prop requirements
- Dependency Inversion: Depend on abstractions (contexts)

#### Responsive Design
- Mobile-first approach
- Breakpoint-based layouts (sm, md, lg, xl)
- Flexible grid systems
- Touch-friendly interactions

---

## 3. Component Design

### 3.1 Component Hierarchy

```
App
├── RouterProvider
│   └── Root (Layout)
│       ├── Header
│       │   ├── Logo
│       │   ├── Navigation
│       │   └── MobileMenu
│       │
│       └── Page Components
│           ├── LandingPage
│           │   ├── Hero Section
│           │   ├── Features Grid
│           │   └── CTA Section
│           │
│           ├── LanguageSelection
│           │   └── Language Cards
│           │
│           ├── LoginPage / SignupPage
│           │   └── Auth Form
│           │
│           ├── Dashboard
│           │   ├── Welcome Section
│           │   ├── Stats Grid
│           │   ├── Services Grid
│           │   ├── Recent Activity
│           │   └── Learning Progress
│           │
│           ├── AIChat
│           │   ├── Chat Header
│           │   ├── Messages Container
│           │   │   └── Message Bubbles
│           │   ├── Input Area
│           │   └── Quick Suggestions
│           │
│           ├── StudentSupport
│           │   ├── Feature Cards
│           │   └── Progress Tracking
│           │
│           ├── GovernmentSchemes
│           │   ├── Search & Filter
│           │   ├── Scheme Cards
│           │   ├── SchemeDetailModal
│           │   └── EligibilityResultModal
│           │
│           ├── CareerGuidance
│           │   └── Career Resources
│           │
│           └── ProfilePage
│               └── User Information
│
└── Toaster (Global Notifications)
```

### 3.2 Key Component Specifications

#### Header Component
```typescript
Props: None (uses contexts)
State: mobileMenuOpen
Features:
- Logo and branding
- Navigation links (conditional based on auth)
- Language selector
- User profile dropdown
- Mobile hamburger menu
- Responsive design
```

#### Dashboard Component
```typescript
Props: None (uses contexts)
Features:
- Personalized welcome
- Statistics cards (study hours, schemes, skills)
- Quick access service cards
- Recent activity feed
- Learning progress bars
- Daily motivation card
- Responsive grid layout
```

#### AIChat Component
```typescript
State:
- messages: Message[]
- inputMessage: string
- isLoading: boolean
- isListening: boolean

Features:
- Real-time chat interface
- Message history with timestamps
- User/AI message differentiation
- Voice input button (future)
- Quick suggestion chips
- Auto-scroll to latest message
- Loading indicators
```

#### GovernmentSchemes Component
```typescript
State:
- schemes: GovernmentScheme[]
- selectedCategory: string
- searchQuery: string
- selectedScheme: GovernmentScheme | null

Features:
- Category filter tabs
- Search functionality
- Scheme cards with eligibility badges
- Detail modal with full information
- Eligibility checker
- Application process steps
- Official link redirects
```

---

## 4. Data Models

### 4.1 User Model
```typescript
interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
  language: Language;
  createdAt: Date;
  lastLogin: Date;
}
```

### 4.2 Government Scheme Model
```typescript
interface GovernmentScheme {
  id: number;
  name: string;
  category: 'agriculture' | 'health' | 'education' | 'business' | 
            'housing' | 'employment' | 'social_security';
  description: string;
  fullDescription: string;
  eligibility: string[];
  benefits: string;
  documents: string[];
  howToApply: string[];
  officialLink: string;
  ministry: string;
  eligible: boolean;
  isActive: boolean;
}
```

### 4.3 Message Model
```typescript
interface Message {
  id: string;
  text: string;
  sender: 'user' | 'ai';
  timestamp: Date;
}
```

### 4.4 Language Model
```typescript
type Language = 'en' | 'hi' | 'ta' | 'te' | 'bn' | 'mr' | 'gu' | 'kn';

interface Translation {
  [key: string]: string;
}

interface Translations {
  [language in Language]: Translation;
}
```

---

## 5. UI/UX Design

### 5.1 Design System

#### Color Palette
```css
Primary Colors:
- Orange: #f97316 (orange-500) to #ea580c (orange-600)
- Green: #16a34a (green-600) to #15803d (green-700)

Gradients:
- Primary: from-orange-500 to-green-600
- Background: from-orange-50 via-white to-green-50

Neutral Colors:
- Gray scale: gray-50 to gray-900
- White: #ffffff
- Black: #000000

Semantic Colors:
- Success: green-600
- Error: red-600
- Warning: yellow-600
- Info: blue-600
```

#### Typography
```css
Font Family: System font stack (default)

Heading Sizes:
- h1: text-3xl (30px) - Page titles
- h2: text-2xl (24px) - Section titles
- h3: text-xl (20px) - Card titles
- h4: text-lg (18px) - Subsections

Body Text:
- Base: text-base (16px)
- Small: text-sm (14px)
- Extra small: text-xs (12px)

Font Weights:
- Normal: font-normal (400)
- Medium: font-medium (500)
- Semibold: font-semibold (600)
- Bold: font-bold (700)
```

#### Spacing System
```css
Tailwind spacing scale (4px base):
- xs: 0.5 (2px)
- sm: 1 (4px)
- md: 2 (8px)
- lg: 4 (16px)
- xl: 6 (24px)
- 2xl: 8 (32px)
- 3xl: 12 (48px)
```

#### Border Radius
```css
- sm: 0.125rem (2px)
- DEFAULT: 0.25rem (4px)
- md: 0.375rem (6px)
- lg: 0.5rem (8px)
- xl: 0.75rem (12px)
- 2xl: 1rem (16px)
- full: 9999px (circular)
```

### 5.2 Component Styling Patterns

#### Card Component
```css
- Border: 2px solid
- Shadow: shadow-xl on hover
- Padding: p-6
- Border radius: rounded-lg
- Background: white
- Hover effect: -translate-y-1 transition
```

#### Button Component
```css
Primary:
- Background: gradient from-orange-500 to-green-600
- Text: white
- Padding: px-4 py-2
- Border radius: rounded-md
- Hover: from-orange-600 to-green-700

Secondary:
- Border: 2px solid
- Background: transparent
- Hover: background-gray-100
```

#### Input Component
```css
- Border: 2px solid gray-300
- Focus: border-orange-500 ring-2 ring-orange-200
- Padding: px-3 py-2
- Border radius: rounded-md
- Font size: text-base
```

### 5.3 Layout Patterns

#### Grid Layouts
```css
Dashboard Stats:
- grid-cols-1 md:grid-cols-3
- gap-6

Service Cards:
- grid-cols-1 md:grid-cols-2 lg:grid-cols-4
- gap-6

Scheme Cards:
- grid-cols-1 md:grid-cols-2 lg:grid-cols-3
- gap-6
```

#### Responsive Breakpoints
```css
- sm: 640px (mobile landscape)
- md: 768px (tablet)
- lg: 1024px (desktop)
- xl: 1280px (large desktop)
- 2xl: 1536px (extra large)
```

### 5.4 Animation & Transitions

#### Hover Effects
```css
- Scale: hover:scale-110
- Translate: hover:-translate-y-1
- Shadow: hover:shadow-xl
- Color: hover:from-orange-600
- Duration: transition-all
```

#### Loading States
```css
- Spinner: animate-spin
- Pulse: animate-pulse
- Skeleton: bg-gray-200 animate-pulse
```

---

## 6. State Management

### 6.1 Context Architecture

#### AuthContext
```typescript
State:
- user: User | null
- isAuthenticated: boolean

Methods:
- login(email, password): Promise<void>
- signup(name, email, password): Promise<void>
- logout(): void

Usage: Authentication status, user info
```

#### LanguageContext
```typescript
State:
- language: Language
- translations: Translations

Methods:
- setLanguage(lang: Language): void
- t(key: string): string

Usage: UI translations, language switching
```

#### UserDataContext (Future)
```typescript
State:
- preferences: UserPreferences
- progress: ProgressData
- history: ActivityHistory

Methods:
- updatePreferences(prefs): void
- trackProgress(data): void
- getHistory(): ActivityHistory[]

Usage: User preferences, progress tracking
```

### 6.2 Local State Management

#### Component State
- useState for local UI state
- useRef for DOM references
- useEffect for side effects

#### Form State
- Controlled components
- Validation logic
- Error handling

---

## 7. Routing Design

### 7.1 Route Structure

```typescript
Routes:
/ (index)           → LandingPage (public)
/language           → LanguageSelection (public)
/login              → LoginPage (public)
/signup             → SignupPage (public)
/dashboard          → Dashboard (protected)
/chat               → AIChat (protected)
/student-support    → StudentSupport (protected)
/government-schemes → GovernmentSchemes (protected)
/career-guidance    → CareerGuidance (protected)
/profile            → ProfilePage (protected)
```

### 7.2 Navigation Flow

```
Landing Page
    ├─→ Language Selection
    │       └─→ Login / Signup
    │               └─→ Dashboard
    │                       ├─→ Student Support
    │                       ├─→ Government Schemes
    │                       ├─→ Career Guidance
    │                       ├─→ AI Chat
    │                       └─→ Profile
    │
    └─→ Direct Login (if language already set)
```

### 7.3 Route Protection (Future)

```typescript
Protected Route Logic:
- Check authentication status
- Redirect to /login if not authenticated
- Preserve intended destination
- Redirect after successful login
```

---

## 8. Performance Optimization

### 8.1 Code Splitting
- Route-based code splitting
- Lazy loading of components
- Dynamic imports for heavy modules

### 8.2 Asset Optimization
- Image lazy loading
- WebP format with fallbacks
- SVG for icons
- Font optimization

### 8.3 Rendering Optimization
- React.memo for expensive components
- useMemo for computed values
- useCallback for function props
- Virtual scrolling for long lists (future)

### 8.4 Bundle Optimization
- Tree shaking
- Minification
- Compression (gzip/brotli)
- CDN delivery

---

## 9. Accessibility Design

### 9.1 Semantic HTML
- Proper heading hierarchy (h1 → h6)
- Semantic tags (nav, main, article, section)
- ARIA labels where needed
- Alt text for images

### 9.2 Keyboard Navigation
- Tab order optimization
- Focus indicators
- Keyboard shortcuts
- Skip to content link

### 9.3 Screen Reader Support
- ARIA roles and attributes
- Live regions for dynamic content
- Descriptive labels
- Form field associations

### 9.4 Visual Accessibility
- Color contrast ratios (WCAG AA)
- Focus indicators
- Text resizing support
- No color-only information

---

## 10. Error Handling & Validation

### 10.1 Form Validation
```typescript
Validation Rules:
- Email: Valid email format
- Password: Minimum 8 characters
- Name: Required, 2-50 characters
- Real-time validation feedback
- Clear error messages
```

### 10.2 Error States
```typescript
Error Types:
- Network errors
- Validation errors
- Authentication errors
- Not found errors

Error Display:
- Toast notifications (sonner)
- Inline form errors
- Error boundaries for crashes
- Fallback UI
```

### 10.3 Loading States
```typescript
Loading Indicators:
- Skeleton screens
- Spinner animations
- Progress bars
- Disabled states during operations
```

---

## 11. Internationalization (i18n)

### 11.1 Translation Architecture

```typescript
Structure:
translations/
  ├── en.ts (English)
  ├── hi.ts (Hindi)
  ├── ta.ts (Tamil)
  ├── te.ts (Telugu)
  ├── bn.ts (Bengali)
  ├── mr.ts (Marathi)
  ├── gu.ts (Gujarati)
  └── kn.ts (Kannada)

Usage:
const { t } = useLanguage();
<h1>{t('welcome')}</h1>
```

### 11.2 RTL Support (Future)
- Right-to-left layout for applicable languages
- Mirrored UI elements
- Text alignment adjustments

---

## 12. Testing Strategy (Future)

### 12.1 Unit Testing
- Component testing (React Testing Library)
- Utility function testing (Jest)
- Context testing
- Hook testing

### 12.2 Integration Testing
- User flow testing
- API integration testing
- Form submission testing
- Navigation testing

### 12.3 E2E Testing
- Critical user journeys
- Cross-browser testing
- Mobile responsiveness testing
- Accessibility testing

---

## 13. Deployment Architecture (Future)

### 13.1 Frontend Deployment
```
Build Process:
1. npm run build (Vite)
2. Generate optimized bundle
3. Deploy to CDN/hosting
4. Configure routing (SPA)

Hosting Options:
- Vercel
- Netlify
- AWS S3 + CloudFront
- Firebase Hosting
```

### 13.2 CI/CD Pipeline
```
Pipeline Stages:
1. Code commit (Git)
2. Automated tests
3. Build process
4. Deploy to staging
5. Manual approval
6. Deploy to production
7. Post-deployment tests
```

---

## 14. Security Design

### 14.1 Frontend Security
- XSS prevention (React auto-escaping)
- CSRF tokens (future API)
- Secure storage (httpOnly cookies)
- Input sanitization
- Content Security Policy

### 14.2 Authentication Security (Future)
- Password hashing (bcrypt)
- JWT tokens
- Refresh token rotation
- Session timeout
- Rate limiting

---

## 15. Monitoring & Analytics (Future)

### 15.1 Performance Monitoring
- Page load times
- API response times
- Error rates
- User engagement metrics

### 15.2 User Analytics
- Feature usage
- User journeys
- Conversion funnels
- A/B testing

---

## Document Version
- Version: 1.0
- Last Updated: February 15, 2026
- Status: Draft
