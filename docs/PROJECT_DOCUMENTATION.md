# Angular RealWorld Project Documentation

## 1. Project Overview

**Purpose**: Conduit is a social blogging platform (Medium.com clone) that enables users to publish articles, engage with content through comments and favorites, and follow other writers.

**Domain**: Social media and content publishing platform serving writers, readers, and content creators.

**Target Users**:

- **Writers**: Create, edit, and publish articles with markdown support
- **Readers**: Browse, read, and engage with articles through comments and favorites
- **Content Curators**: Follow favorite authors and manage personalized feeds

**Core Value Proposition**: Provides a simplified, modern blogging experience with social features that connects writers and readers through quality content discovery and community interaction.

## 2. Functional Documentation

### 2.1 Feature Inventory

#### Authentication & User Management

- **User Registration**: Any user can create an account with email, username, and password
- **User Login**: Registered users can authenticate via email/password
- **User Settings**: Authenticated users can update profile (image, bio, username, email, password)
- **User Logout**: Clear session and redirect to home page

#### Article Management

- **Article Creation**: Authenticated users can create new articles with title, description, body (markdown), and tags
- **Article Editing**: Article authors can modify their published articles
- **Article Deletion**: Article authors can permanently remove their articles
- **Article Viewing**: Any user can read published articles with rendered markdown
- **Article Favoriting**: Authenticated users can favorite/unfavorite articles
- **Article Listing**: Browse articles via global feed, personal feed, or filtered by tags/authors

#### Social Features

- **User Profiles**: View author profiles with bio, image, and article listings
- **Follow System**: Authenticated users can follow/unfollow other users
- **Comment System**: Add, view, and delete comments on articles (authenticated users only)
- **Personal Feed**: Authenticated users see articles from followed authors
- **Tag System**: Browse articles by topic tags

### 2.2 User Journey Maps

#### Authentication Flow

1. **Guest User**: Access home page → View global feed → Click "Sign up" → Complete registration → Redirected to home with authenticated state
2. **Returning User**: Access home page → Click "Sign in" → Enter credentials → Redirected to home with personal feed access

#### Core Content Workflows

1. **Article Creation**: Click "New Article" → Fill form (title, description, body, tags) → Click "Publish Article" → Redirected to article view
2. **Article Engagement**: Browse articles → Click article → Read content → Add comments → Favorite article → Visit author profile
3. **Profile Management**: Click "Settings" → Update profile information → Save changes → Redirected to profile page

### 2.3 Business Logic Rules

#### Data Validation Rules

- **Email**: Required field for registration and login (basic format validation)
- **Username**: Required, unique identifier for user profiles
- **Password**: Required for registration and login
- **Article Title**: Required field for article creation
- **Article Description**: Required field for article creation
- **Article Body**: Required field for article creation

#### Business Constraints

- **Article Ownership**: Only article authors can edit or delete their articles
- **Comment Ownership**: Only comment authors can delete their comments
- **Authentication Required**: Creating articles, commenting, favoriting, and following require authentication
- **Unique Usernames**: Each user must have a unique username for profile URLs

#### Workflow Dependencies

- **Profile Access**: Requires user authentication to view settings
- **Personal Feed**: Requires following other users to populate content
- **Article Editing**: Requires matching author username to enable edit/delete options

## 3. Technical Documentation

### 3.1 Architecture Overview

#### Technology Stack

- **Angular**: 20.0.0 (latest version using standalone components)
- **TypeScript**: 5.8.3
- **RxJS**: 7.4.0 for reactive programming
- **Marked**: 11.1.0 for markdown rendering
- **Node.js**: ^20.11.1 (required runtime)
- **@rx-angular/template**: 18.0.0 for reactive template utilities

#### Project Structure

```
src/
├── app/
│   ├── core/                 # Core application functionality
│   │   ├── auth/            # Authentication services and components
│   │   ├── interceptors/    # HTTP interceptors
│   │   ├── layout/          # Header and footer components
│   │   └── models/          # Shared data models
│   ├── features/            # Feature-specific modules
│   │   ├── article/         # Article management
│   │   ├── profile/         # User profile management
│   │   └── settings/        # User settings
│   └── shared/              # Shared components and utilities
```

#### Design Patterns

- **Standalone Components**: Uses Angular 20's standalone component architecture
- **Service-Oriented Architecture**: Separated concerns with dedicated services for API calls
- **Reactive Programming**: RxJS Observables for data flow and state management
- **Lazy Loading**: Route-based code splitting for performance optimization
- **Dependency Injection**: Angular's DI system for service management

#### Data Flow

