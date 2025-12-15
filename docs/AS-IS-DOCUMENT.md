# AS-IS DOCUMENT: Brownfield Pet Clinic Application

**Document Date:** December 15, 2025  
**Project:** Brownfield Pet Clinic  
**Version:** 0.0.1-SNAPSHOT  
**Repository Location:** `d:\laboratory\sandbox\java_stuff\bmad-petshop\docs\`  
**Source Repository:** `D:\laboratory\sandbox\java_stuff\bmad-exploration\`

---

## Executive Summary

The Brownfield Pet Clinic is a full-stack web application that manages veterinary clinic operations. The system consists of two independently deployable components:
- **Backend**: Spring Boot 3.5.7 REST API (Java 21)
- **Frontend**: React 19 + TypeScript + Vite SPA

The application manages three primary entities: **Pets**, **Vets**, and **Visits**, with CRUD operations and relationship management.

---

## I. Backend Architecture

### A. Technology Stack
| Component | Version | Purpose |
|-----------|---------|---------|
| Spring Boot | 3.5.7 | Application framework |
| Java | 21 | Runtime language |
| Spring Data JPA | (boot) | ORM & database abstraction |
| HSQLDB | (runtime) | In-memory relational database |
| SpringDoc OpenAPI | 2.8.6 | API documentation (Swagger/OpenAPI) |
| Maven | (mvn wrapper) | Build & dependency management |

**Java Version**: 21 (configured in `pom.xml`)

### B. Project Structure
```
brownfield-backend/
├── .mvn/                          # Maven wrapper files
├── src/
│   ├── main/
│   │   ├── java/com/petclinic/
│   │   │   ├── PetclinicApplication.java    # Spring Boot entry point
│   │   │   ├── config/
│   │   │   │   └── CorsConfig.java          # CORS configuration
│   │   │   ├── owner/
│   │   │   │   ├── Owner.java
│   │   │   │   └── OwnerRepository.java
│   │   │   ├── pet/
│   │   │   │   ├── Pet.java                 # Entity
│   │   │   │   ├── PetRepository.java       # Data access
│   │   │   │   ├── PetService.java          # Business logic
│   │   │   │   └── PetController.java       # REST endpoints
│   │   │   ├── vet/
│   │   │   │   ├── Vet.java
│   │   │   │   ├── VetRepository.java
│   │   │   │   ├── VetService.java
│   │   │   │   └── VetController.java
│   │   │   └── visit/
│   │   │       ├── Visit.java
│   │   │       ├── VisitRepository.java
│   │   │       ├── VisitService.java
│   │   │       └── VisitController.java
│   │   └── resources/
│   │       └── application.properties       # Runtime config
│   └── test/
├── pom.xml                        # Maven configuration
├── mvnw / mvnw.cmd               # Maven wrapper executables
├── .gitignore / .gitattributes   # Git configuration

