# Ordu Bilişim Topluluğu (Dashboard)

Skill: Deno/Node.js Ecosystem & Runtime Selection Evidence: Backend built on Deno 2.x (not Node) using Deno.serve(), JSR imports (@yelix/hono), and npm compatibility layer. Frontend uses Astro with Node.js adapter in standalone mode. Depth: Advanced — Consciously chose Deno for type safety and modern runtime features while maintaining npm interoperability. Uses JSR (Deno's registry) for specialized packages and npm for mainstream libraries.

Skill: PostgreSQL Database Operations & Schema Design Evidence: Native pg client (not ORM), hand-written migrations with numbered sequential files (0.ts-10.ts), proper indexing strategies (CREATE INDEX), foreign key constraints with ON DELETE CASCADE, and JSONB columns for flexible env vars. Implements connection retry logic with exponential backoff in db.ts. Depth: Advanced — Operates database directly without ORM abstraction; understands normalization, indexing for query performance, and transaction safety.

Skill: Containerization & Production Deployment Evidence: Multi-stage Dockerfile for frontend (builder + production stages with Alpine Linux), Deno official image for backend with dependency caching layer. Exposes specific ports, sets production environment flags. Depth: Intermediate — Understands Docker layer caching, image size optimization (Alpine), and separation of build/runtime environments. Likely self-hosts or uses container orchestration.

Skill: Web Push Notifications (VAPID Protocol) Evidence: Implements web-push library with VAPID key configuration, handles subscription lifecycle (404/410 cleanup for expired endpoints), connection reset error handling, and notification preference management per-user per-location. Turkish locale-specific content in notifications. Depth: Advanced — Operates Web Push infrastructure end-to-end; understands VAPID key generation, subscription persistence, and edge case handling (expired subscriptions, connection resets).

Skill: Sandboxed Code Execution & Workflow Engine Evidence: Custom workflow executor using Blob + URL.createObjectURL + dynamic import() to run user-submitted JavaScript in isolated context with timeout (5min), resource injection (db, fetch, env), and execution tracking. Prevents JSON injection attacks via validation. Depth: Advanced — Implements secure-ish sandboxing without external libraries; understands JavaScript module loading, memory leaks (URL.revokeObjectURL), and execution timeouts.

Skill: Cron Scheduling & Job Queue Patterns Evidence: Custom cron parser implementation supporting ranges (1-5), steps (*/5), lists (1,2,3), and day-of-week logic. Deduplication via runningWorkflows Set. Scheduler runs as background loop in main HTTP process. Depth: Intermediate — Built custom scheduling rather than using Bull/Agenda; understands cron syntax parsing and concurrency control.

Skill: API Design & Documentation Evidence: Uses Hono with OpenAPI spec generation, Yelix for type-safe validation with Zod, Bearer JWT security schema, and Scalar API reference UI at /docs. Proper route modularization per-domain. Depth: Intermediate — Follows OpenAPI standards, implements auth middleware consistently, structures APIs hierarchically.

Skill: String Similarity Algorithms & Internationalization Evidence: Implements Levenshtein distance algorithm for fuzzy meal name matching, Turkish locale-aware normalization (toLocaleLowerCase('tr')), and configurable similarity thresholds (95%). Depth: Intermediate — Understands algorithmic string comparison, Unicode locale handling for Turkish-specific characters (i, ı).

Skill: Caching Strategy Implementation Evidence: Custom in-memory cache with TTL, wildcard pattern invalidation (regex-based), cache-aside pattern via wrap() method, and cache stats for observability. Depth: Intermediate — Built cache manager from scratch rather than using Redis/memcached; understands cache invalidation patterns.

Skill: Environment Configuration Management Evidence: Strict env variable validation at boot time with explicit allowlist (_env array), clear error messages for missing vars, type-safe ENV export. Separate env vars for PG, VAPID, and server config. Depth: Intermediate — Enforces configuration completeness at startup; understands 12-factor app principles.

Skill: TypeScript & Type Safety Evidence: Strict typing throughout (QueryResultRow generics, typed interfaces for WorkflowContext, explicit return types). Uses Zod for runtime validation. No any abuse except minimal escape hatches. Depth: Intermediate — Leverages TypeScript for both compile-time and runtime (Zod) safety; uses generic types for reusable query patterns.

# Dijital Staj

Skill Analysis Summary
This is a production-grade internship management system ("Staj İşleri" = Internship Affairs) for Ordu University, Turkey. The codebase reveals significant real-world engineering expertise beyond typical tutorial-level implementations.

Skill: Full-Stack TypeScript Architecture
Evidence:

Frontend: Next.js 16 + React 19 + TypeScript with Tailwind v4
Backend: Deno 2.x (intentionally choosing modern runtime over Node.js default)
Database: PostgreSQL with Drizzle ORM (type-safe SQL-like queries)
Custom domain dijitalstaj.online with CDN (cdn.dijitalstaj.online)
Depth: Advanced — Deno selection indicates deliberate evaluation of runtimes, not just following defaults.

Skill: Self-Hosted Infrastructure Operations
Evidence:

MinIO for S3-compatible object storage (configured with MINIO_ENDPOINT, MINIO_FORCE_PATH_STYLE)
Docker containerization with multi-service orchestration (docker-compose.test.yml with healthchecks)
Custom CDN domain separate from API domain
Environment-based configuration for test/production parity
Depth: Advanced — "Operates" not "uses". Custom MinIO deployment with presigned URLs and in-memory test fallback demonstrates operational knowledge.

Skill: Legacy System Integration (Web Scraping)
Evidence:

lib/unipa/auth.ts: Complex ASP.NET form scraping from oidb.odu.edu.tr
Handles __EVENTTARGET, __EVENTARGUMENT hidden fields
Multi-page navigation with cookie jar management
HTML regex parsing for student data extraction (department, class level, national ID, parent names)
Depth: Advanced — Integrating with 2000s-era ASP.NET WebForms requires deep HTTP/cookie mechanics knowledge.

Skill: Email Infrastructure & Deliverability
Evidence:

Pluggable email architecture: MAILWAY=RESEND or SMTP
16+ transactional email templates with context-aware personalization
Resend integration for production + SMTP fallback for self-hosting
Depth: Advanced — Routing abstraction with environment-based provider selection shows production email ops experience.

Skill: RBAC & Workflow State Machines
Evidence:

4-role system: STUDENT, ADVISOR, OFFICE, ADMIN
9-state internship lifecycle: PENDING → COMPANY_APPROVED → ADVISOR_APPROVED → INTERNSHIP_IN_PROGRESS → ... → COMPLETED
Audit logging with JSONB oldData/newData tracking
Depth: Advanced — Complex state transitions with audit trails indicate enterprise workflow design experience.

Skill: PDF Document Generation & Form Automation
Evidence:

lib/pdf/ with typed document generators: d0.ts, d1.ts, d2.ts, d3.ts, d4.ts, sb2.ts, sb4.ts
pdf-lib for dynamic PDF field population
jspdf on frontend for client-side generation
Depth: Intermediate/Advanced — Government form automation requires precise field mapping.

Skill: Modern React Component Architecture
Evidence:

shadcn/ui pattern with components.json configuration
Radix UI primitives for accessibility
Custom Tiptap editor extensions (mathematics, color, typography)
lucide-react icon system
Depth: Advanced — Component architecture follows 2024 best practices with full RSC support.

Skill: Testing & CI/CD Design
Evidence:

30+ test files with granular categorization (test:skip-email, test:with-internship-seed)
In-memory MinIO store for test isolation (ENABLE_TEST_LOGIN mode)
Docker Compose test environment with PostgreSQL healthchecks
Depth: Advanced — Test environment design with service dependencies shows CI/CD pipeline experience.

Skill: API Design & Documentation
Evidence:

OpenAPI/Swagger generation with Scalar UI (/docs endpoint)
Bearer JWT security schema configuration
Structured error responses with discriminated unions
Depth: Intermediate — Proper API documentation with interactive reference.

Skill: Security Implementation
Evidence:

JWT signed with hono/jwt (HS256), 24h expiration
httpOnly, secure, sameSite: "lax" cookies
Request ID tracing with structured logging
Sentry integration with environment-based sampling rates
Depth: Intermediate/Advanced — Defense-in-depth with multiple security layers.

# Sportyluxe GYM

Skill: Deno & Modern JavaScript Runtime Ecosystem
Evidence:

Uses Deno as primary runtime with native cron support (Deno.cron), TypeScript out-of-the-box, and explicit permission flags
Leverages JSR (JavaScript Registry) for modern module distribution (@yelix/hono, @yelix/dashboard)
Properly handles Node.js compatibility layer via nodeModulesDir: "auto" and npm specifiers Depth: Advanced — Not just using Deno as a Node alternative, but exploiting Deno-native features like built-in cron scheduling and first-class TypeScript support.
Skill: Multi-Tenant SaaS Architecture
Evidence:

MONGODB_DB_NAME config pattern enables same-server/different-database multi-tenancy ("Same server, different db per gym")
Per-gym configuration via environment variables (GYM_NAME, GYM_LOCATION, GYM_CAPACITY)
Directus collection namespacing (sportylux_programs vs generic programs) Depth: Advanced — Understanding of true multi-tenant data isolation patterns, not just superficial "tenant_id" columns.
Skill: Push Notification Infrastructure (FCM)
Evidence:

Custom Firebase Admin SDK initialization with PEM key parsing for private key handling across environments
Direct FCM v1 REST API usage instead of relying on SDK (workaround for Deno HTTP/2 issues)
Token invalidation handling with automatic database cleanup on messaging/registration-token-not-registered
Notification persistence layer with scheduled notification queue processing Depth: Advanced — Operating FCM at protocol level, handling edge cases like token rotation and environment-specific key formatting (JSON-escaped newlines in env vars).
Skill: SMS/Phone Verification Systems
Evidence:

Twilio Verify API integration with rate-limiting via verificationSMSScore windowing algorithm
Custom phone format +90_5551236634 that normalizes to Twilio's +905551236634
Apple Store review bypass pattern with configurable test phones and static code 000000 Depth: Advanced — Production-grade SMS orchestration including fraud prevention, carrier formatting quirks, and App Store review compliance patterns.
Skill: Security Architecture
Evidence:

Temporary key system for QR code entry: 30-second TTL, single-use, auto-invalidation of previous keys
Bearer token auth middleware with Hono context propagation
Separate auth domains: JWT for API access, temporary UUID for physical door access Depth: Advanced — Understanding of threat modeling for physical access control, replay attack prevention, and principle of least privilege across authentication boundaries.
Skill: Headless CMS Integration
Evidence:

Directus SDK integration for content management (workout programs, exercises)
Authentication flow: REST client with JSON auth, login at module initialization time
Asset URL construction with fallback logic Depth: Intermediate — Correctly integrating external CMS as content source-of-truth, though this is standard SDK usage.
Skill: DevOps & Container Orchestration
Evidence:

Docker Compose with multi-service orchestration (Deno app + MongoDB)
Justfile for cross-platform task automation (Windows cmd.exe aware)
Health check endpoint pattern Depth: Intermediate — Solid containerization practices but not Kubernetes-level complexity.
Skill: Error Tracking & Observability
Evidence:

Sentry integration with Deno-specific cron monitoring (Sentry.denoCronIntegration())
Structured error capturing with tags and context throughout Twilio/FCM flows
Custom request logging middleware with timing Depth: Advanced — Implementing observability across async boundaries and external service calls, not just basic error logging.
Skill: Database Design & Data Integrity
Evidence:

MongoDB with Mongoose, connection resilience with auto-reconnect on disconnected event
Membership history audit trail with immutable snapshots and change logs (actor tracking, field-level diffs)
Proper schema discrimination using subdocument patterns (memberModal, personalTrainerModal for role-specific data) Depth: Advanced — Event sourcing-style audit logging and polymorphic schema design for role-based data.
Skill: API Design & Documentation
Evidence:

OpenAPI 3.1 generation via Yelix/Hono with Zod schemas driving both validation and documentation
Scalar API Reference integration for interactive docs
Comprehensive inline documentation in OpenAPI operation descriptions (security considerations, workflows) Depth: Advanced — Schema-first API design where validation and documentation are derived from the same source of truth.

# AAD (Chrome/Firefox Extension, Customizable GitHub)

Skill: Browser Extension Architecture (Manifest V3) Evidence: Complex content script system with provider pattern (storageProvider, userEventsProvider, themeProvider), custom security layer with user confirmation dialogs for network requests, drag-and-drop widget system, and deep GitHub DOM manipulation with multiple fallback strategies for username extraction. Depth: Advanced

Skill: Full-Stack System Design Evidence: Architected a 3-tier system: extension (client) → Next.js API (edge) → Go service (backend) + MongoDB. Uses custom domain aad.yelix.cloud with proper CORS configuration for cross-origin communication between GitHub pages and the API. Depth: Advanced

Skill: Frontend Engineering (React/Next.js) Evidence: Next.js 14 with App Router, comprehensive Radix UI primitive integration (15+ components), custom Tremor-based design system with dark mode support, Tailwind CSS with custom theme extensions, and instrumentation hooks for startup DB connection. Depth: Advanced

Skill: Go Backend Development Evidence: Gin framework with cron job scheduling (robfig/cron), MongoDB native driver usage with proper connection lifecycle management, user activity tracking with automated "dead user" detection after 15 days of inactivity. Depth: Intermediate

Skill: Security Engineering Evidence: Custom SHA-256 implementation (from scratch, no crypto library), network layer request whitelisting with user confirmation for untrusted domains, password hashing for admin authentication. Depth: Intermediate

Skill: Database Design & DevOps Evidence: MongoDB with connection caching pattern (global.mongoose), environment-based configuration (.env.local), Vercel deployment with custom headers, separate Go service likely self-hosted on custom domain. Depth: Intermediate

Skill: UX/Product Engineering Evidence: Onboarding system with "studio" and "video" introduction, notification manager with action buttons, widget drag-and-drop with position persistence, semantic versioning enforcement (soft/hard minimum versions), and dark mode support throughout. Depth: Advanced

# Astro Hackathon

Skill Analysis
Backend Architecture & API Design
Evidence: Custom endpoint auto-discovery system (findEndpoints.ts, serveEndpoints.ts, endpointList.ts), dynamic route registration with middleware chaining, OpenAPI auto-generation from Zod schemas, OPTIONS endpoint auto-generation Depth: Advanced

Deno Runtime Expertise
Evidence: Uses Deno as primary runtime (not Node.js), leverages Deno-native permissions (--allow-net, --allow-read, --allow-env), Deno.serve for HTTP, imports via npm: and jsr: specifiers, Deno-specific testing with @std/testing Depth: Advanced — Shows production-level familiarity with Deno's unique paradigm, not just surface usage

TypeScript & Type Safety
Evidence: Comprehensive Zod validation schemas with .describe() annotations for OpenAPI, custom type definitions for endpoints, strict typing on MongoDB models, middleware context typing Depth: Advanced — Uses TypeScript as a design tool, not just syntax

Authentication & Security Engineering
Evidence: Custom JWT implementation with refresh token rotation, bearer auth middleware with Hono, IP-based rate limiting, IP whitelist for docs access, password hashing, email verification with OTP Depth: Advanced — Implements auth correctly without relying on off-the-shelf solutions

Database Operations (MongoDB/Mongoose)
Evidence: Connection retry logic with exponential backoff, connection state management middleware, Mongoose schema design with proper types, MongoDB aggregation patterns Depth: Intermediate-Advanced

DevOps & Infrastructure
Evidence: Multi-platform deployment configs (Deno Deploy, Vercel, AWS Amplify), environment-based CORS origins, self-hosted Supabase for storage, custom domain configurations, IP-based access control for API docs Depth: Advanced — Operates/maintains services, not just consumes them

Observability & Distributed Tracing
Evidence: Custom request tracer with span timing, UUID-based trace IDs in headers, color-coded console output with grouped logs, structured logging service with metadata filtering by role Depth: Advanced — Built mini-Datadog internally, not just using console.log

Testing & Quality Assurance
Evidence: E2E test suite with custom mock request utilities, global test store for cleanup, faker.js for test data, BDD-style tests with @std/testing, database cleanup patterns Depth: Intermediate-Advanced

React/Next.js Frontend Architecture
Evidence: App Router with [locale] dynamic segments, route groups (main)/(admin), TanStack Query for server state with custom hooks per endpoint, memory cleanup utilities (timers, event listeners, async cleanup) Depth: Advanced — Production-grade patterns, not tutorial-level React

Internationalization (i18n)
Evidence: next-intl with locale prefix strategy, middleware-based routing, message files for EN/TR, navigation helpers Depth: Intermediate

Mapping & Geospatial
Evidence: Multiple map libraries: Deck.gl, Leaflet, react-simple-maps, MapLibre GL, Pigeon Maps, Google Maps API — for location-based event features Depth: Intermediate — Broad familiarity with geospatial tooling

API Documentation
Evidence: Auto-generated OpenAPI 3.1.0 spec from code, custom Zod-to-JSON schema converter covering all Zod types (optional, nullable, default, unions, etc.), Scalar API Reference UI Depth: Advanced — Built a complete documentation pipeline from Zod schemas

Email & External Services Integration
Evidence: Resend API for transactional email, Supabase for file storage, custom email service abstraction Depth: Intermediate 