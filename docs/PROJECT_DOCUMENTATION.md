# Angular RealWorld Example App - Project Documentation

## 1. Project Overview

**Purpose**: A social blogging platform (Medium.com clone) called "Conduit" that allows users to create, read, and interact with articles.

**Domain**: Social content publishing and consumption platform for bloggers and readers.

**Target Users**:

- **Writers**: Users who create and publish articles with markdown support
- **Readers**: Users who consume, favorite, and comment on articles
- **Social Users**: Users who follow other writers and engage with content

**Core Value Proposition**: Provides a clean, modern interface for content creators to share articles and build an audience through social features like following, favoriting, and commenting.

## 2. Functional Documentation

### 2.1 Feature Inventory

**Authentication & User Management**

- **User Registration**: Anonymous users can create accounts with username, email, password
- **User Login**: Registered users can authenticate with email/password
- **User Settings**: Authenticated users can update profile information (username, email, bio, image, password)
- **User Logout**: Authenticated users can sign out and clear session

**Article Management**

- **Article Creation**: Authenticated users can create new articles with title, description, body (markdown), and tags
- **⚠️ Article Editing**: **CRITICAL BUG** - Editor component always creates new articles instead of updating existing ones due to implementation error (calls `create()` method regardless of edit context)
- **Article Deletion**: Article authors can delete their articles
- **Article Viewing**: All users can read articles with rendered markdown content

**Social Features**

- **Article Favoriting**: Authenticated users can favorite/unfavorite articles
- **User Following**: Authenticated users can follow/unfollow other users
- **Profile Viewing**: All users can view user profiles showing their articles and favorites
- **Commenting**: Authenticated users can add and delete comments on articles

**Content Discovery**

- **Article Feed**: Authenticated users see personalized feeds from followed users
- **Global Feed**: All users can browse all public articles
- **Tag-based Filtering**: All users can filter articles by tags
- **Pagination**: Article lists support pagination for performance

### 2.2 User Journey Maps

**Authentication Flow**:

1. User visits login/register page
2. Submits credentials via form
3. JWT token stored in localStorage
4. User redirected to home page
5. Authenticated state persists across sessions

**Article Creation Workflow**:

1. Authenticated user navigates to editor
2. Fills article form (title, description, body, tags)
3. Submits article for creation
4. Redirected to published article view
5. Article appears in user's profile and global feed

**Social Interaction Flow**:

1. User discovers article via feed or profile
2. Reads article content with rendered markdown
3. Can favorite article or follow author
4. Can add comments to engage with content
5. Actions update user's social graph

### 2.3 Business Logic Rules

**Authentication Rules**:

- JWT tokens required for protected routes (editor, settings, profile actions)
- Unauthenticated users redirected to login for protected actions
- Token persistence in localStorage for session management

**Content Ownership Rules**:

- Only article authors can edit/delete their articles
- Only comment authors can delete their comments
- All users can view public articles and profiles

**Data Validation Rules**:

- User registration requires unique username and email
- Article creation requires title and body content
- All forms include client-side validation for required fields
- **⚠️ Inconsistent Email Validation**: Authentication forms use `type="text"` for email fields while settings form uses HTML5 `type="email"` validation

## 3. Technical Documentation

### 3.1 Architecture Overview

**Technology Stack**:

- **Framework**: Angular 20.0.0
- **Language**: TypeScript 5.8.3
- **Styling**: RealWorld CSS Framework (loaded via CDN), Ionicons, Google Fonts
- **HTTP Client**: Angular HttpClient
- **State Management**: RxJS with BehaviorSubject pattern
- **Build Tool**: Angular CLI with esbuild
- **Runtime**: Node.js ^20.11.1
- **External Dependencies**: RX Angular reactive extensions, Marked (markdown parsing)

**Project Structure**:

```
src/app/
├── core/                    # Shared services and utilities
│   ├── auth/               # Authentication components and services
│   ├── interceptors/       # HTTP interceptors
│   ├── layout/            # Header and footer components
│   └── models/            # Shared data models
├── features/              # Feature-specific modules
│   ├── article/          # Article CRUD and display
│   ├── profile/          # User profile management
│   └── settings/         # User settings management
└── shared/               # Reusable components and utilities
```

**Design Patterns**:

- **Standalone Components**: All components use Angular's standalone component architecture
- **Lazy Loading**: Feature modules loaded on demand via router
- **Dependency Injection**: Services injected using Angular's inject() function
- **Reactive Programming**: Extensive use of RxJS Observables for state management
- **Interceptor Pattern**: HTTP requests processed through authentication and error interceptors

**Data Flow**:

1. Components inject services using Angular DI
2. Services make HTTP calls through interceptors
3. API responses update BehaviorSubject state
4. Components subscribe to Observable state changes
5. Templates reactively update via async pipe