1. **User Interactions** → Components
2. **Components** → Services (API calls)
3. **Services** → HTTP Interceptors → External API
4. **API Responses** → Services → Components
5. **Components** → Templates (UI Updates)

#### Component Architecture

- **Standalone Components**: All components are self-contained with explicit imports
- **Feature-Based Organization**: Components grouped by domain (articles, profiles, auth)
- **Shared Components**: Reusable UI elements in shared directory
- **Lazy Loading**: Route-level code splitting for optimal performance

### 3.2 Development Environment

#### Prerequisites

- **Node.js**: ^20.11.1
- **npm**: Latest version (package manager)
- **Angular CLI**: ^20.0.0 (for development commands)

#### Installation Steps

```bash
# Clone the repository
git clone https://github.com/jiteshy/angular-realworld-example-app.git
cd angular-realworld-example-app

# Install dependencies
npm install

# Start development server
npm start
```

#### Configuration

**Note**: This application uses hardcoded configuration. No environment files exist.

#### Common Commands

- **Development**: `npm start` - Start development server on http://localhost:4200
- **Build**: `npm run build` - Build for production
- **Lint**: `npm run lint` - Run linting checks
- **Test**: Testing infrastructure is configured but no tests exist

### 3.3 Application Configuration

#### Required Configuration

**Database Connection**: Does not exist - application uses external API
**External Services**: API endpoint is hardcoded to `https://api.realworld.io/api`
**Security Settings**: No configuration required - JWT tokens managed automatically
**Environment Variables**: Does not exist - no environment configuration files

#### Configuration Sources

**Environment Variables**: Not used - no environment variable reading implemented
**Configuration Files**: Do not exist - no .env, environment.ts, or config files
**Runtime Configuration**: Hardcoded API base URL in `apiInterceptor`

#### Configuration Validation

**Startup Validation**: No configuration validation exists
**Error Messages**: Standard HTTP error handling for API failures
**Configuration Testing**: No configuration testing implemented

### 3.4 Code Organization

#### Directory Structure

- **`src/app/core/`**: Authentication, interceptors, layout, and shared models
- **`src/app/features/`**: Domain-specific features (articles, profiles, settings)
- **`src/app/shared/`**: Reusable components and utilities
- **`src/assets/`**: Static assets and images

#### File Naming Conventions

- **Components**: `*.component.ts` (e.g., `home.component.ts`)
- **Services**: `*.service.ts` (e.g., `articles.service.ts`)
- **Models**: `*.model.ts` (e.g., `user.model.ts`)
- **Pipes**: `*.pipe.ts` (e.g., `markdown.pipe.ts`)
- **Directives**: `*.directive.ts` (e.g., `if-authenticated.directive.ts`)

#### Import/Export Patterns

- **Standalone Components**: Each component explicitly imports its dependencies
- **Service Injection**: Uses Angular's `inject()` function for dependency injection
- **Lazy Loading**: Route-level imports using dynamic `import()` statements
- **Barrel Exports**: Feature routes exported from index files

### 3.5 API Documentation

#### External APIs

**RealWorld API**: `https://api.realworld.io/api` - Backend service for all application data

#### API Endpoints Inventory

**Authentication Endpoints**:

- `POST /users/login` - User login with email/password
- `POST /users` - User registration
- `GET /user` - Get current user profile
- `PUT /user` - Update current user profile

**Article Endpoints**:

- `GET /articles` - Get global articles list with pagination
- `GET /articles/feed` - Get personal feed articles (authenticated)
- `GET /articles/:slug` - Get single article by slug
- `POST /articles` - Create new article (authenticated)
- `PUT /articles/:slug` - Update article (authenticated, author only)
- `DELETE /articles/:slug` - Delete article (authenticated, author only)
- `POST /articles/:slug/favorite` - Favorite article (authenticated)
- `DELETE /articles/:slug/favorite` - Unfavorite article (authenticated)

**Comment Endpoints**:

- `GET /articles/:slug/comments` - Get article comments
- `POST /articles/:slug/comments` - Add comment (authenticated)
- `DELETE /articles/:slug/comments/:id` - Delete comment (authenticated, author only)

**Profile Endpoints**:

- `GET /profiles/:username` - Get user profile
- `POST /profiles/:username/follow` - Follow user (authenticated)
- `DELETE /profiles/:username/follow` - Unfollow user (authenticated)

**Tag Endpoints**:

- `GET /tags` - Get all tags

#### API Service Architecture

**Service Files**:

- `ArticlesService`: Handles all article-related API calls
- `CommentsService`: Manages comment operations
- `UserService`: Authentication and user management
- `ProfileService`: User profile operations
- `TagsService`: Tag data retrieval

**HTTP Client Configuration**:

- `apiInterceptor`: Adds base URL `https://api.realworld.io/api` to all requests
- `tokenInterceptor`: Adds JWT token to authenticated requests
- `errorInterceptor`: Handles and formats API errors

#### Authentication Flow

**Method**: JWT (JSON Web Token) storage in localStorage
**Token Storage**: `localStorage['jwtToken']`
**Token Transmission**: `Authorization: Token {jwt}` header
**Session Management**: Automatic token inclusion in authenticated requests
**Protected Routes**: Route guards check authentication status

#### API Integration Patterns

**Async Operations**: RxJS Observables for all HTTP operations
**Loading States**: LoadingState enum (NOT_LOADED, LOADING, LOADED)
**Error Handling**: Centralized error interceptor with observable error streams
**Data Persistence**: No caching - fresh API calls for each request

### 3.6 Data Management

#### State Management Architecture

**State Library**: No external state management library used
**Global State**: Simple RxJS BehaviorSubject pattern in UserService
**Local State**: Component-level state management with reactive forms
**State Initialization**: User authentication state loaded on app initialization

#### Store Documentation

**UserService State**:

- **Responsibility**: Manages current user authentication and profile data
- **State Shape**: `BehaviorSubject<User | null>`
- **Methods**: `login()`, `register()`, `logout()`, `getCurrentUser()`, `update()`
- **Selectors**: `currentUser`, `isAuthenticated` observables

**Article State**:

- **Responsibility**: Manages article data and lists
- **State Pattern**: Service-based with direct API calls
- **Methods**: `query()`, `get()`, `create()`, `update()`, `delete()`, `favorite()`

#### Data Flow Patterns

**Component to Service**: Direct service method calls for API operations
**Service to Component**: Observable subscriptions for reactive updates
**Side Effects**: Navigation handled in components after successful operations
**State Updates**: Immutable updates through service method responses

#### Data Models

**Core Entity Interfaces**:

- `User`: email, token, username, bio, image
- `Article`: slug, title, description, body, tagList, createdAt, updatedAt, favorited, favoritesCount, author
- `Comment`: id, body, createdAt, updatedAt, author
- `Profile`: username, bio, image, following
- `Errors`: errors object for form validation

#### Local Storage & Persistence

**Persisted Data**: JWT token only (`localStorage['jwtToken']`)
**Persistence Strategy**: Manual token save/remove in JwtService
**Data Lifecycle**: Token persists until logout or manual removal
**Cache Strategy**: No caching - all data fetched fresh from API

### 4. Development Guidelines

#### Code Standards

- **TypeScript**: Strict type checking enabled
- **Linting**: ESLint with Angular recommended rules
- **Formatting**: Prettier with 2-space indentation
- **File Naming**: Kebab-case for all files and components

#### Testing Strategy

**Testing**: No test files found in the codebase. Testing framework is configured in `tsconfig.spec.json` but no tests are implemented.

#### Performance Considerations

- **Lazy Loading**: Route-level code splitting
- **OnPush Change Detection**: Used in some components
- **Reactive Forms**: For optimal form performance
- **Standalone Components**: Reduced bundle size

### 5. Quick Start Guide

#### For New Developers

1. **Setup**: Install Node.js 20.11.1 and npm
2. **Clone**: `git clone https://github.com/jiteshy/angular-realworld-example-app.git`
3. **Install**: `npm install`
4. **Run**: `npm start`
5. **First Feature**: Try adding a new article component or service method

#### For QA Engineers

- **No Test Automation**: No automated tests exist
- **Manual Testing**: Focus on user authentication flows and article operations
- **API Testing**: All endpoints connect to external RealWorld API
- **Key Test Areas**: Registration, login, article creation, commenting, favoriting

### 6. Troubleshooting

#### Common Issues

- **Node Version**: Ensure Node.js ^20.11.1 is installed
- **Dependencies**: Run `npm install` if modules are missing
- **API Connectivity**: Application depends on external API availability
- **Authentication**: Clear localStorage if experiencing auth issues

#### Known Limitations

- **No Environment Configuration**: API URL is hardcoded
- **No Test Coverage**: Testing infrastructure exists but no tests implemented
- **No Error Boundaries**: Limited error handling beyond HTTP interceptors
- **No Offline Support**: Application requires internet connectivity

#### Critical Known Issues

- **Article Editor Bug**: The editor component always calls `create()` method instead of `update()` when editing existing articles (`src/app/features/article/pages/editor/editor.component.ts:82-88`). This results in duplicate articles being created instead of updating existing ones.
- **Form Validation Inconsistency**: Auth forms use `type="text"` for email fields while settings form uses `type="email"`, creating inconsistent user experience and validation behavior.