```

### C. Core Entities & Data Model

#### 1. **Pet** Entity
- **Endpoints**: `GET /api/v1/pet`, `GET /api/v1/pet/{id}`, `GET /api/v1/pet/name/{name}`, `POST /api/v1/pet`, `DELETE /api/v1/pet/{id}`
- **Service**: `PetService` – Handles pet CRUD and queries
- **Repository**: `PetRepository` – Spring Data JPA interface for database access

#### 2. **Vet** Entity
- **Endpoints**: Similar CRUD pattern at `/api/v1/vet`
- **Service**: `VetService`
- **Repository**: `VetRepository`

#### 3. **Visit** Entity
- **Endpoints**: Similar CRUD pattern at `/api/v1/visit`
- **Service**: `VisitService`
- **Repository**: `VisitRepository`

#### 4. **Owner** Entity (Secondary)
- **Repository**: `OwnerRepository`
- Note: Controller not yet implemented; likely future feature

### D. API Configuration

**Base Path**: `/api/v1/`

**CORS Policy** (`CorsConfig.java`):
```
- Allowed Origins: http://localhost:5173 (frontend dev server)
- Methods: GET, POST, PUT, DELETE, OPTIONS
- Headers: * (all)
- Credentials: Enabled
```

**API Documentation**: 
- OpenAPI/Swagger UI available via `/swagger-ui.html` (springdoc-openapi-starter-webmvc-ui)

### E. Database Configuration

**Current Setup**:
- **Database**: HSQLDB (in-memory, runtime scope)
- **DDL Auto**: `none` (via `spring.jpa.hibernate.ddl-auto=none`)
- **Implication**: Schema must be pre-created; Hibernate will not auto-generate tables

**Schema Source**: Not explicitly visible in config; likely initialized via:
- SQL scripts in `src/main/resources/db/` (not confirmed in listing)
- Or manual initialization

### F. Build & Deployment

**Build Command**:
```bash
mvn clean install
```

**Run Command**:
```bash
java -jar target/brownfield-backend-0.0.1-SNAPSHOT.jar
```
Or via Maven:
```bash
mvn spring-boot:run
```

**Default Port**: 8080 (Spring Boot default)

### G. Dependencies Summary
- **ORM/Data**: `spring-boot-starter-data-jpa`
- **Web**: `spring-boot-starter-web` (embedded Tomcat)
- **Database**: `hsqldb` (in-memory relational DB)
- **API Docs**: `springdoc-openapi-starter-webmvc-ui` (Swagger/OpenAPI)
- **Testing**: `spring-boot-starter-test`

---

## II. Frontend Architecture

### A. Technology Stack
| Component | Version | Purpose |
|-----------|---------|---------|
| React | 19.1.1 | UI framework |
| TypeScript | ~5.9.2 | Type safety |
| Vite | 7.1.2 | Build tool & dev server |
| Tailwind CSS | 4.1.17 | Utility-first styling |
| Lucide React | 0.554.0 | Icon library |
| Vitest | 2.1.8 | Unit testing framework |
| ESLint | 9.33.0 | Code linting |

### B. Project Structure
```
brownfield-frontend/
├── src/
│   ├── main.tsx                   # React app entry point
│   ├── App.tsx                    # Root component (SPA router state)
│   ├── index.css                  # Global Tailwind styles
│   ├── vite-env.d.ts             # Vite type definitions
│   ├── components/
│   │   ├── Header.tsx             # Navigation header
│   │   └── index.ts               # Component barrel exports
│   ├── pet/
│   │   ├── PetList.tsx            # List view component
│   │   ├── PetDetail.tsx          # Detail view component
│   │   ├── petService.ts          # API client for pets
│   │   └── index.ts               # Type exports
│   ├── vet/
│   │   ├── VetList.tsx
│   │   ├── vetService.ts          # API client for vets
│   │   └── index.ts
│   └── visit/
│       ├── VisitList.tsx
│       ├── VisitDetail.tsx
│       ├── visitService.ts        # API client for visits
│       └── index.ts
├── tests/                         # Unit/integration tests
├── public/
│   └── index.html                 # HTML template
├── package.json                   # Dependencies & scripts
├── vite.config.ts                 # Vite build config
├── vitest.config.ts               # Vitest test config
├── tsconfig.json                  # TypeScript config
├── tsconfig.app.json              # App-specific TypeScript config
├── tsconfig.node.json             # Node script TypeScript config
├── eslint.config.js               # ESLint rules
├── postcss.config.js              # PostCSS (Tailwind) config
├── .gitignore
└── node_modules/                  # Dependencies (generated)

```

### C. Application Architecture

**Routing Model**: Client-side state management (React hooks)
- **App.tsx** manages view state with `useState`
- Views: `petList`, `petDetail`, `visitList`, `visitDetail`, `vetList`
- No external router library (react-router not installed); navigation via state changes

**Component Hierarchy**:
```
App (state manager)
├── Header (navigation controls)
├── PetList (renders all pets)
├── PetDetail (single pet view)
├── VisitList (renders all visits)
├── VisitDetail (single visit view)
└── VetList (renders all vets)
```

### D. API Integration

**Service Pattern**: Separate service files for each domain
- `pet/petService.ts` – HTTP calls for pet endpoints
- `vet/vetService.ts` – HTTP calls for vet endpoints
- `visit/visitService.ts` – HTTP calls for visit endpoints

**Base URL**: `http://localhost:8080/api/v1/` (inferred from CORS config)

**HTTP Client**: Likely using `fetch` API (no axios/apollo visible in package.json)

### E. Styling

**Approach**: Tailwind CSS (utility classes)
- **PostCSS**: Configured via `postcss.config.js` for Tailwind processing
- **Colors/Layout**: Defined in class names (e.g., `bg-gray-50`, `min-h-screen`)

### F. Build & Development

**Development Server**:
```bash
npm run dev
```
Runs Vite dev server on `http://localhost:5173` (default)

**Production Build**:
```bash
npm run build
```
Compiles TypeScript and bundles with Vite

**Testing**:
```bash
npm run test
```
Runs Vitest suite

**Linting**:
```bash
npm run lint
```
Runs ESLint for code quality

### G. Testing Setup