**Routing Strategy**: Standard Angular routing (not hash-based) with lazy-loaded feature modules and route guards for authentication.

### 3.2 Development Environment

**Prerequisites**:

- Node.js ^20.11.1
- Angular CLI (required globally for ng commands)

**⚠️ Package Manager Conflict**:

- **Actual Setup**: Project uses npm (package-lock.json present)
- **README Claims**: README mentions Yarn but this conflicts with actual setup
- **Resolution**: Use npm commands as shown below, not yarn

**Installation Steps**:

```bash
# Clone the repository
git clone https://github.com/jiteshy/angular-realworld-example-app.git
cd angular-realworld-example-app

# Install dependencies (use npm, not yarn despite README)
npm install

# Start development server
npm start
# OR
ng serve

# Navigate to http://localhost:4200
```

**Configuration**:

- **Environment Files**: Does not exist - no environment-based configuration
- **API Configuration**: Hardcoded to `https://api.realworld.io/api` in api.interceptor.ts
- **Development Port**: 4200 (Angular CLI default)

**Common Commands**:

```bash
# Development
npm start              # Start dev server
ng serve              # Alternative start command

# Building
npm run build         # Production build
ng build              # Alternative build command

# Code Quality
npm run lint          # Run ESLint
npm run lint --force  # Ignore lint warnings

# Documentation
npm run serve:docs    # Serve docs on port 8080
```

### 3.3 Code Organization

**Directory Structure**:

- `src/app/core/`: Authentication, interceptors, layout components, shared models
- `src/app/features/`: Business logic grouped by domain (articles, profiles, settings)
- `src/app/shared/`: Reusable UI components and utilities
- `src/assets/`: Static assets (favicon, images)

**File Naming Conventions**:

- Components: `*.component.ts` (kebab-case)
- Services: `*.service.ts` (kebab-case)
- Models: `*.model.ts` (kebab-case)
- Directives: `*.directive.ts` (kebab-case)
- Pipes: `*.pipe.ts` (kebab-case)

**Import/Export Patterns**:

- Named exports for all components and services
- Relative imports for local modules
- Absolute imports for Angular core modules
- Barrel exports not used

**State Management**:

- BehaviorSubject pattern for user authentication state
- Service-based state management without external libraries
- Reactive forms for user input handling
- RxJS operators for data transformation

### 3.4 API Documentation

**External APIs**:

- **RealWorld API**: `https://api.realworld.io/api` (hardcoded in api.interceptor.ts)
- **Purpose**: Backend service providing all CRUD operations for articles, users, comments

**API Integration**:

- All HTTP requests automatically prefixed with base URL via interceptor
- JWT token automatically attached to requests via token interceptor
- Error responses handled globally via error interceptor
- API responses wrapped in Observable streams

**Authentication Flow**:

- Login/Register endpoints return JWT token
- Token stored in localStorage via JwtService
- Token included in Authorization header for protected requests
- Token cleared on logout or authentication errors

**Error Handling**:

- Global error interceptor catches HTTP errors
- Network errors displayed to users via error components
- Authentication errors trigger automatic logout
- Service-level error handling with fallback states

### 3.5 Data Management

**Client-Side State**:

- User authentication state managed via BehaviorSubject in UserService
- Article data fetched on-demand and not persisted client-side
- Form state managed through Angular Reactive Forms
- No complex client-side caching implemented

**Data Models**:

```typescript
interface User {
  email: string;
  token: string;
  username: string;
  bio: string;
  image: string;
}

interface Article {
  slug: string;
  title: string;
  description: string;
  body: string;
  tagList: string[];
  createdAt: string;
  updatedAt: string;
  favorited: boolean;
  favoritesCount: number;
  author: Profile;
}
```

**Local Storage**:

- JWT tokens stored in localStorage for session persistence
- No other client-side data persistence implemented
- Token automatically retrieved on app initialization

**Data Synchronization**:

- Real-time synchronization not implemented
- Data refreshed through user-initiated actions
- Optimistic updates for favoriting actions
- Manual refresh required for new content

## 4. Development Guidelines

### 4.1 Code Standards

**Coding Style**:

- Single quotes for string literals
- 2-space indentation
- Kebab-case for file names
- camelCase for variables and functions
- PascalCase for classes and interfaces

**Linting and Formatting**:

- ESLint configured with Angular recommended rules
- Prettier configured for code formatting
- Husky git hooks for pre-commit linting
- Lint-staged for efficient formatting

**Git Workflow**:

- Pre-commit hooks enforce code formatting
- Prettier runs on TypeScript, HTML, CSS, JSON, and Markdown files
- No specific branch naming conventions documented

### 4.2 Testing Strategy

**Testing**: No test files found in the codebase. Testing framework configured but completely broken.

