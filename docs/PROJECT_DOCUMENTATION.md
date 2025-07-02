# Angular RealWorld Example App - Project Documentation

## 1. Project Overview

**Purpose**: A social blogging platform (Medium.com clone) called "Conduit" built with Angular that demonstrates real-world application patterns.

**Domain**: Social media and content publishing platform for sharing knowledge and articles.

**Target Users**:

- **Content Creators**: Users who write and publish articles on various topics
- **Readers**: Users who consume content, follow authors, and engage with articles
- **Community Members**: Users who participate through comments, favorites, and following other users

**Core Value Proposition**: Provides a simplified yet fully-featured blogging platform that demonstrates modern Angular development practices while enabling knowledge sharing and community building.

## 2. Functional Documentation

### 2.1 Feature Inventory

#### Authentication & User Management

- **User Registration**: Anonymous users can create new accounts
- **User Login**: Existing users can authenticate with email/password
- **User Settings**: Authenticated users can update profile information (username, email, bio, profile image, password)
- **User Logout**: Authenticated users can terminate their session

#### Article Management

- **Article Creation**: Authenticated users can create new articles with title, description, body (markdown), and tags
- **Article Editing**: Article authors can edit their existing articles
- **Article Deletion**: Article authors can delete their articles
- **Article Viewing**: All users can read individual articles with rendered markdown content
- **Article Favoriting**: Authenticated users can favorite/unfavorite articles

#### Content Discovery

- **Global Feed**: Display paginated list of all articles
- **Personal Feed**: Display articles from followed users (authenticated users only)
- **Tag-based Filtering**: Filter articles by specific tags
- **Popular Tags**: Browse articles by popular tags displayed in sidebar
- **Article Search**: Filter articles by author or favorited by user

#### Social Features

- **User Profiles**: View user profile pages showing bio, stats, and articles
- **User Following**: Authenticated users can follow/unfollow other users
- **Comments**: Authenticated users can add comments to articles
- **Comment Deletion**: Comment authors can delete their comments

#### Content Organization

- **Pagination**: Navigate through large lists of articles (10 articles per page)
- **Article Lists**: View articles by author or user's favorited articles
- **Tag System**: Articles can be tagged and filtered by tags

### 2.2 User Journey Maps

#### Authentication Flow

1. User visits site → Sees global feed and can browse articles
2. User clicks "Sign in/Sign up" → Directed to authentication page
3. User submits credentials → JWT token stored in localStorage
4. User redirected to home page → Now sees personal feed option
5. User can access protected features (create articles, comment, follow, etc.)

#### Article Creation Workflow

1. Authenticated user clicks "New Article" → Directed to editor page
2. User fills form (title, description, body, tags) → Real-time tag management
3. User clicks "Publish Article" → Article created and user redirected to article view
4. Author can edit or delete article from article view page

**⚠️ CRITICAL BUG**: Article editing functionality is broken - the editor always calls `create()` method instead of `update()`, resulting in duplicate articles when attempting to edit.

#### Content Discovery Flow

1. User browses global feed → Sees paginated article list
2. User clicks on article → Views full article with comments
3. User can favorite article or follow author → Immediate UI feedback
4. User clicks on tag → Filters articles by that tag
5. User navigates pagination → Loads additional articles

### 2.3 Business Logic Rules

#### Authentication Rules

- JWT tokens stored in localStorage for session persistence
- Unauthenticated users redirected to login for protected actions
- Users cannot access settings, editor, or perform social actions without authentication
- Login/register pages inaccessible to authenticated users

#### Content Access Rules

- All articles publicly readable regardless of authentication status
- Only article authors can edit or delete their articles
- Only comment authors can delete their comments
- Personal feed only available to authenticated users

#### Data Validation Rules

- Authentication forms use Angular Reactive Forms with basic required validation
- Required fields: email, password for login; email, password, username for registration
- **No email format validation implemented** - any text accepted as email
- **No client-side validation on article forms** - title, description, body can be empty
- Settings form only requires password field - other fields optional

#### Social Interaction Rules

- **UI-Only Restrictions**: Follow and favorite buttons hidden when viewing own content (`@if (!isUser)` logic)
- **No actual prevention** of self-following or self-favoriting - these are UI conveniences only
- Following/unfavoriting requires authentication (redirects to login if not authenticated)
- Comment creation requires authentication

## 3. Technical Documentation

### 3.1 Architecture Overview

**Technology Stack**:

- **Framework**: Angular 20.0.0 with standalone components
- **Language**: TypeScript 5.8.3
- **Build Tool**: Angular CLI 20.0.0 with esbuild
- **HTTP Client**: Angular HttpClient with functional interceptors
- **State Management**: RxJS with BehaviorSubject for user state
- **Styling**: CSS with Bootstrap classes
- **Markdown**: Marked.js library for markdown parsing
- **Testing Framework**: Jasmine + Karma (configured but no tests implemented)
- **Node Runtime**: Node.js ^20.11.1

**Project Structure**:

```
src/app/
├── core/                    # Singleton services and app-wide components
│   ├── auth/               # Authentication module
│   ├── interceptors/       # HTTP interceptors
│   ├── layout/             # Header and footer components
│   └── models/             # Core data models
├── features/               # Feature modules
│   ├── article/            # Article management feature
│   ├── profile/            # User profile feature
│   └── settings/           # User settings feature
└── shared/                 # Reusable components and utilities
    ├── components/         # Shared UI components
    └── pipes/              # Custom pipes
```

**Design Patterns**:

- **Component Architecture**: Standalone components with lazy loading
- **Service Layer Pattern**: Injectable services for data access and business logic
- **Observer Pattern**: RxJS observables for reactive data flow
- **Interceptor Pattern**: HTTP interceptors for cross-cutting concerns
- **Guard Pattern**: Route guards for authentication

**Data Flow**:

1. User interactions trigger component methods
2. Components call service methods for data operations
3. Services make HTTP requests through interceptors
4. Interceptors handle API URL prefixing, authentication tokens, and error formatting
5. Responses flow back through observables to update UI

**Frontend Architecture**:

- **Component Hierarchy**: App → Layout (Header/Footer) → Routed Components → Feature Components
- **State Management**: Central user state in UserService with BehaviorSubject
- **Routing Strategy**: Standard routing (not hash-based) with lazy-loaded components
- **API Integration**: RESTful API consumption with RxJS observables

### 3.2 Development Environment

**Prerequisites**:

- Node.js ^20.11.1
- npm (package manager)
- Angular CLI (installed globally via `npm install -g @angular/cli`)

**Installation Steps**:

**⚠️ Package Manager Discrepancy Note**: The README.md recommends using Yarn, but the project contains `package-lock.json`, indicating npm is the actual package manager used. Follow these npm-based instructions:

```bash
# Clone the repository
git clone <repository-url>
cd angular-realworld-example-app

# Install dependencies using npm (NOT yarn despite README recommendation)
npm install

# Start development server
npm start
# or
ng serve
```

**Configuration**:

- **API Configuration**: Hardcoded API base URL `https://api.realworld.io/api` in `src/app/core/interceptors/api.interceptor.ts`
- **No Environment Files**: No environment configuration files exist - API URL is not configurable
- **JWT Storage**: Authentication tokens stored in browser localStorage

**Common Commands**:

```bash
# Development server (http://localhost:4200)
npm start

# Production build
npm run build

# Linting
npm run lint

# Testing (no tests currently implemented)
npm test

# Serve documentation
npm run serve:docs
```

### 3.3 Code Organization

**Directory Structure**:

- **`src/app/core/`**: Singleton services, authentication, interceptors, layout components
- **`src/app/features/`**: Feature-specific modules (article, profile, settings)
- **`src/app/shared/`**: Reusable components, pipes, and utilities
- **`src/assets/`**: Static assets (images, fonts, etc.)

**File Naming Conventions**:

- `*.component.ts` - Angular components
- `*.service.ts` - Injectable services
- `*.model.ts` - TypeScript interfaces
- `*.pipe.ts` - Custom pipes
- `*.directive.ts` - Custom directives
- All files use kebab-case naming

**Import/Export Patterns**:

- ES6 imports with named exports
- Relative imports for local files
- Absolute imports for Angular core modules
- Service injection using `inject()` function

**State Management**:

- **User State**: Centralized in `UserService` using BehaviorSubject
- **Component State**: Local state using component properties and RxJS operators
- **Authentication State**: Boolean observable derived from user state
- **Loading States**: Enum-based loading state management

### 3.4 API Documentation

**External APIs**:

- **RealWorld API**: `https://api.realworld.io/api` - RESTful API for all backend operations
- **Authentication**: JWT token-based authentication
- **Content Type**: JSON for all requests/responses

**API Integration**:

- **HTTP Client**: Angular HttpClient with functional interceptors
- **Base URL**: Automatically prefixed by `apiInterceptor`
- **Authentication**: JWT token automatically added to requests via `tokenInterceptor`
- **Error Handling**: Standardized error format via `errorInterceptor`

**Authentication Flow**:

- Login/Register returns JWT token
- Token stored in localStorage via `JwtService`
- Token automatically included in subsequent API requests
- Token removal on logout clears authentication state

