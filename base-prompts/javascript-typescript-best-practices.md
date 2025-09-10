# JavaScript/TypeScript Best Practices Context Engineering Base Prompt

*"Enterprise-grade JavaScript/TypeScript development through comprehensive context engineering and battle-tested conventions."*

---

## 🧠 CORE PHILOSOPHY

### The JavaScript/TypeScript Excellence Paradigm
**Type Safety > Consistency > Flexibility**

- **Type Safety First**: Leverage TypeScript's type system to catch errors at compile time
- **Consistency Over Cleverness**: Prefer readable, maintainable code over clever one-liners
- **Modern Standards**: Use ES2020+ features and avoid legacy patterns
- **Performance Conscious**: Write efficient code without premature optimization

### Fundamental Principles
```
Quality = TypeSafety(Consistency(ModernFeatures(Performance)))
```

### Core Tenets
1. **Explicit Over Implicit**: "Make interfaces obvious, dependencies clear, and intentions visible"
2. **Fail Fast**: Use strict TypeScript settings and comprehensive validation
3. **Composition Over Inheritance**: Prefer function composition and interfaces over class hierarchies
4. **Pure Functions**: Minimize side effects and embrace functional programming principles

---

## 🏗️ JAVASCRIPT/TYPESCRIPT PROJECT FRAMEWORK

### 1. PROJECT GENESIS
```markdown
# JavaScript/TypeScript Project Overview
**Purpose**: [Clear description of application domain and primary functionality]
**Runtime Environment**: [Node.js, Browser, Deno, Bun, Universal]
**Core Value**: [Business problem solved or user benefit provided]
**Key Stakeholders**: [End users, developers, system integrators]
```

### 2. TECHNICAL ECOSYSTEM
```markdown
## Tech Stack & Architecture
**Language**: TypeScript 5.0+ with strict mode enabled
**Runtime**: [Node.js LTS, Deno, Browser ES2020+]
**Framework**: [React, Vue, Angular, Express, Fastify, Next.js]
**State Management**: [Redux Toolkit, Zustand, Pinia, Recoil]
**Build System**: [Vite, Webpack, Rollup, esbuild, tsc]
**Package Manager**: [npm, yarn, pnpm]
**Testing**: [Jest, Vitest, Cypress, Playwright]
**Code Quality**: [ESLint, Prettier, TypeScript strict mode]
```

### 3. ARCHITECTURAL BLUEPRINT
```markdown
## Project Structure (Domain-Driven Design)
```
project-root/
├── src/
│   ├── domain/          # Core business logic (entities, value objects)
│   ├── application/     # Use cases, services, commands/queries
│   ├── infrastructure/  # External concerns (databases, APIs, frameworks)
│   ├── presentation/    # UI components, controllers, adapters
│   └── shared/          # Common utilities, types, constants
├── tests/
│   ├── unit/           # Isolated component tests
│   ├── integration/    # Cross-component tests
│   └── e2e/           # End-to-end user flows
├── docs/              # Technical documentation
└── config/            # Build and environment configuration
```

**Domain Layer Principles**:
- Pure TypeScript with no external dependencies
- Business logic encapsulated in entities and value objects
- Domain events for cross-boundary communication

**Application Layer**:
- Orchestrates domain objects
- Handles use cases and business workflows
- Defines ports/interfaces for external dependencies

**Infrastructure Layer**:
- Implements application layer interfaces
- Handles external API calls, database operations
- Framework-specific implementations
```

---

## 💎 LANGUAGE STANDARDS MATRIX

### 1. MODERN JAVASCRIPT/TYPESCRIPT CONVENTIONS

#### Variable Declaration
```typescript
// ✅ Preferred
const API_BASE_URL = 'https://api.example.com';
const userConfig = { theme: 'dark', locale: 'en' };
let mutableCounter = 0;

// ❌ Avoid
var globalVariable = 'dangerous';
const user_name = 'snake_case_bad';
```

#### Type Definitions
```typescript
// ✅ Explicit interfaces
interface UserProfile {
  readonly id: string;
  email: string;
  preferences: {
    theme: 'light' | 'dark';
    notifications: boolean;
  };
}

// ✅ Utility types
type UserUpdateRequest = Partial<Pick<UserProfile, 'email' | 'preferences'>>;
type UserResponse = UserProfile & { createdAt: Date };

// ✅ Generic constraints
interface Repository<T extends { id: string }> {
  findById(id: string): Promise<T | null>;
  save(entity: T): Promise<T>;
}
```

