# Angular RealWorld Example App Documentation

## 1. Project Overview

**Purpose**: A social blogging platform (Medium.com clone) called "Conduit" that demonstrates modern Angular development patterns.

**Domain**: Social media/content publishing platform with article creation, user interactions, and community features.

**Target Users**: 
- Content creators who write and publish articles
- Readers who consume content and engage through comments and favorites
- Community members who follow other users

**Core Value Proposition**: Provides a complete example of a production-ready Angular application with authentication, CRUD operations, and social features using modern Angular patterns.

## 2. Functional Documentation

### 2.1 Feature Inventory

**Authentication System**
- User role: Anonymous and authenticated users
- Primary use case: Account management and secure access
- Key user flows: 
  1. Register with username, email, password
  2. Login with email, password
  3. Automatic authentication check on app initialization
  4. Logout with token cleanup
  5. Settings page for profile updates

**Article Management**
- User role: Authenticated users (authors)
- Primary use case: Content creation and management
- Key user flows:
  1. Create new article with title, description, body, tags
  2. Edit existing articles (author only)
  3. Delete articles (author only)
  4. View article with markdown rendering
  5. Browse articles with pagination

**Article Discovery**
- User role: All users
- Primary use case: Content consumption and exploration
- Key user flows:
  1. Browse global feed of all articles
  2. Browse personal feed (authenticated users only)
  3. Filter articles by tags
  4. Paginated article listing
  5. Tag-based article discovery

**Social Interactions**
- User role: Authenticated users
- Primary use case: Community engagement
- Key user flows:
  1. Favorite/unfavorite articles
  2. Follow/unfollow other users
  3. Add comments to articles
  4. Delete own comments
  5. View user profiles with articles

**User Profile Management**
- User role: All users (view), authenticated users (edit)
- Primary use case: User information and content management
- Key user flows:
  1. View user profiles with bio and image
  2. Browse user's published articles
  3. Browse user's favorited articles
  4. Edit own profile settings
  5. Profile image and bio management

### 2.2 User Journey Maps

**Authentication Flow**:
1. User accesses application
2. App checks for existing JWT token in localStorage
3. If token exists, automatically fetches current user data
4. User can login/register or access public content
5. Successful authentication stores JWT token and redirects to home

**Core Article Workflow**:
1. User browses articles on home page (global/personal feed)
2. User clicks on article to read full content
3. User can favorite article or follow author
4. Authenticated users can add comments
5. Article authors can edit/delete their content

**Content Creation Flow**:
1. Authenticated user navigates to editor
2. User creates article with title, description, body, tags
3. Article is published and user redirected to article view
4. Article appears in author's profile and global feed

### 2.3 Business Logic Rules

**Authentication Rules**:
- JWT tokens stored in localStorage with key "jwtToken"
- Authentication required for: creating articles, commenting, favoriting, following
- Token automatically added to API requests via interceptor
- Invalid tokens trigger automatic logout

**Article Access Rules**:
- All users can view articles and comments
- Only authenticated users can create/favorite articles
- Only article authors can edit/delete their articles
- Article slugs used for URL routing and API identification

**Comment System Rules**:
- Comments require authentication
- Users can only delete their own comments
- Comments displayed in chronological order (newest first)
- Comment creation updates UI immediately on success

**Following/Favoriting Rules**:
- Requires user authentication
- Following affects personal feed content
- Favoriting affects user's favorites list
- Actions are immediately reflected in UI

## 3. Technical Documentation

### 3.1 Architecture Overview

**Technology Stack**:
- Angular: 20.0.0
- TypeScript: ~5.8.3
- RxJS: ^7.4.0
- Node.js: ^20.11.1 (required)
- Additional libraries: @rx-angular/cdk, @rx-angular/template, marked (markdown parser)

**Project Structure**:
```
src/app/
├── core/                    # Core application modules
│   ├── auth/               # Authentication system
│   ├── interceptors/       # HTTP interceptors
│   ├── layout/            # Header/footer components
│   └── models/            # Core data models
├── features/              # Feature modules
│   ├── article/           # Article management
│   ├── profile/           # User profiles
│   └── settings/          # User settings
└── shared/               # Shared components and utilities
```