**Error Handling**:

- Global error interception via `errorInterceptor`
- Error objects follow `{ errors: { [key: string]: string } }` interface
- Error display via `ListErrorsComponent`
- Client-side error recovery through retry patterns

### 3.5 Data Management

**Client-Side State**:

- **User Authentication**: BehaviorSubject in `UserService` for current user state
- **Article Lists**: Component-level state with pagination support
- **Form State**: Angular Reactive Forms for all user inputs
- **Loading States**: Enum-based loading state tracking

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

interface Profile {
  username: string;
  bio: string;
  image: string;
  following: boolean;
}

interface Comment {
  id: string;
  body: string;
  createdAt: string;
  author: Profile;
}
```

**Local Storage**:

- **JWT Token**: Stored as `jwtToken` in localStorage
- **Session Persistence**: User remains logged in across browser sessions
- **Token Management**: Automatic cleanup on logout

**Data Synchronization**:

- **Real-time Updates**: Immediate UI updates for user actions (follow, favorite)
- **Optimistic Updates**: UI updates before server confirmation
- **Error Recovery**: Rollback on failed operations

## 4. Development Guidelines

### 4.1 Code Standards

**Coding Style**:

- TypeScript strict mode enabled
- Single quotes for string literals
- 2-space indentation
- Trailing comma enforcement via Prettier
- Consistent import ordering (Angular → RxJS → Libraries → Local)

**Linting Rules**:

- ESLint with Angular-specific rules
- Prettier for code formatting
- Husky pre-commit hooks with lint-staged
- Force linting via `ng lint --force`

**Angular Best Practices**:

- Standalone components preferred
- `inject()` function for dependency injection
- `takeUntilDestroyed()` for subscription management
- Reactive forms for all user inputs
- OnPush change detection where applicable

**Git Workflow**:

- Husky pre-commit hooks configured
- Prettier formatting enforced on commit
- Lint-staged runs on `*.{ts,html,css,json,md}` files

### 4.2 Testing Strategy

**Testing**: No test files found in the codebase. Testing framework (Jasmine + Karma) is configured in `package.json` and `angular.json` but no tests are implemented.

**Configured but Not Implemented**:

- Jasmine testing framework
- Karma test runner
- Code coverage reporting
- Chrome headless testing setup

### 4.3 Performance Considerations

**Optimization Strategies**:

- **Lazy Loading**: Components loaded on-demand via dynamic imports
- **OnPush Change Detection**: Used in presentational components
- **TrackBy Functions**: Implemented for `*ngFor` loops in article lists
- **Bundle Optimization**: Angular CLI build optimization with tree-shaking
- **Image Optimization**: NgOptimizedImage directive available but not currently used

**Build Performance**:

- **Bundle Budget Limits**: 500kb initial warning, 1MB error limit
- **Component Style Budget**: 2kb warning, 4kb error limit
- **Code Splitting**: Automatic via Angular CLI and dynamic imports

## 5. Deployment & Operations

### 5.1 Build Process

**Build Configuration**:

- **Build Tool**: Angular CLI with esbuild application builder
- **Output Directory**: `dist/angular-conduit/`
- **Production Optimizations**: Minification, tree-shaking, output hashing
- **Development Build**: Source maps enabled, no optimization

**Build Commands**:

```bash
# Development build
ng build

# Production build
ng build --configuration production