**Framework**: Vitest (Vite-native test runner)
- **Test UI**: `@vitest/ui` (optional interactive test dashboard)
- **DOM Testing**: `@testing-library/react` (component testing utilities)
- **JSDOM**: `jsdom` (DOM emulation for Node.js)

**Test Directory**: `tests/` (convention; can be located elsewhere per vitest.config.ts)

---

## III. Integration Points

### A. Backend-Frontend Communication
1. **Frontend** (localhost:5173) makes HTTP requests to **Backend** (localhost:8080)
2. **CORS Enabled** on backend for origin `http://localhost:5173`
3. **API Format**: RESTful JSON endpoints at `/api/v1/{resource}`
4. **Verbs Supported**: GET, POST, PUT, DELETE

### B. Data Flow Example (Pet Management)
```
User clicks "View Pet" in UI
  ↓
PetList.tsx calls petService.getPetById(id)
  ↓
petService.ts calls fetch('http://localhost:8080/api/v1/pet/{id}')
  ↓
PetController.findById(id) retrieves Pet from database
  ↓
PetService executes business logic (if any)
  ↓
Response returns JSON Pet object to frontend
  ↓
PetDetail.tsx renders pet information
```

### C. Type Sharing
- **Backend**: Java entity classes define data structure
- **Frontend**: TypeScript interface files (likely in `pet/index.ts`, etc.) mirror backend entity structure
- **Note**: No automated code generation (OpenAPI codegen) detected; types manually maintained

---

## IV. Current State Assessment

### A. Implemented Features
✅ **Core CRUD Operations**:
- Pets: List, Detail, Create, Delete
- Vets: List
- Visits: List, Detail

✅ **Navigation**: Client-side view switching via state management

✅ **API Documentation**: OpenAPI/Swagger UI available

✅ **Type Safety**: Full TypeScript support on frontend

✅ **Styling**: Tailwind CSS configured with responsive design

✅ **Testing Framework**: Vitest configured for unit tests

### B. Gaps & Limitations
⚠️ **Owner Entity**: Entity defined but no controller/API exposure; likely incomplete

⚠️ **Routing**: No formal routing library (react-router); state-based navigation limits deep linking and browser history

⚠️ **No API Client Generation**: TypeScript types manually maintained; risk of drift from backend contracts

⚠️ **Database Schema**: DDL set to `none`; schema creation process unclear (likely requires SQL setup scripts)

⚠️ **Authentication/Authorization**: No security configuration visible; all endpoints open to CORS origin

⚠️ **Error Handling**: Service classes may not have comprehensive error handling; unclear in codebase

⚠️ **Pagination**: No pagination visible in list endpoints; could be performance issue with large datasets

⚠️ **Validation**: Client-side and server-side validation strategy not documented

---

## V. Deployment & DevOps

### A. Build Artifacts
- **Backend**: JAR file (`brownfield-backend-0.0.1-SNAPSHOT.jar`) generated in `target/` after Maven build
- **Frontend**: Static assets generated in `dist/` after Vite build

### B. Deployment Targets
- **Backend**: Any Java 21+ runtime with HTTP port exposure
- **Frontend**: Static file server (Nginx, Apache) or CDN; served at root path

### C. Environment Configuration
- **Frontend**: API base URL hardcoded as `http://localhost:8080` (requires refactoring for multi-env support)
- **Backend**: Port defaulted to 8080; Spring properties can override

### D. CI/CD
- No GitHub Actions, GitLab CI, or similar detected in repository root
- No Docker or Kubernetes configuration visible

---

## VI. Code Quality & Testing

### A. Backend Testing
- `spring-boot-starter-test` dependency present
- Test directory structure unclear from listing
- No test count or coverage metrics available

### B. Frontend Testing
- **Vitest** configured with `@testing-library/react`
- Test location: `tests/` directory
- Test coverage tool not configured (could add via vitest coverage mode)

### C. Code Style
- **Java**: Likely follows Spring conventions (no custom checkstyle config detected)
- **TypeScript**: ESLint configured with React plugin; enforces code quality standards

---

## VII. Security Considerations

### A. Current Posture
⚠️ **No Authentication**: No JWT, OAuth, or session management visible
⚠️ **No Authorization**: All API endpoints accessible once CORS passes
⚠️ **CORS**: Limited to localhost:5173; acceptable for development
⚠️ **Database**: In-memory HSQLDB; no persistence between restarts
⚠️ **Input Validation**: Not confirmed in codebase review

### B. Recommendations (For Production)
1. Implement Spring Security with JWT tokens
2. Add input validation on backend (e.g., Bean Validation)
3. Implement role-based access control (RBAC) if multi-user
4. Use persistent database (PostgreSQL, MySQL) instead of HSQLDB
5. Enable HTTPS/TLS in production
6. Add rate limiting and CSRF protection