- Jasmine and Karma configured in package.json
- **⚠️ CRITICAL**: `npm test` command fails with TypeScript configuration errors due to missing test files
- Testing infrastructure present but non-functional (tsconfig.spec.json expects test files that don't exist)

### 4.3 Performance Considerations

**Bundle Optimization**:

- Lazy loading implemented for feature modules
- Standalone components reduce bundle size
- esbuild used for fast compilation

**Runtime Performance**:

- Async pipe used for reactive data binding
- OnPush change detection not implemented
- No image optimization implemented

## 5. Deployment & Operations

### 5.1 Build Process

**Build Commands**:

```bash
npm run build    # Production build
ng build         # Alternative command
```

**Build Output**:

- Static files generated in `dist/` directory
- Angular CLI with esbuild for optimization
- No specific environment configurations

**Asset Handling**:

- Static assets served from `src/assets/`
- **External CDN Dependencies**: RealWorld CSS framework, Ionicons, Google Fonts loaded from external CDNs
- Basic favicon and logo assets included
- No custom CDN integration for local assets

### 5.2 Deployment Process

**Static Hosting**:

- Application built as static files suitable for any web server
- No server-side rendering implemented
- Requires routing configuration for single-page application

**Environment Configuration**:

- No environment-specific configurations available
- API URL hardcoded in source code
- No build-time variable injection

### 5.3 Monitoring & Debugging

**Error Tracking**:

- Global error interceptor for HTTP errors
- Console logging for development debugging
- No external error tracking services integrated

**Performance Monitoring**:

- No performance monitoring implemented
- Standard browser developer tools available
- No analytics or user behavior tracking

## 6. Quick Start Guides

### 6.1 For New Developers

**5-Step Onboarding**:

1. **Setup**: Install Node.js 20.11.1+ and Angular CLI globally
2. **Install**: Clone repo and run `npm install` (ignore README's yarn reference)
3. **Run**: Execute `npm start` and open http://localhost:4200
4. **Explore**: Navigate through login, article creation, and user profiles
5. **Code**: Start with `src/app/features/` to understand feature implementation

**First Feature Implementation**:
Create a new article tag component:

1. Generate component: `ng generate component features/article/components/tag-list`
2. Add to article template with tag display logic
3. Style with CSS classes following existing patterns
4. Test integration with article data

**Common Pitfalls**:

- Don't mix npm and yarn commands despite README suggestions
- Remember that routes require authentication - check UserService.isAuthenticated
- API calls automatically prefixed - don't add base URL manually

### 6.2 For QA Engineers

**Testing Areas**:

- **Authentication**: Login/register with various credential combinations
- **Article Management**: Create, edit, delete articles with different content types
- **Social Features**: Follow/unfollow users, favorite articles, add comments
- **Navigation**: Test routing between all pages and authentication redirects

**Test Data**:

- Use RealWorld API for backend testing
- Create test accounts for different user roles
- Test with articles containing markdown formatting

**Bug Reporting**:

- Include browser console errors
- Note authentication state when issues occur
- Test across different user permission levels

## 7. Troubleshooting

**Common Issues**:

**Installation Problems**:

- _Issue_: Package manager conflicts between README and actual setup
- _Solution_: Use npm commands only, ignore yarn references in README

**Development Server Issues**:

- _Issue_: ng command not found
- _Solution_: Install Angular CLI globally: `npm install -g @angular/cli`

**Authentication Problems**:

- _Issue_: Unexpected logouts or authentication errors
- _Solution_: Check browser localStorage for JWT token, clear and re-login

**API Connection Issues**:

- _Issue_: Network errors when accessing features
- _Solution_: Verify RealWorld API availability at https://api.realworld.io/api

**Build Failures**:

- _Issue_: TypeScript compilation errors
- _Solution_: Ensure Node.js version matches requirement (^20.11.1)
- _Solution_: Clear node_modules and reinstall: `rm -rf node_modules && npm install`

**Performance Issues**:

- _Issue_: Slow page loads or navigation
- _Solution_: Check browser network tab for API response times
- _Solution_: Clear browser cache and localStorage

---

## ⚠️ Critical Issues Summary

### Functional Bugs

1. **Article Editor Bug**: Editing existing articles creates duplicates instead of updating them due to implementation error in `src/app/features/article/pages/editor/editor.component.ts` (line 82-88)

### Development Issues

2. **Testing Infrastructure Broken**: `npm test` fails with TypeScript configuration errors - testing framework configured but non-functional
3. **Inconsistent Email Validation**: Auth forms use basic text validation while settings form uses HTML5 email validation

### Documentation Accuracy

4. **External Dependencies**: Application uses RealWorld CSS framework, Ionicons, and Google Fonts via CDN (not "no framework detected")

_This documentation reflects validated analysis with all critical issues identified through systematic verification against the actual codebase._