**Design Patterns**:
- **Standalone Components**: Modern Angular pattern using standalone components throughout
- **Service-based Architecture**: Services handle all API communication and business logic
- **Reactive Programming**: Heavy use of RxJS observables for state management
- **Component Composition**: Modular component design with clear separation of concerns
- **Interceptor Pattern**: HTTP request/response handling for API integration and authentication

**Data Flow**:
1. Components inject services for data operations
2. Services handle HTTP requests through Angular HttpClient
3. Interceptors automatically handle API URL prefixing and token injection
4. Observables stream data to components
5. Components use async pipe for template rendering

**Component Hierarchy**:
- App component serves as root with router outlet
- Feature components lazy-loaded via Angular router
- Shared components imported where needed
- Layout components (header/footer) provide consistent structure

**State Management**:
- No global state management library (NgRx, Akita, etc.)
- Services with BehaviorSubjects for shared state
- Local component state for UI-specific data
- Authentication state managed via UserService

**Routing Strategy**:
- Standard routing (not hash-based) configured in app.routes.ts
- Lazy loading for feature modules
- Route guards for authentication protection
- Functional guards using inject() pattern

### 3.2 Development Environment

**Prerequisites**:
- Node.js ^20.11.1
- npm (package manager)
- Angular CLI (requires global installation)

**CRITICAL PACKAGE MANAGER DISCREPANCY**: 
- README.md recommends using Yarn and `yarn install`
- Project actually uses npm (package-lock.json present, no yarn.lock)
- **Use npm commands only** - mixing package managers can cause dependency issues

**Installation Steps**:
```bash
# Verify Node.js version
node --version  # Should be ^20.11.1

# Install Angular CLI globally (required for ng commands)
npm install -g @angular/cli

# Clone repository
git clone <repository-url>
cd angular-realworld-example-app

# Install dependencies using npm (NOT yarn despite README)
npm install

# Start development server
npm start
```

**Configuration**:
- **API Configuration**: Hardcoded to `https://api.realworld.io/api` in `src/app/core/interceptors/api.interceptor.ts`
- **Environment Configuration**: Does not exist - no environment.ts or .env files found
- **Build Configuration**: Managed via angular.json with production/development configurations

**Common Commands**:
```bash
# Development
npm start              # Start dev server (ng serve)
npm run build         # Build for production (ng build)
npm run lint          # Run linting (ng lint --force)

# Note: npm test available but no test files exist
# Note: All ng commands require Angular CLI to be globally installed
```

### 3.3 Code Organization

**Directory Structure**:
- `core/`: Application-wide services, models, and utilities
- `features/`: Domain-specific modules organized by business feature
- `shared/`: Reusable components, pipes, and utilities
- `assets/`: Static assets and images

**File Naming Conventions**:
- Components: `*.component.ts` (kebab-case)
- Services: `*.service.ts` (kebab-case)
- Models: `*.model.ts` (kebab-case)
- Pipes: `*.pipe.ts` (kebab-case)
- Routes: `*.routes.ts` (kebab-case)

**Import/Export Patterns**:
- Standalone components with explicit imports array
- Named exports for services and utilities
- Default exports for routed components
- Barrel exports not used - direct imports throughout

**State Management**:
- Services use BehaviorSubject for reactive state
- UserService manages authentication state globally
- Component-level state for UI-specific data
- No global state management library

### 3.4 API Documentation

**External APIs**: 
- **RealWorld API**: Complete integration with https://api.realworld.io/api
- **Authentication**: JWT-based with token stored in localStorage
- **All endpoints**: Automatically prefixed via API interceptor

**API Integration**:
- **Base URL**: Hardcoded as `https://api.realworld.io/api` (not configurable)
- **HTTP Client**: Angular's HttpClient with interceptors
- **Request Flow**: Component → Service → HttpClient → Interceptors → API

**Authentication Flow**:
- JWT token stored in localStorage with key "jwtToken"
- Token automatically added to request headers as `Authorization: Token {jwt}`
- Token validation on app initialization
- Automatic logout on token expiration/invalid responses