# Build with output hashing
ng build --output-hashing all
```

**Asset Handling**:

- **Static Assets**: Copied from `src/assets/` and `src/favicon.ico`
- **Redirects**: Netlify `_redirects` file included for SPA routing
- **Styles**: Global CSS from `src/styles.css`

### 5.2 Deployment Process

**Static Hosting**:

- **Target**: Static hosting platforms (Netlify, Vercel, GitHub Pages)
- **SPA Routing**: Requires server configuration for client-side routing
- **Redirects**: `_redirects` file configured for Netlify deployment
- **Build Output**: Single-page application with static assets

**Environment Configuration**:

- **API URL**: Hardcoded in interceptor - no runtime configuration
- **No Environment Variables**: No environment-specific configuration files exist
- **Build-time Configuration**: All settings compiled into build artifacts

### 5.3 Monitoring & Debugging

**Browser Developer Tools**:

- Angular DevTools extension for component inspection
- RxJS debugging through observable streams
- Network tab for API request monitoring
- Console logging for error tracking

**Debugging Techniques**:

- Component state inspection via Angular DevTools
- Observable stream debugging with RxJS operators
- HTTP request/response monitoring via network inspector
- Local storage inspection for JWT token debugging

## 6. Quick Start Guides

### 6.1 For New Developers

**5-Step Onboarding**:

1. **Environment Setup**:

   ```bash
   # Install Node.js ^20.11.1 and Angular CLI globally
   npm install -g @angular/cli
   git clone <repository-url>
   cd angular-realworld-example-app
   ```

2. **Dependency Installation**:

   ```bash
   # Use npm (not yarn despite README recommendation)
   npm install
   ```

3. **Start Development Server**:

   ```bash
   npm start
   # Access application at http://localhost:4200
   ```

4. **Understand Project Structure**:

   - Review `src/app/core/` for authentication and services
   - Explore `src/app/features/` for main application features
   - Check `src/app/shared/` for reusable components

5. **Make First Change**:
   - Modify home page banner text in `src/app/features/article/pages/home/home.component.html`
   - Observe hot reload in browser

**First Feature to Implement**:
Implement user avatar display in article previews:

1. Add avatar image to `ArticlePreviewComponent` template
2. Style avatar with CSS classes
3. Link avatar to author profile page
4. Test with different user profiles

**Common Pitfalls**:

- **Package Manager**: Don't use yarn commands - use npm despite README
- **Global CLI**: Ensure Angular CLI installed globally for `ng` commands
- **API Dependencies**: Application requires internet connection for backend API
- **Authentication**: Many features require user authentication to test properly

### 6.2 For QA Engineers

**Testing Framework Status**: No test suites exist in the codebase.

**Manual Testing Areas**:

**Authentication Testing**:

- User registration with valid/invalid credentials
- User login with correct/incorrect credentials
- Session persistence across browser restarts
- Logout functionality and state cleanup

**Article Management Testing**:

- Article creation with various content types
- Article editing permissions (author-only)
- Article deletion permissions (author-only)
- Markdown rendering verification

**Social Features Testing**:

- Follow/unfollow user interactions
- Article favoriting/unfavoriting
- Comment creation and deletion
- Permission checks for all social actions

**Navigation and UI Testing**:

- Client-side routing functionality
- Pagination on article lists
- Tag filtering and search
- Responsive design on mobile devices

**Bug Reporting**:

- Include browser version and device information
- Provide steps to reproduce with specific user credentials
- Note authentication state when issue occurs
- Include console errors and network request failures

## 7. Troubleshooting

### Common Issues

**Package Manager Conflicts**:

- **Issue**: README mentions yarn but package-lock.json exists
- **Solution**: Use npm commands exclusively, ignore yarn references in README
- **Prevention**: Stick to npm for all dependency management

**Global Angular CLI Missing**:

- **Issue**: `ng` command not found
- **Solution**: Install Angular CLI globally: `npm install -g @angular/cli`
- **Prevention**: Include global CLI installation in setup instructions

**Authentication State Issues**:

- **Issue**: User appears logged out after page refresh
- **Solution**: Check localStorage for `jwtToken`, clear if corrupted
- **Prevention**: Implement token expiration handling

**API Connection Problems**:

- **Issue**: Network errors when loading articles or authenticating
- **Solution**: Verify internet connection and API availability at `https://api.realworld.io/api`
- **Prevention**: Implement offline state handling

### Environment Issues

**Node Version Mismatch**:

- **Issue**: Build failures with wrong Node.js version
- **Solution**: Use Node.js ^20.11.1 as specified in package.json engines
- **Prevention**: Use Node Version Manager (nvm) to maintain correct version

**Port Already in Use**:

- **Issue**: Default port 4200 unavailable
- **Solution**: Use different port: `ng serve --port 4201`
- **Prevention**: Check running processes before starting dev server

### Runtime Errors

**Component Loading Failures**:

- **Issue**: Lazy-loaded components fail to load
- **Solution**: Check for TypeScript compilation errors and fix imports
- **Prevention**: Use Angular Language Service in IDE for early error detection

**HTTP Request Failures**:

- **Issue**: API requests return CORS or network errors
- **Solution**: Verify API interceptors are configured correctly
- **Prevention**: Implement proper error handling and user feedback

### Performance Issues

**Slow Initial Load**:

- **Issue**: Application takes long time to load initially
- **Solution**: Analyze bundle size with `ng build --stats-json` and webpack-bundle-analyzer
- **Prevention**: Implement proper lazy loading and code splitting

**Memory Leaks**:

- **Issue**: Application becomes sluggish over time
- **Solution**: Check for unsubscribed observables and use `takeUntilDestroyed()`
- **Prevention**: Follow subscription management best practices consistently

---

_This documentation reflects the actual implementation as of the current codebase state. All features and configurations documented here have been verified against the source code._