#### Modern ES Features
```typescript
// ✅ Destructuring with defaults
const { theme = 'light', locale = 'en' } = userConfig;

// ✅ Optional chaining and nullish coalescing
const userName = user?.profile?.name ?? 'Anonymous';

// ✅ Array methods over loops
const activeUsers = users.filter(user => user.isActive);
const userNames = users.map(user => user.name);

// ✅ Async/await over promises
const fetchUser = async (id: string): Promise<UserProfile> => {
  try {
    const response = await api.get(`/users/${id}`);
    return response.data;
  } catch (error) {
    throw new UserNotFoundError(`User ${id} not found`);
  }
};
```

### 2. ARCHITECTURAL PATTERNS

#### Domain-Driven Design
```typescript
// Domain Entity
class User {
  private constructor(
    private readonly _id: UserId,
    private _email: Email,
    private _profile: UserProfile
  ) {}

  static create(email: string, profileData: ProfileData): User {
    const userId = UserId.generate();
    const userEmail = Email.fromString(email);
    const profile = UserProfile.create(profileData);
    
    return new User(userId, userEmail, profile);
  }

  get id(): string { return this._id.value; }
  get email(): string { return this._email.value; }
  
  updateEmail(newEmail: string): void {
    const email = Email.fromString(newEmail);
    this._email = email;
    // Emit domain event
    DomainEvents.raise(new UserEmailUpdated(this._id, email));
  }
}

// Value Object
class Email {
  private constructor(private readonly _value: string) {}
  
  static fromString(email: string): Email {
    if (!this.isValid(email)) {
      throw new InvalidEmailError(`Invalid email: ${email}`);
    }
    return new Email(email.toLowerCase());
  }
  
  get value(): string { return this._value; }
  
  private static isValid(email: string): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
}
```

#### Hexagonal Architecture (Ports & Adapters)
```typescript
// Port (Interface)
interface UserRepository {
  findById(id: string): Promise<User | null>;
  save(user: User): Promise<void>;
  findByEmail(email: string): Promise<User | null>;
}

// Application Service
class UserService {
  constructor(private userRepository: UserRepository) {}
  
  async updateUserEmail(userId: string, newEmail: string): Promise<void> {
    const user = await this.userRepository.findById(userId);
    if (!user) {
      throw new UserNotFoundError(`User ${userId} not found`);
    }
    
    const existingUser = await this.userRepository.findByEmail(newEmail);
    if (existingUser && existingUser.id !== userId) {
      throw new EmailAlreadyInUseError(`Email ${newEmail} is already in use`);
    }
    
    user.updateEmail(newEmail);
    await this.userRepository.save(user);
  }
}

// Adapter (Implementation)
class DatabaseUserRepository implements UserRepository {
  constructor(private db: Database) {}
  
  async findById(id: string): Promise<User | null> {
    const userData = await this.db.query('SELECT * FROM users WHERE id = ?', [id]);
    return userData ? User.fromData(userData) : null;
  }
  
  async save(user: User): Promise<void> {
    await this.db.query(
      'UPDATE users SET email = ?, profile = ? WHERE id = ?',
      [user.email, JSON.stringify(user.profile), user.id]
    );
  }
}
```

### 3. ERROR HANDLING STANDARDS

#### Custom Error Classes
```typescript
abstract class DomainError extends Error {
  abstract readonly code: string;
  
  constructor(message: string, public readonly context?: Record<string, any>) {
    super(message);
    this.name = this.constructor.name;
  }
}

class UserNotFoundError extends DomainError {
  readonly code = 'USER_NOT_FOUND';
}

class InvalidEmailError extends DomainError {
  readonly code = 'INVALID_EMAIL';
}
```

#### Result Pattern
```typescript
type Result<T, E = Error> = {
  success: true;
  data: T;
} | {
  success: false;
  error: E;
};

const parseUser = (data: unknown): Result<User, ValidationError> => {
  try {
    const user = User.fromData(data);
    return { success: true, data: user };
  } catch (error) {
    return { success: false, error: error as ValidationError };
  }
};
```

---

## 🧪 TESTING EXCELLENCE FRAMEWORK

### 1. TEST STRATEGY PYRAMID

#### Integration/Component Tests (Primary Focus)
```typescript
// ✅ Test entire features through their API
describe('User Registration Feature', () => {
  let app: Application;
  let database: TestDatabase;
  
  beforeEach(async () => {
    database = await TestDatabase.create();
    app = createTestApp({ database });
  });
  
  afterEach(async () => {
    await database.cleanup();
  });
  
  test('should register user with valid email', async () => {
    const response = await request(app)
      .post('/users/register')
      .send({ email: 'user@example.com', password: 'secure123' });
    
    expect(response.status).toBe(201);
    expect(response.body.user.email).toBe('user@example.com');
    
    // Verify database state
    const savedUser = await database.findUserByEmail('user@example.com');
    expect(savedUser).toBeDefined();
  });
});
```