**Key API Endpoints** (via services):
```typescript
// Authentication
POST /users/login          # User login
POST /users               # User registration  
GET /user                 # Current user data
PUT /user                 # Update user

// Articles
GET /articles             # List articles
GET /articles/feed        # Personal feed
GET /articles/:slug       # Get article
POST /articles            # Create article
PUT /articles/:slug       # Update article
DELETE /articles/:slug    # Delete article
POST /articles/:slug/favorite    # Favorite article
DELETE /articles/:slug/favorite  # Unfavorite article

// Comments
GET /articles/:slug/comments     # Get comments
POST /articles/:slug/comments    # Add comment
DELETE /articles/:slug/comments/:id  # Delete comment

// Profile
GET /profiles/:username   # Get profile
POST /profiles/:username/follow    # Follow user
DELETE /profiles/:username/follow  # Unfollow user

// Tags
GET /tags                 # Get all tags
```

**Error Handling**:
- Error interceptor converts API errors to application format
- Form validation errors displayed via ListErrorsComponent
- Network errors handled gracefully with loading states
- Authentication errors trigger automatic logout

### 3.5 Data Management

**Client-Side State**:
- **User State**: Managed by UserService with BehaviorSubject
- **Authentication State**: Reactive observable from UserService
- **Component State**: Local state for UI-specific data
- **Form State**: Angular reactive forms for user input

**Data Models** (TypeScript interfaces):
```typescript
// Core models
User: email, token, username, bio, image
Article: slug, title, description, body, tagList, dates, favorites, author
Comment: id, body, createdAt, author
Profile: username, bio, image, following
```

**Local Storage**:
- **JWT Token**: Stored as "jwtToken" for authentication persistence
- **No other data**: Application relies on API for all data
- **Session Management**: Token-based, no additional session storage

**Data Synchronization**:
- Real-time updates through immediate UI updates after API responses
- Optimistic updates for favorites/following actions
- No polling or WebSocket connections
- Manual refresh required for latest data

## 4. Development Guidelines

### 4.1 Code Standards

**Coding Style**:
- TypeScript with strict type checking
- Single quotes for string literals
- 2-space indentation
- Trailing whitespace removal via prettier
- Standalone components with explicit imports

**Linting and Formatting**:
- Angular ESLint configuration
- Prettier for code formatting
- Husky pre-commit hooks with lint-staged
- Force mode linting (--force flag used)

**Git Workflow**:
- Standard Git workflow (no specific branching strategy documented)
- Prettier runs on commit via husky hooks
- File patterns: "*.{ts,html,css,json,md}"

### 4.2 Testing Strategy

**Testing**: No test files found in the codebase. Testing framework may be configured but no tests are implemented.

- Karma and Jasmine configured in package.json
- Test command available (`npm test`) but will fail as no test files exist
- Testing types configured: jasmine-core, karma-coverage, karma-jasmine-html-reporter

### 4.3 Performance Considerations

**Implemented Optimizations**:
- Lazy loading for all feature routes
- Standalone components reduce bundle size
- OnPush change detection strategy not implemented
- TrackBy functions used in article lists for performance
- RxJS operators for efficient data streaming

**Bundle Configuration**:
- Initial bundle: 500kb warning threshold, 1mb error threshold
- Component styles: 2kb warning threshold, 4kb error threshold  
- Output hashing enabled for production builds

## 5. Deployment & Operations

### 5.1 Build Process

**Build Commands**:
```bash
npm run build              # Production build
ng build --configuration development  # Development build
```

**Build Outputs**:
- Output directory: `dist/angular-conduit`
- Production: Optimized bundles with output hashing
- Development: Unoptimized with source maps

**Asset Handling**:
- Favicon and assets folder copied to dist
- `_redirects` file included for SPA routing support
- CSS bundling from styles.css

### 5.2 Deployment Process

**Static Hosting Setup**:
- SPA routing support via `_redirects` file
- All routes should serve index.html for Angular router
- Production build creates optimized static assets
- No server-side rendering (SSR) configured