---

## VIII. Performance & Scalability

### A. Current Limitations
- **In-Memory Database**: No data persistence; all data lost on restart
- **Single JVM Process**: Backend runs as single monolith; no horizontal scaling
- **No Caching**: Frontend and backend make fresh API calls each time
- **No Pagination**: List endpoints may load all records; OK for small datasets

### B. Improvements for Scale
1. Migrate from HSQLDB to persistent database (PostgreSQL)
2. Implement pagination & lazy loading in list views
3. Add Redis caching layer for frequently accessed data
4. Implement API response compression
5. Add async processing for long-running operations

---

## IX. File Inventory

### Backend Key Files
| File | Purpose |
|------|---------|
| `pom.xml` | Maven project configuration, dependencies, build plugins |
| `src/main/java/com/petclinic/PetclinicApplication.java` | Spring Boot entry point |
| `src/main/java/com/petclinic/config/CorsConfig.java` | CORS configuration |
| `src/main/java/com/petclinic/pet/PetController.java` | REST endpoints for pets |
| `src/main/java/com/petclinic/pet/PetService.java` | Business logic for pets |
| `src/main/java/com/petclinic/pet/PetRepository.java` | Data access for pets |
| `src/main/resources/application.properties` | Spring Boot configuration |

### Frontend Key Files
| File | Purpose |
|------|---------|
| `package.json` | Dependencies and build scripts |
| `vite.config.ts` | Vite build configuration |
| `vitest.config.ts` | Vitest test configuration |
| `tsconfig.json` | TypeScript compiler options |
| `src/App.tsx` | Root component; manages view state |
| `src/components/Header.tsx` | Navigation header component |
| `src/pet/petService.ts` | API client for pet endpoints |
| `src/pet/PetList.tsx` | Pet list view component |
| `src/pet/PetDetail.tsx` | Pet detail view component |

---

## X. Getting Started (Development)

### Prerequisites
- **Backend**: Java 21 JDK, Maven (or use mvnw wrapper)
- **Frontend**: Node.js 18+, npm or yarn

### Setup & Execution

#### Backend
```bash
cd brownfield-backend
mvn clean install
mvn spring-boot:run
# API available at http://localhost:8080
# Swagger UI at http://localhost:8080/swagger-ui.html
```

#### Frontend
```bash
cd brownfield-frontend
npm install
npm run dev
# Dev server available at http://localhost:5173
```

#### Access Application
- Open browser to `http://localhost:5173`
- Frontend communicates with backend via `http://localhost:8080/api/v1/`

---

## XI. Next Steps / Recommendations

### Short Term (Fixes & Completeness)
1. **Complete Owner Feature**: Add OwnerController and expose REST endpoints
2. **Formalize Routing**: Consider adding react-router-dom for proper SPA routing
3. **API Type Generation**: Implement OpenAPI code generation to auto-sync TypeScript types
4. **Error Handling**: Add global error boundaries and API error handling
5. **Database Persistence**: Replace HSQLDB with PostgreSQL and initialize schema

### Medium Term (DevOps & Quality)
1. **CI/CD Pipeline**: Add GitHub Actions for automated testing and builds
2. **Containerization**: Add Dockerfile and docker-compose for local dev and deployment
3. **Test Coverage**: Establish minimum coverage thresholds; add integration tests
4. **Environment Config**: Externalize API base URL and other config per environment

### Long Term (Production Readiness)
1. **Authentication/Authorization**: Add Spring Security and JWT-based auth
2. **Monitoring & Logging**: Add structured logging (Spring Cloud Config, ELK)
3. **API Versioning Strategy**: Establish v2 migration path for breaking changes
4. **Performance Optimization**: Add caching, pagination, and async processing
5. **Documentation**: API docs (OpenAPI), architecture decision records (ADRs)

---

## XII. Appendix: Configuration Reference

### Application Properties (Backend)
```properties
spring.application.name=petclinic
spring.jpa.hibernate.ddl-auto=none
```

### Maven Parent (Spring Boot 3.5.7)
- Provides: Spring Boot baseline, dependency management, build plugins

### Package Versions (Frontend)
```json
{
  "react": "^19.1.1",
  "typescript": "~5.9.2",
  "vite": "^7.1.2",
  "tailwindcss": "^4.1.17"
}
```

---

**Document End**

---

**Prepared By**: System Analysis Agent  
**Date**: December 15, 2025  
**Classification**: Internal / Development Reference  
**Document Location**: `bmad-petshop/docs/AS-IS-DOCUMENT.md`