#### Unit Tests (For Complex Logic)
```typescript
// ✅ Test pure functions and domain logic
describe('Email Value Object', () => {
  test('should create valid email', () => {
    const email = Email.fromString('user@example.com');
    expect(email.value).toBe('user@example.com');
  });
  
  test('should reject invalid email format', () => {
    expect(() => Email.fromString('invalid-email'))
      .toThrow(InvalidEmailError);
  });
  
  test('should normalize email to lowercase', () => {
    const email = Email.fromString('USER@EXAMPLE.COM');
    expect(email.value).toBe('user@example.com');
  });
});
```

#### End-to-End Tests (Critical User Journeys)
```typescript
// ✅ Test complete user workflows
describe('User Registration Journey', () => {
  test('user can register and login successfully', async () => {
    await page.goto('/register');
    
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'secure123');
    await page.click('[data-testid="register-button"]');
    
    await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
    
    // Navigate to login
    await page.goto('/login');
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'secure123');
    await page.click('[data-testid="login-button"]');
    
    await expect(page.locator('[data-testid="dashboard"]')).toBeVisible();
  });
});
```

### 2. TEST INFRASTRUCTURE

#### Test Database Setup
```typescript
export class TestDatabase {
  private constructor(private connection: DatabaseConnection) {}
  
  static async create(): Promise<TestDatabase> {
    const connection = await createDatabaseConnection({
      host: 'localhost',
      database: `test_${Date.now()}_${Math.random()}`,
      migrations: true
    });
    
    return new TestDatabase(connection);
  }
  
  async cleanup(): Promise<void> {
    await this.connection.dropAllTables();
    await this.connection.close();
  }
}
```

#### Mocking Strategy
```typescript
// ✅ Mock external dependencies, not domain logic
const mockEmailService = {
  sendWelcomeEmail: jest.fn().mockResolvedValue(undefined),
  sendPasswordReset: jest.fn().mockResolvedValue(undefined)
} satisfies EmailService;

// ✅ Use type-safe mocks
const mockUserRepository: jest.Mocked<UserRepository> = {
  findById: jest.fn(),
  save: jest.fn(),
  findByEmail: jest.fn()
};
```

---

## ⚙️ DEVELOPMENT WORKFLOW

### 1. CODE QUALITY GATES

#### TypeScript Configuration
```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "exactOptionalPropertyTypes": true
  }
}
```

#### ESLint Rules
```json
{
  "extends": [
    "@typescript-eslint/recommended",
    "@typescript-eslint/recommended-requiring-type-checking"
  ],
  "rules": {
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/prefer-readonly": "error",
    "@typescript-eslint/explicit-function-return-type": "error"
  }
}
```

### 2. PERFORMANCE STANDARDS

#### Bundle Analysis
- Bundle size targets: < 200KB for libraries, < 1MB for applications
- Tree-shaking enabled for all dependencies
- Code splitting at route and feature boundaries

#### Runtime Performance
- Use `performance.mark()` and `performance.measure()` for profiling
- Implement lazy loading for non-critical components
- Prefer `const` assertions for literal types to reduce runtime overhead

---

## 🔧 AVAILABLE TOOLS & COMMANDS

### Development Commands
```bash
# Development server with hot reload
npm run dev

# Type checking (no emit)
npm run type-check

# Linting and formatting
npm run lint
npm run lint:fix
npm run format

# Testing
npm run test
npm run test:watch
npm run test:coverage
npm run test:e2e

# Building
npm run build
npm run build:analyze
```

### Quality Assurance
```bash
# Pre-commit hooks
npm run pre-commit  # Runs lint, type-check, and tests

# Build validation
npm run validate    # Full build and test suite

# Security audit
npm audit --audit-level high
```

---

## 🚨 VALIDATION GATES

### Pre-Implementation Checklist
- [ ] **Requirements Clear**: Business logic and acceptance criteria understood
- [ ] **Types Defined**: All interfaces and types explicitly declared
- [ ] **Architecture Aligned**: Solution follows established patterns
- [ ] **Dependencies Minimal**: No unnecessary external dependencies

### Implementation Standards
- [ ] **Type Safety**: No `any` types, strict mode enabled
- [ ] **Error Handling**: All error paths handled explicitly
- [ ] **Testing**: Integration tests for features, unit tests for complex logic
- [ ] **Performance**: No performance regressions, bundle size acceptable

### Post-Implementation Checklist
- [ ] **Code Review**: Peer review completed with architectural validation
- [ ] **Tests Passing**: All test suites green, coverage targets met
- [ ] **Documentation**: README updated, API documentation current
- [ ] **Build Success**: Production build completes without warnings