**Environment Configuration**:
- **No environment files exist** - API URL is hardcoded
- **Not configurable at build time** - would require code changes
- **No environment variable injection** setup

### 5.3 Monitoring & Debugging

**Frontend Debugging**:
- Angular DevTools compatible
- Source maps available in development mode
- RxJS debugging through browser dev tools
- Network requests visible in browser dev tools

**Error Tracking**:
- No error tracking service integrated (Sentry, Bugsnag, etc.)
- Errors logged to browser console
- API errors handled gracefully with user feedback

## 6. Quick Start Guides

### 6.1 For New Developers

**5-Step Onboarding**:

1. **Environment Setup**
   ```bash
   # Verify Node.js ^20.11.1
   node --version
   
   # Install Angular CLI globally
   npm install -g @angular/cli
   ```

2. **Project Setup**
   ```bash
   # Clone and install (use npm, NOT yarn)
   git clone <repo-url>
   cd angular-realworld-example-app
   npm install
   ```

3. **Start Development**
   ```bash
   npm start
   # Application available at http://localhost:4200
   ```

4. **First Feature Exploration**
   - Create test account at http://localhost:4200/register
   - Explore article creation flow: Home → Sign In → New Article
   - Test commenting and favoriting features

5. **Code Exploration**
   - Start with `src/app/app.routes.ts` to understand routing
   - Examine `src/app/core/auth/services/user.service.ts` for authentication
   - Review `src/app/features/article/` for main functionality

**Common Pitfalls**:
- **Don't use yarn** - README is outdated, use npm only
- **Angular CLI required globally** - ng commands won't work without it
- **No environment configuration** - API URL changes require code modification
- **No tests exist** - don't run `npm test` expecting working tests

### 6.2 For QA Engineers

**Testing Areas** (Manual Testing Required):
- **Authentication Flow**: Registration, login, logout, persistence
- **Article Management**: Create, edit, delete, view articles
- **Social Features**: Commenting, favoriting, following users
- **Navigation**: All routes and navigation flows
- **Responsive Design**: Mobile and desktop layouts

**Test Data Creation**:
- Use registration form to create test accounts
- Create articles with various content types and tags
- Test user interactions across different accounts

**Key Testing Focus**:
- Form validation and error handling
- Authentication state persistence
- Route protection for authenticated-only features
- Article pagination and filtering
- Comment management

**Bug Reporting**:
- Include browser and version information
- Note authentication state when reporting issues
- Document steps to reproduce with specific test data

## 7. Troubleshooting

### Common Issues

**Package Manager Conflicts**:
- **Problem**: README mentions yarn but npm lock file exists
- **Solution**: Always use npm commands, ignore yarn references in README

**Angular CLI Not Found**:
- **Problem**: `ng` command not recognized
- **Solution**: Install Angular CLI globally: `npm install -g @angular/cli`

**Build Failures**:
- **Problem**: Production build fails with budget errors
- **Solution**: Check bundle sizes against configured limits in angular.json

**Authentication Issues**:
- **Problem**: User appears logged out after refresh
- **Solution**: Check localStorage for "jwtToken" key, verify token validity

### Environment Issues

**Node Version Mismatch**:
- **Problem**: Application fails to start or build
- **Solution**: Ensure Node.js version matches ^20.11.1 requirement

**Port Conflicts**:
- **Problem**: Development server won't start on port 4200
- **Solution**: Use `ng serve --port <alternative-port>`

### Runtime Errors

**API Connection Issues**:
- **Problem**: API requests failing
- **Solution**: Verify https://api.realworld.io/api is accessible
- **Note**: API URL is hardcoded and not configurable

**Routing Issues**:
- **Problem**: Direct URL access returns 404 in production
- **Solution**: Ensure hosting platform supports SPA routing with `_redirects` file

### Performance Issues

**Slow Initial Load**:
- **Solution**: Check network tab for large bundle sizes, ensure production build used
- **Note**: Bundle size limits configured in angular.json

**Memory Leaks**:
- **Solution**: Check for unsubscribed observables (takeUntilDestroyed used throughout) 