---

## 📊 SUCCESS METRICS

### Code Quality Indicators
- **Type Coverage**: > 95% of code covered by TypeScript types
- **Test Coverage**: > 80% line coverage, 100% branch coverage for critical paths
- **Build Performance**: < 30 seconds for development builds
- **Bundle Size**: Within defined limits for production builds
- **Zero Runtime Errors**: No uncaught exceptions in production

### Development Velocity
- **Feature Delivery**: Consistent sprint velocity with quality gates
- **Defect Rate**: < 5% of features require post-release fixes
- **Developer Experience**: Fast feedback loops, minimal context switching

---

## 🎯 FRAMEWORK-SPECIFIC EXTENSIONS

### React/Next.js Context
```markdown
## React Development Standards
**Component Patterns**: Functional components with hooks
**State Management**: React Query for server state, Zustand for client state
**Styling**: Tailwind CSS with component variants
**Routing**: Next.js App Router with type-safe routes
**Forms**: React Hook Form with Zod validation
**Testing**: Testing Library with user-centric queries
```

### Node.js/Express Context
```markdown
## Backend Development Standards
**Framework**: Express.js with TypeScript decorators or Fastify
**Validation**: Zod for runtime type validation
**Database**: Prisma ORM with PostgreSQL
**Authentication**: JWT with refresh token rotation
**API Design**: OpenAPI specification with auto-generated types
**Monitoring**: Structured logging with correlation IDs
```

### Vue/Nuxt Context
```markdown
## Vue Development Standards
**Composition API**: `<script setup>` syntax with TypeScript
**State Management**: Pinia with type-safe stores
**UI Components**: Headless UI with custom styling
**Forms**: VeeValidate with Yup/Zod schemas
**Testing**: Vue Testing Utils with component testing focus
```

---

## 🚀 ADVANCED PATTERNS

### 1. Dependency Injection
```typescript
// IoC Container setup
interface Container {
  get<T>(token: Token<T>): T;
  register<T>(token: Token<T>, factory: () => T): void;
}

const TOKENS = {
  UserRepository: Symbol('UserRepository'),
  EmailService: Symbol('EmailService'),
  UserService: Symbol('UserService')
} as const;

// Service registration
container.register(TOKENS.UserRepository, () => new DatabaseUserRepository(db));
container.register(TOKENS.EmailService, () => new SMTPEmailService(config));
container.register(TOKENS.UserService, () => 
  new UserService(
    container.get(TOKENS.UserRepository),
    container.get(TOKENS.EmailService)
  )
);
```

### 2. Event-Driven Architecture
```typescript
interface DomainEvent {
  readonly type: string;
  readonly aggregateId: string;
  readonly occurredAt: Date;
}

class UserEmailUpdated implements DomainEvent {
  readonly type = 'UserEmailUpdated';
  
  constructor(
    readonly aggregateId: string,
    readonly newEmail: string,
    readonly occurredAt = new Date()
  ) {}
}

// Event Bus
interface EventBus {
  publish(event: DomainEvent): Promise<void>;
  subscribe<T extends DomainEvent>(
    eventType: string, 
    handler: (event: T) => Promise<void>
  ): void;
}
```

### 3. Functional Programming Utilities
```typescript
// Monadic error handling
const pipe = <T>(...fns: Array<(arg: T) => T>) => (value: T): T =>
  fns.reduce((acc, fn) => fn(acc), value);

const compose = <T>(...fns: Array<(arg: T) => T>) => (value: T): T =>
  fns.reduceRight((acc, fn) => fn(acc), value);

// Maybe monad for null handling
class Maybe<T> {
  private constructor(private value: T | null) {}
  
  static of<T>(value: T | null): Maybe<T> {
    return new Maybe(value);
  }
  
  map<U>(fn: (value: T) => U): Maybe<U> {
    return this.value === null ? Maybe.of(null) : Maybe.of(fn(this.value));
  }
  
  flatMap<U>(fn: (value: T) => Maybe<U>): Maybe<U> {
    return this.value === null ? Maybe.of(null) : fn(this.value);
  }
}
```

---

*"Excellence in JavaScript/TypeScript development comes from disciplined application of proven patterns, comprehensive type safety, and relentless focus on maintainability."*

---

**Version**: 1.0  
**Last Updated**: 2025-09-10  
**Compatibility**: TypeScript 5.0+, Node.js 18+, Modern Browsers (ES2020+)  
**Maintenance**: Review and update quarterly with ecosystem changes
**References**: [JavaScript/TypeScript Best Practices Links](/research/javascript-typescript-convensions-links.md)