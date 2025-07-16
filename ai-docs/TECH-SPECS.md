## System Architecture Analysis
Core Technology Stack
Frontend: NextJS 14+ with App Router, TypeScript, React Server Components
Backend: NextJS Server Actions for mutations, API routes for webhooks
Database: Supabase (PostgreSQL) with Prisma ORM
Authentication: Clerk for multi-role auth with custom metadata
State Management: Zustand for client state, Server state via React Query/SWR
Styling: TailwindCSS with custom design system tokens
Animations: Framer Motion for complex interactions
Deployment: Vercel with Edge Functions
Infrastructure Requirements
Real-time: Supabase Realtime for visit updates, notifications
Storage: Supabase Storage for documents, property images
CDN: Vercel Edge Network for static assets
Background Jobs: Vercel Cron Jobs or Supabase Edge Functions
Caching: Vercel KV for session data, Redis for complex caching
Feature Breakdown
1. User Management & Multi-Role Authentication
Roles: Owner, Tenant, Martillero (Real Estate Agent), ALP (Property Validator)
KYC Process: DNI/CUIT validation via OCR, BCRA/Veraz credit checks
Trust Scoring: Algorithm based on verification level, payment history, reviews
Document Storage: Encrypted storage in Supabase with access control
Challenges:
Integrating multiple external APIs (BCRA, Veraz)
Handling OCR accuracy and manual override
Real-time trust score calculation
Role-based UI/UX differentiation
2. Property Listing & Management
Photo Optimization: Client-side compression, Cloudinary integration
AI Pricing: ML model for zone-based pricing suggestions
Availability Calendar: Complex state management for date ranges
ALP Validation: Workflow for first listing verification
Challenges:
Large file uploads with progress tracking
Offline photo capture on mobile
AI model integration and training data
Calendar conflict resolution
3. Visit Coordination & ALP Management
Geolocation: Browser API + fallback to IP-based location
Offline Capability: Service Workers, IndexedDB for offline data
Real-time Updates: WebSockets via Supabase Realtime
Assignment Algorithm: Distance + availability + performance score
Challenges:
Offline-first architecture with sync
Real-time coordination across multiple users
GPS accuracy in buildings
Route optimization for multiple visits
4. Insurance & Financial Integration
Insurance APIs: Real-time quote generation, policy management
MercadoPago: Split payments, webhooks for confirmations
Commission Distribution: Automated splits based on roles
Late Payment Tracking: Scheduled jobs for monitoring
Challenges:
Payment webhook reliability
Commission calculation complexity
Financial compliance reporting
Handling payment failures gracefully
5. Contract Generation & Legal Compliance
Template Engine: React PDF or Puppeteer for generation
E-signatures: DocuSign/ZapSign integration
Version Control: Database-backed versioning with diff tracking
Tax Calculations: Regional API integration for stamp taxes
Challenges:
Multi-party signature coordination
Template flexibility vs standardization
Regional tax law variations
Audit trail requirements
Data Models
Core Entities
User: Multi-role support with verification status
Property: Listings with media, availability, pricing
Visit: Coordination between tenants and ALPs
Contract: Legal documents with signatures
Transaction: Financial records with commission splits
Insurance: Policies and claims
Review: Multi-directional rating system
Notification: Multi-channel delivery tracking
API Structure
Internal APIs (Server Actions)
User authentication and verification
Property CRUD operations
Visit scheduling and management
Contract generation and signing
Payment processing
Notification dispatch
External Integrations
Clerk (Auth)
BCRA/Veraz (Credit checks)
OCR Services (Document validation)
MercadoPago (Payments)
Insurance Partners (Quotes/Policies)
E-signature Providers (DocuSign/ZapSign)
WhatsApp Business API (Notifications)
Google Maps (Geocoding/Routing)
Security Considerations
Authentication: Clerk with custom claims for roles
Authorization: Row-level security in Supabase
Data Encryption: At-rest encryption for sensitive documents
PII Protection: Separate PII storage with access logging
API Security: Rate limiting, webhook signatures
Financial Security: PCI compliance via MercadoPago
Performance & Scalability
Database: Indexes on frequently queried fields
Caching: Strategic caching of property listings, user profiles
Image Optimization: Next/Image with on-demand optimization
Code Splitting: Route-based splitting with dynamic imports
Background Jobs: Queue system for heavy operations
Real-time: Connection pooling for WebSocket efficiency
Project Structure
/src
  /app                    # Next.js app directory
    /(auth)              # Auth-related pages
    /(dashboard)         # Role-specific dashboards
    /(public)            # Public pages
    /api                 # API routes for webhooks
  /components            # Shared components
    /ui                  # Design system components
    /features            # Feature-specific components
  /features              # Feature modules
    /auth                # Authentication logic
    /properties          # Property management
    /visits              # Visit coordination
    /contracts           # Contract handling
    /payments            # Financial operations
  /hooks                 # Custom React hooks
  /lib                   # Utilities and configurations
    /db                  # Database client and queries
    /api                 # External API clients
    /utils               # Helper functions
  /stores                # Zustand stores
  /types                 # TypeScript type definitions
  /styles                # Global styles and Tailwind config

Deployment Strategy
Environment: Development, Staging, Production
CI/CD: GitHub Actions with Vercel integration
Database Migrations: Prisma migrate with review apps
Feature Flags: Vercel Edge Config for gradual rollouts
Monitoring: Vercel Analytics + Sentry for errors
Backup: Automated Supabase backups </brainstorm>
NEO-RENT Technical Specification
1. Executive Summary
NEO-RENT is a revolutionary rental platform for the Argentine market that eliminates traditional rental guarantees through innovative insurance integration, leveraging the regulatory changes introduced by DNU 70/2023. The platform connects property owners, tenants, real estate agents (Martilleros), and property validators (ALPs) in a seamless digital ecosystem.
Key Technical Decisions and Rationale
NextJS with Server Actions: Chosen for its excellent DX, built-in optimizations, and seamless server-client integration
Supabase + Prisma: Combines managed PostgreSQL infrastructure with type-safe ORM for rapid development
Clerk Authentication: Enterprise-grade auth solution with native multi-role support
Vercel Deployment: Optimal for NextJS with edge computing capabilities for Latin American market
Zustand State Management: Lightweight, TypeScript-first solution perfect for complex client state
Progressive Web App: Enables offline functionality crucial for ALPs in areas with poor connectivity
High-level Architecture
┌─────────────────────────────────────────────────────────────┐
│                         Client Layer                         │
├─────────────────────────┬───────────────────────────────────┤
│   NextJS Web App (SSR)  │   ALP Mobile PWA (Offline-First) │
└─────────────────────────┴───────────────────────────────────┘
                                    │
┌─────────────────────────────────────────────────────────────┐
│                    API Gateway (Vercel)                      │
├─────────────────────────┬───────────────────────────────────┤
│   Server Actions (RPC)  │      Edge Functions (REST)        │
└─────────────────────────┴───────────────────────────────────┘
                                    │
┌─────────────────────────────────────────────────────────────┐
│                    Authentication Layer                       │
│                         Clerk Auth                           │
└─────────────────────────────────────────────────────────────┘
                                    │
┌─────────────────────────────────────────────────────────────┐
│                      Service Layer                           │
├───────────┬────────────┬──────────┬────────────┬────────────┤
│   Users   │ Properties │  Visits  │ Contracts  │  Finance   │
└───────────┴────────────┴──────────┴────────────┴────────────┘
                                    │
┌─────────────────────────────────────────────────────────────┐
│                       Data Layer                             │
├─────────────────────────┬───────────────────────────────────┤
│  Supabase PostgreSQL    │        Supabase Storage          │
│    (via Prisma ORM)     │     (Documents & Images)         │
└─────────────────────────┴───────────────────────────────────┘

Technology Stack Summary
Layer
Technology
Purpose
Frontend
NextJS 14+
React framework with SSR/SSG
Styling
TailwindCSS
Utility-first CSS framework
Animation
Framer Motion
Physics-based animations
State
Zustand
Client state management
Backend
Server Actions
Type-safe RPC mutations
Database
Supabase (PostgreSQL)
Managed database with real-time
ORM
Prisma
Type-safe database client
Auth
Clerk
Multi-role authentication
Storage
Supabase Storage
File storage with CDN
Deployment
Vercel
Edge deployment platform

2. System Architecture
2.1 Architecture Overview
The system follows a modern, serverless architecture optimized for the Latin American market with considerations for intermittent connectivity and mobile-first usage.
System Components
Web Application (NextJS)


Server-side rendered for SEO and performance
Progressive enhancement for offline capabilities
Responsive design with mobile-first approach
Mobile PWA (ALP App)


Service Worker for offline functionality
IndexedDB for local data persistence
Background sync for data reconciliation
API Layer


Server Actions for authenticated mutations
Edge Functions for webhooks and public APIs
RESTful endpoints for third-party integrations
Real-time Infrastructure


Supabase Realtime for live updates
Presence tracking for active users
Optimistic UI updates with conflict resolution
Background Processing


Scheduled jobs for payment processing
Queue system for email/SMS notifications
Batch operations for data synchronization
Data Flow
sequenceDiagram
    participant U as User
    participant W as Web App
    participant SA as Server Action
    participant DB as Database
    participant EXT as External API

    U->>W: Interact with UI
    W->>SA: Call Server Action
    SA->>DB: Query/Mutation
    SA->>EXT: External API Call
    EXT-->>SA: API Response
    SA-->>W: Typed Response
    W-->>U: Update UI

    Note over DB: Real-time subscription
    DB-->>W: Live updates via WebSocket

2.2 Technology Stack
Frontend Technologies
Core Framework
NextJS 14.2+ with App Router
TypeScript 5.3+ with strict mode
React 18+ with Server Components
UI/UX Libraries
TailwindCSS 3.4+ with custom design tokens
Framer Motion 11+ for animations
Radix UI for accessible components
React Hook Form for form management
Zod for schema validation
Development Tools
ESLint + Prettier for code quality
Husky for git hooks
Jest + React Testing Library
Playwright for E2E testing
Backend Technologies
Runtime & Framework
Node.js 20 LTS
NextJS Server Actions
Edge Runtime for performance
Database & ORM
Supabase (PostgreSQL 15)
Prisma 5.10+ with migrations
Redis for caching (via Upstash)
Authentication & Security
Clerk with custom metadata
JWT for API authentication
bcrypt for additional hashing
External Services
Infrastructure
Vercel for hosting and CI/CD
Cloudflare for CDN and DDoS protection
Sentry for error monitoring
Vercel Analytics for metrics
Third-party APIs
BCRA API for credit checks
Veraz/Nosis for identity verification
MercadoPago SDK for payments
WhatsApp Business API for notifications
Google Maps Platform for geocoding
AWS S3 compatible storage (Supabase)
DocuSign/ZapSign for e-signatures
Custom OCR service for document scanning
3. Feature Specifications
3.1 User Management & Multi-Role Authentication
User Stories and Acceptance Criteria
As a Property Owner:
I want to register quickly using my DNI/CUIT
I want to verify my identity to build trust
I want to list multiple properties efficiently
I want to track my rental income and occupancy
As a Tenant:
I want to search properties without upfront guarantee requirements
I want to verify my income to access better properties
I want to schedule visits efficiently
I want transparent contract terms and insurance options
As a Martillero:
I want to validate my professional license
I want to manage multiple property listings
I want to review and approve contracts
I want to track my commissions
As an ALP:
I want to manage my visit schedule efficiently
I want offline access to property information
I want to submit visit reports easily
I want to track my earnings
Technical Requirements
// User role enum
enum UserRole {
  OWNER = 'OWNER',
  TENANT = 'TENANT',
  MARTILLERO = 'MARTILLERO',
  ALP = 'ALP'
}

// Verification status tracking
enum VerificationStatus {
  UNVERIFIED = 'UNVERIFIED',
  PENDING = 'PENDING',
  VERIFIED = 'VERIFIED',
  REJECTED = 'REJECTED'
}

// Trust score calculation
interface TrustScore {
  overall: number; // 0-100
  components: {
    identityVerification: number;
    creditCheck: number;
    paymentHistory: number;
    userReviews: number;
  };
  lastUpdated: Date;
}

Implementation Approach
Registration Flow

 // Server Action for registration
'use server';

export async function registerUser(data: {
  email: string;
  phone: string;
  role: UserRole;
  dni: string;
}) {
  // Validate input with Zod
  const validated = registrationSchema.parse(data);

  // Create Clerk user
  const clerkUser = await clerk.users.createUser({
    emailAddress: validated.email,
    phoneNumber: validated.phone,
    publicMetadata: { role: validated.role }
  });

  // Create database record
  const dbUser = await prisma.user.create({
    data: {
      clerkId: clerkUser.id,
      ...validated,
      verificationStatus: 'UNVERIFIED',
      trustScore: { create: { overall: 0 } }
    }
  });

  // Trigger verification workflow
  await startVerificationProcess(dbUser.id);

  return { success: true, userId: dbUser.id };
}


Document Verification with OCR

 export async function verifyDocument(
  userId: string,
  documentImage: File
) {
  // Upload to Supabase Storage
  const { data: upload } = await supabase.storage
    .from('documents')
    .upload(`${userId}/dni-${Date.now()}.jpg`, documentImage);

  // Send to OCR service
  const ocrResult = await ocrService.extractDNI(upload.path);

  // Validate extracted data
  const validation = await validateDNIWithRENAPER(ocrResult);

  // Update user verification
  await prisma.user.update({
    where: { id: userId },
    data: {
      verificationStatus: validation.success ? 'VERIFIED' : 'REJECTED',
      verificationData: validation.data
    }
  });
}


Credit Check Integration

 export async function performCreditCheck(userId: string) {
  const user = await prisma.user.findUnique({
    where: { id: userId },
    include: { verificationData: true }
  });

  // BCRA API call
  const bcraResult = await bcraClient.checkCredit({
    cuit: user.verificationData.cuit,
    dni: user.verificationData.dni
  });

  // Veraz/Nosis check
  const verazResult = await verazClient.getScore({
    document: user.verificationData.dni
  });

  // Calculate trust score
  const trustScore = calculateTrustScore({
    bcra: bcraResult,
    veraz: verazResult,
    verified: user.verificationStatus === 'VERIFIED'
  });

  // Update user record
  await prisma.trustScore.update({
    where: { userId },
    data: trustScore
  });
}


User Flow Diagrams
graph TD
    A[User Selects Role] --> B{Role Type}
    B -->|Owner/Martillero| C[Professional Registration]
    B -->|Tenant| D[Basic Registration]
    B -->|ALP| E[ALP Registration]

    C --> F[License Verification]
    D --> G[Income Verification]
    E --> H[Background Check]

    F --> I[Document Upload]
    G --> I
    H --> I

    I --> J[OCR Processing]
    J --> K[External Validation]
    K --> L[Trust Score Calculation]
    L --> M[Account Activated]

Error Handling
OCR Failures: Manual review queue with admin interface
API Timeouts: Retry mechanism with exponential backoff
Validation Errors: Clear user feedback with correction options
Document Quality: Client-side validation before upload
3.2 Property Listing & Management
Technical Requirements
interface Property {
  id: string;
  ownerId: string;
  martilleroId?: string;

  // Basic Information
  title: string;
  description: string;
  type: 'HOUSE' | 'APARTMENT' | 'STUDIO';

  // Location
  address: Address;
  coordinates: { lat: number; lng: number; };
  neighborhood: string;

  // Specifications
  bedrooms: number;
  bathrooms: number;
  surfaceArea: number;

  // Pricing
  basePrice: number;
  suggestedPrice?: number;
  currency: 'ARS' | 'USD';

  // Media
  images: PropertyImage[];
  virtualTour?: string;

  // Availability
  availableFrom: Date;
  minimumStay?: number;

  // Features
  amenities: string[];
  petFriendly: boolean;
  furnished: boolean;

  // Validation
  alpValidated: boolean;
  validationDate?: Date;

  // Analytics
  views: number;
  inquiries: number;
  conversionRate?: number;
}

Implementation Approach
Multi-step Listing Wizard

 // Zustand store for wizard state
interface ListingWizardStore {
  step: number;
  data: Partial<Property>;
  images: File[];

  setStep: (step: number) => void;
  updateData: (data: Partial<Property>) => void;
  addImages: (images: File[]) => void;
  submit: () => Promise<void>;
}

// Server Action for property creation
export async function createProperty(
  data: PropertyCreateInput,
  images: FormData
) {
  // Validate property data
  const validated = propertySchema.parse(data);

  // Geocode address
  const coordinates = await geocodeAddress(validated.address);

  // Create property record
  const property = await prisma.property.create({
    data: {
      ...validated,
      coordinates,
      status: 'PENDING_VALIDATION'
    }
  });

  // Upload and optimize images
  const imageUrls = await processPropertyImages(
    property.id,
    images
  );

  // Get AI pricing suggestion
  const suggestedPrice = await getPricingSuggestion({
    ...validated,
    neighborhood: coordinates.neighborhood
  });

  // Update with images and pricing
  await prisma.property.update({
    where: { id: property.id },
    data: {
      images: { create: imageUrls },
      suggestedPrice
    }
  });

  // Schedule ALP validation
  await scheduleValidation(property.id);

  return { success: true, propertyId: property.id };
}


Image Processing Pipeline

 async function processPropertyImages(
  propertyId: string,
  images: FormData
): Promise<PropertyImage[]> {
  const processedImages = [];

  for (const [key, file] of images.entries()) {
    if (file instanceof File) {
      // Client-side optimization already done
      // Additional server-side processing
      const optimized = await sharp(await file.arrayBuffer())
        .resize(1920, 1080, { fit: 'inside' })
        .jpeg({ quality: 85 })
        .toBuffer();

      // Upload to Supabase Storage
      const filename = `${propertyId}/${key}-${Date.now()}.jpg`;
      const { data } = await supabase.storage
        .from('properties')
        .upload(filename, optimized);

      // Generate thumbnails
      const thumbnail = await generateThumbnail(optimized);
      const thumbPath = `${propertyId}/thumb-${key}.jpg`;
      await supabase.storage
        .from('properties')
        .upload(thumbPath, thumbnail);

      processedImages.push({
        url: data.path,
        thumbnailUrl: thumbPath,
        order: parseInt(key),
        alt: `Property image ${key}`
      });
    }
  }

  return processedImages;
}


AI-Powered Pricing

 interface PricingFactors {
  location: { lat: number; lng: number; neighborhood: string; };
  type: string;
  bedrooms: number;
  surfaceArea: number;
  amenities: string[];
}

async function getPricingSuggestion(
  factors: PricingFactors
): Promise<number> {
  // Get comparable properties
  const comparables = await prisma.property.findMany({
    where: {
      neighborhood: factors.neighborhood,
      type: factors.type,
      bedrooms: { gte: factors.bedrooms - 1, lte: factors.bedrooms + 1 },
      status: 'ACTIVE'
    },
    select: {
      basePrice: true,
      surfaceArea: true,
      amenities: true,
      conversionRate: true
    }
  });

  // Calculate price per m²
  const pricePerM2 = comparables.map(c => ({
    price: c.basePrice / c.surfaceArea,
    weight: c.conversionRate || 0.5
  }));

  // Weighted average with amenity adjustments
  const basePrice = calculateWeightedPrice(pricePerM2, factors);

  // Market demand adjustment
  const demandMultiplier = await getMarketDemand(factors.neighborhood);

  return Math.round(basePrice * demandMultiplier);
}


User Flow
Address Entry: Autocomplete with map visualization
Property Details: Type, rooms, size, amenities
Photo Upload: Drag-drop with optimization feedback
Pricing: AI suggestion with manual override
Availability: Calendar with booking rules
Review: Summary with edit capabilities
Validation: ALP assignment for first listing
3.3 Visit Coordination & ALP Management
Technical Requirements
interface Visit {
  id: string;
  propertyId: string;
  tenantId: string;
  alpId?: string;

  // Scheduling
  scheduledDate: Date;
  duration: number; // minutes
  status: VisitStatus;

  // Location
  meetingPoint?: string;
  accessInstructions?: string;

  // Validation
  checkInTime?: Date;
  checkOutTime?: Date;
  report?: VisitReport;

  // Communication
  messages: Message[];
  calls: CallLog[];
}

enum VisitStatus {
  REQUESTED = 'REQUESTED',
  SCHEDULED = 'SCHEDULED',
  CONFIRMED = 'CONFIRMED',
  IN_PROGRESS = 'IN_PROGRESS',
  COMPLETED = 'COMPLETED',
  CANCELLED = 'CANCELLED',
  NO_SHOW = 'NO_SHOW'
}

Implementation Approach
ALP Assignment Algorithm

 export async function assignALP(visitId: string) {
  const visit = await prisma.visit.findUnique({
    where: { id: visitId },
    include: { property: true }
  });

  // Get available ALPs
  const availableALPs = await prisma.user.findMany({
    where: {
      role: 'ALP',
      status: 'ACTIVE',
      availability: {
        some: {
          date: visit.scheduledDate,
          isAvailable: true
        }
      }
    },
    include: {
      location: true,
      stats: true
    }
  });

  // Calculate scores
  const scoredALPs = await Promise.all(
    availableALPs.map(async (alp) => {
      const distance = calculateDistance(
        alp.location,
        visit.property.coordinates
      );

      const score = calculateALPScore({
        distance,
        rating: alp.stats.averageRating,
        completionRate: alp.stats.completionRate,
        responseTime: alp.stats.avgResponseTime
      });

      return { alp, score };
    })
  );

  // Sort by score and assign
  const bestALP = scoredALPs.sort((a, b) => b.score - a.score)[0];

  await prisma.visit.update({
    where: { id: visitId },
    data: {
      alpId: bestALP.alp.id,
      status: 'SCHEDULED'
    }
  });

  // Send notifications
  await notifyALP(bestALP.alp.id, visit);
}


Offline-Capable Mobile App

 // Service Worker for offline functionality
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('neo-rent-v1').then((cache) => {
      return cache.addAll([
        '/',
        '/offline',
        '/static/js/bundle.js',
        '/static/css/main.css'
      ]);
    })
  );
});

// IndexedDB schema for offline data
interface OfflineVisit {
  id: string;
  propertyData: Property;
  scheduledDate: Date;
  checkInTime?: Date;
  photos: Blob[];
  notes: string;
  syncStatus: 'pending' | 'synced' | 'error';
}

// Sync when back online
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-visits') {
    event.waitUntil(syncOfflineVisits());
  }
});

async function syncOfflineVisits() {
  const db = await openDB('neo-rent', 1);
  const visits = await db.getAll('offline-visits');

  for (const visit of visits) {
    if (visit.syncStatus === 'pending') {
      try {
        await fetch('/api/visits/sync', {
          method: 'POST',
          body: JSON.stringify(visit)
        });

        await db.put('offline-visits', {
          ...visit,
          syncStatus: 'synced'
        });
      } catch (error) {
        console.error('Sync failed:', error);
      }
    }
  }
}


Real-time Coordination

 // Supabase Realtime subscription
export function useVisitUpdates(visitId: string) {
  useEffect(() => {
    const channel = supabase
      .channel(`visit:${visitId}`)
      .on(
        'postgres_changes',
        {
          event: '*',
          schema: 'public',
          table: 'visits',
          filter: `id=eq.${visitId}`
        },
        (payload) => {
          // Update local state
          updateVisit(payload.new);
        }
      )
      .on('presence', { event: 'sync' }, () => {
        const state = channel.presenceState();
        updateActiveUsers(state);
      })
      .subscribe();

    return () => {
      supabase.removeChannel(channel);
    };
  }, [visitId]);
}


3.4 Insurance & Financial Integration
Technical Requirements
interface InsurancePolicy {
  id: string;
  tenantId: string;
  propertyId: string;
  contractId: string;

  // Coverage
  provider: string;
  policyNumber: string;
  coverageType: 'BASIC' | 'COMPLETE' | 'PREMIUM';
  coverageAmount: number;

  // Pricing
  monthlyPremium: number;
  paymentMethod: 'MONTHLY' | 'ANNUAL';

  // Status
  status: 'QUOTED' | 'ACTIVE' | 'SUSPENDED' | 'CANCELLED';
  startDate: Date;
  endDate: Date;

  // Claims
  claims: Claim[];
}

interface Transaction {
  id: string;
  type: 'RENT' | 'DEPOSIT' | 'INSURANCE' | 'COMMISSION';

  // Parties
  fromUserId: string;
  toUserId?: string;

  // Amount
  amount: number;
  currency: 'ARS';

  // Payment
  paymentMethod: string;
  mercadopagoId?: string;
  status: 'PENDING' | 'PROCESSING' | 'COMPLETED' | 'FAILED';

  // Commission splits
  splits?: CommissionSplit[];
}

Implementation Approach
Insurance Quote Generation

 export async function generateInsuranceQuote(
  data: QuoteRequest
): Promise<QuoteResponse> {
  // Validate tenant trust score
  const tenant = await prisma.user.findUnique({
    where: { id: data.tenantId },
    include: { trustScore: true }
  });

  if (tenant.trustScore.overall < 60) {
    throw new Error('Insufficient trust score for insurance');
  }

  // Call insurance partner APIs
  const quotes = await Promise.all([
    sancorAPI.getQuote(data),
    libertadAPI.getQuote(data),
    provinciaAPI.getQuote(data)
  ]);

  // Process and normalize quotes
  const processedQuotes = quotes.map(quote => ({
    provider: quote.provider,
    coverageType: mapCoverageType(quote),
    monthlyPremium: quote.premium,
    coverageAmount: quote.coverage,
    features: quote.features
  }));

  // Store quotes for reference
  await prisma.insuranceQuote.create({
    data: {
      tenantId: data.tenantId,
      propertyId: data.propertyId,
      quotes: processedQuotes,
      expiresAt: new Date(Date.now() + 24 * 60 * 60 * 1000)
    }
  });

  return { quotes: processedQuotes };
}


MercadoPago Integration

 export async function processPayment(
  data: PaymentRequest
): Promise<PaymentResult> {
  // Create MercadoPago preference
  const preference = {
    items: [{
      title: data.description,
      unit_price: data.amount,
      quantity: 1
    }],
    payer: {
      email: data.payerEmail
    },
    payment_methods: {
      excluded_payment_types: [{ id: 'ticket' }],
      installments: 1
    },
    notification_url: `${process.env.NEXT_PUBLIC_URL}/api/webhooks/mercadopago`,
    back_urls: {
      success: `${process.env.NEXT_PUBLIC_URL}/payments/success`,
      failure: `${process.env.NEXT_PUBLIC_URL}/payments/failure`
    }
  };

  const mpResponse = await mercadopago.preferences.create(preference);

  // Create transaction record
  const transaction = await prisma.transaction.create({
    data: {
      type: data.type,
      amount: data.amount,
      fromUserId: data.payerId,
      status: 'PENDING',
      mercadopagoId: mpResponse.body.id,
      metadata: {
        preferenceId: mpResponse.body.id,
        initPoint: mpResponse.body.init_point
      }
    }
  });

  return {
    transactionId: transaction.id,
    paymentUrl: mpResponse.body.init_point
  };
}

// Webhook handler
export async function handleMercadoPagoWebhook(
  notification: MPNotification
) {
  if (notification.type === 'payment') {
    const payment = await mercadopago.payment.findById(
      notification.data.id
    );

    if (payment.body.status === 'approved') {
      // Update transaction
      const transaction = await prisma.transaction.update({
        where: { mercadopagoId: payment.body.external_reference },
        data: { status: 'COMPLETED' }
      });

      // Process commission splits
      await processCommissionSplits(transaction.id);

      // Activate insurance if applicable
      if (transaction.type === 'INSURANCE') {
        await activateInsurancePolicy(transaction.metadata.policyId);
      }
    }
  }
}


Commission Distribution

 async function processCommissionSplits(transactionId: string) {
  const transaction = await prisma.transaction.findUnique({
    where: { id: transactionId },
    include: { contract: { include: { property: true } } }
  });

  const splits = calculateCommissionSplits({
    totalAmount: transaction.amount,
    hasInsurance: !!transaction.contract.insuranceId,
    hasMartillero: !!transaction.contract.property.martilleroId
  });

  // Create split transactions
  for (const split of splits) {
    await prisma.transaction.create({
      data: {
        type: 'COMMISSION',
        amount: split.amount,
        fromUserId: transaction.fromUserId,
        toUserId: split.recipientId,
        parentTransactionId: transactionId,
        status: 'PENDING'
      }
    });

    // Schedule payout
    await scheduleCommissionPayout(split);
  }
}

function calculateCommissionSplits(params: SplitParams) {
  const splits = [];
  const { totalAmount, hasInsurance, hasMartillero } = params;

  // Platform fee: 2.5%
  splits.push({
    recipientId: 'PLATFORM',
    amount: totalAmount * 0.025,
    type: 'PLATFORM_FEE'
  });

  // Insurance commission: 0.5% if applicable
  if (hasInsurance) {
    splits.push({
      recipientId: 'INSURANCE_PARTNER',
      amount: totalAmount * 0.005,
      type: 'INSURANCE_COMMISSION'
    });
  }

  // Martillero commission: 4.5% of first month
  if (hasMartillero) {
    splits.push({
      recipientId: params.martilleroId,
      amount: totalAmount * 0.045,
      type: 'MARTILLERO_COMMISSION'
    });
  }

  return splits;
}


3.5 Contract Generation & Legal Compliance
Technical Requirements
interface Contract {
  id: string;
  propertyId: string;
  ownerId: string;
  tenantId: string;
  martilleroId?: string;

  // Terms
  startDate: Date;
  endDate: Date;
  monthlyRent: number;
  depositAmount: number;

  // Document
  templateId: string;
  version: number;
  content: string; // HTML/PDF content

  // Signatures
  signatures: Signature[];
  signatureStatus: 'PENDING' | 'PARTIAL' | 'COMPLETE';

  // Legal
  stampTax: number;
  registrationNumber?: string;

  // Audit
  createdAt: Date;
  modifications: ContractModification[];
}

interface Signature {
  userId: string;
  role: 'OWNER' | 'TENANT' | 'MARTILLERO' | 'WITNESS';
  signedAt?: Date;
  ipAddress?: string;
  documentHash: string;
}

Implementation Approach
Template-Based Generation

 export async function generateContract(
  data: ContractData
): Promise<Contract> {
  // Load template based on property location
  const template = await loadContractTemplate({
    province: data.property.province,
    type: data.property.type
  });

  // Calculate stamp tax
  const stampTax = await calculateStampTax({
    province: data.property.province,
    monthlyRent: data.monthlyRent,
    duration: data.durationMonths
  });

  // Generate contract content
  const content = await renderContractTemplate({
    template,
    data: {
      ...data,
      stampTax,
      legalClauses: getLegalClauses(data.property.province)
    }
  });

  // Create contract record
  const contract = await prisma.contract.create({
    data: {
      ...data,
      content,
      stampTax,
      version: 1,
      signatureStatus: 'PENDING',
      documentHash: hashDocument(content)
    }
  });

  // Initialize signature workflow
  await initializeSignatures(contract.id);

  return contract;
}

async function renderContractTemplate(
  params: RenderParams
): Promise<string> {
  const { template, data } = params;

  // Use React PDF for generation
  const ContractDocument = () => (
    <Document>
      <Page size="A4" style={styles.page}>
        <View style={styles.header}>
          <Text>CONTRATO DE LOCACIÓN</Text>
          <Text>DNU 70/2023</Text>
        </View>

        <View style={styles.parties}>
          <Text>LOCADOR: {data.owner.name}</Text>
          <Text>LOCATARIO: {data.tenant.name}</Text>
          {data.martillero && (
            <Text>MARTILLERO: {data.martillero.name}</Text>
          )}
        </View>

        <View style={styles.terms}>
          {/* Contract terms */}
        </View>

        <View style={styles.signatures}>
          {/* Signature blocks */}
        </View>
      </Page>
    </Document>
  );

  const pdf = await renderToStream(<ContractDocument />);
  return pdf;
}


E-Signature Integration

 export async function requestSignature(
  contractId: string,
  userId: string
) {
  const contract = await prisma.contract.findUnique({
    where: { id: contractId },
    include: { signatures: true }
  });

  // Check if user can sign
  const userSignature = contract.signatures.find(
    s => s.userId === userId
  );

  if (!userSignature || userSignature.signedAt) {
    throw new Error('Invalid signature request');
  }

  // Generate signature request
  const signatureRequest = await docuSignClient.createEnvelope({
    templateId: 'neo-rent-contract',
    recipients: [{
      email: userSignature.email,
      name: userSignature.name,
      roleName: userSignature.role
    }],
    customFields: {
      contractId,
      documentHash: contract.documentHash
    }
  });

  // Store signature request ID
  await prisma.signature.update({
    where: {
      contractId_userId: { contractId, userId }
    },
    data: {
      externalId: signatureRequest.envelopeId,
      status: 'SENT'
    }
  });

  return {
    signingUrl: signatureRequest.signingUrl
  };
}

// Webhook handler for signature completion
export async function handleSignatureWebhook(
  event: DocuSignEvent
) {
  if (event.event === 'envelope-completed') {
    const { contractId } = event.customFields;

    // Update signature record
    await prisma.signature.updateMany({
      where: {
        contractId,
        externalId: event.envelopeId
      },
      data: {
        signedAt: new Date(),
        status: 'COMPLETED'
      }
    });

    // Check if all signatures complete
    const contract = await prisma.contract.findUnique({
      where: { id: contractId },
      include: { signatures: true }
    });

    const allSigned = contract.signatures.every(
      s => s.status === 'COMPLETED'
    );

    if (allSigned) {
      await finalizeContract(contractId);
    }
  }
}


Tax Compliance

 interface StampTaxCalculator {
  calculate(params: TaxParams): Promise<number>;
}

class NeuquenStampTax implements StampTaxCalculator {
  async calculate(params: TaxParams): Promise<number> {
    const { monthlyRent, durationMonths } = params;
    const totalValue = monthlyRent * durationMonths;

    // Neuquén: 1.2% of total contract value
    const tax = totalValue * 0.012;

    // Apply exemptions
    if (totalValue < 500000) {
      return tax * 0.5; // 50% reduction for small contracts
    }

    return tax;
  }
}

class RioNegroStampTax implements StampTaxCalculator {
  async calculate(params: TaxParams): Promise<number> {
    const { monthlyRent, durationMonths } = params;
    const totalValue = monthlyRent * durationMonths;

    // Río Negro: 1% of total contract value
    return totalValue * 0.01;
  }
}

// Factory pattern for province-specific calculations
export function getStampTaxCalculator(
  province: string
): StampTaxCalculator {
  switch (province) {
    case 'NEUQUEN':
      return new NeuquenStampTax();
    case 'RIO_NEGRO':
      return new RioNegroStampTax();
    default:
      throw new Error(`Unsupported province: ${province}`);
  }
}


4. Data Architecture
4.1 Data Models
User Entity
model User {
  id            String   @id @default(cuid())
  clerkId       String   @unique
  email         String   @unique
  phone         String?
  role          UserRole

  // Personal Information
  firstName     String
  lastName      String
  dni           String   @unique
  cuit          String?

  // Verification
  verificationStatus VerificationStatus @default(UNVERIFIED)
  verificationDate   DateTime?
  documents          Document[]

  // Trust Score
  trustScore         TrustScore?

  // Relationships
  ownedProperties    Property[]    @relation("PropertyOwner")
  tenantApplications Application[]
  alpVisits          Visit[]       @relation("ALPVisits")
  contracts          Contract[]
  transactions       Transaction[]
  reviews            Review[]

  // Metadata
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  lastActive    DateTime @default(now())

  @@index([role, verificationStatus])
  @@index([email])
}

model TrustScore {
  id        String   @id @default(cuid())
  userId    String   @unique
  user      User     @relation(fields: [userId], references: [id])

  overall   Int      @default(0) // 0-100

  // Components
  identityScore    Int @default(0)
  creditScore      Int @default(0)
  paymentScore     Int @default(0)
  reviewScore      Int @default(0)

  // History
  history          TrustScoreHistory[]
  lastCalculated   DateTime @default(now())

  @@index([overall])
}

Property Entity
model Property {
  id          String   @id @default(cuid())
  ownerId     String
  owner       User     @relation("PropertyOwner", fields: [ownerId], references: [id])
  martilleroId String?
  martillero   User?    @relation(fields: [martilleroId], references: [id])

  // Basic Info
  title       String
  description String   @db.Text
  type        PropertyType
  status      PropertyStatus @default(DRAFT)

  // Location
  address     Address?
  coordinates Json     // { lat: number, lng: number }
  neighborhood String
  city        String
  province    String

  // Specifications
  bedrooms    Int
  bathrooms   Int
  surfaceArea Float    // m²
  floors      Int      @default(1)

  // Pricing
  basePrice      Float
  suggestedPrice Float?
  currency       Currency @default(ARS)
  expenses       Float?   // Monthly expenses

  // Features
  amenities      String[] // Array of amenity codes
  petFriendly    Boolean  @default(false)
  furnished      Boolean  @default(false)
  garage         Boolean  @default(false)

  // Media
  images         PropertyImage[]
  virtualTourUrl String?

  // Availability
  availableFrom  DateTime
  minimumStay    Int?     // months
  maximumStay    Int?     // months

  // Validation
  alpValidated   Boolean  @default(false)
  validationDate DateTime?
  validationReport Json?

  // Analytics
  views         Int      @default(0)
  inquiries     Int      @default(0)
  favorites     Int      @default(0)

  // Relationships
  visits        Visit[]
  applications  Application[]
  contracts     Contract[]

  // Metadata
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  publishedAt   DateTime?

  @@index([status, province, city])
  @@index([type, bedrooms, basePrice])
  @@index([coordinates(ops: GinOps)], type: Gin)
}

model PropertyImage {
  id           String   @id @default(cuid())
  propertyId   String
  property     Property @relation(fields: [propertyId], references: [id])

  url          String
  thumbnailUrl String
  order        Int
  alt          String?

  width        Int?
  height       Int?
  size         Int?     // bytes

  createdAt    DateTime @default(now())

  @@unique([propertyId, order])
}

Visit Entity
model Visit {
  id         String   @id @default(cuid())
  propertyId String
  property   Property @relation(fields: [propertyId], references: [id])
  tenantId   String
  tenant     User     @relation(fields: [tenantId], references: [id])
  alpId      String?
  alp        User?    @relation("ALPVisits", fields: [alpId], references: [id])

  // Scheduling
  requestedDate   DateTime
  scheduledDate   DateTime?
  duration        Int       @default(30) // minutes
  status          VisitStatus @default(REQUESTED)

  // Location
  meetingPoint    String?
  accessNotes     String?

  // Execution
  checkInTime     DateTime?
  checkInLocation Json?     // { lat, lng }
  checkOutTime    DateTime?

  // Report
  report          VisitReport?
  photos          VisitPhoto[]

  // Communication
  messages        Message[]

  // Metadata
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt

  @@index([status, scheduledDate])
  @@index([alpId, scheduledDate])
}

model VisitReport {
  id          String   @id @default(cuid())
  visitId     String   @unique
  visit       Visit    @relation(fields: [visitId], references: [id])

  // Property Condition
  condition   PropertyCondition
  cleanliness Int      // 1-5
  maintenance Int      // 1-5

  // Observations
  notes       String?  @db.Text
  issues      Json[]   // Array of issues with photos

  // Tenant Interaction
  tenantPresent Boolean
  tenantNotes   String?

  submittedAt DateTime @default(now())
}

Contract Entity
model Contract {
  id           String   @id @default(cuid())
  propertyId   String
  property     Property @relation(fields: [propertyId], references: [id])
  ownerId      String
  owner        User     @relation(fields: [ownerId], references: [id])
  tenantId     String
  tenant       User     @relation(fields: [tenantId], references: [id])
  martilleroId String?
  martillero   User?    @relation(fields: [martilleroId], references: [id])

  // Terms
  startDate    DateTime
  endDate      DateTime
  monthlyRent  Float
  depositAmount Float
  currency     Currency @default(ARS)

  // Insurance
  insuranceId  String?
  insurance    Insurance? @relation(fields: [insuranceId], references: [id])

  // Document
  templateId   String
  version      Int      @default(1)
  content      String   @db.Text // HTML content
  pdfUrl       String?

  // Legal
  stampTax     Float
  registrationNumber String?

  // Signatures
  signatures   Signature[]
  signatureStatus SignatureStatus @default(PENDING)

  // Status
  status       ContractStatus @default(DRAFT)

  // Modifications
  modifications ContractModification[]

  // Metadata
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt
  signedAt     DateTime?

  @@index([status, startDate])
}

model Signature {
  id          String   @id @default(cuid())
  contractId  String
  contract    Contract @relation(fields: [contractId], references: [id])
  userId      String
  user        User     @relation(fields: [userId], references: [id])

  role        SignerRole
  signedAt    DateTime?
  ipAddress   String?
  deviceInfo  Json?

  // External signature service
  externalId  String?
  documentHash String

  createdAt   DateTime @default(now())

  @@unique([contractId, userId])
}

Financial Entities
model Transaction {
  id          String   @id @default(cuid())
  type        TransactionType
  status      TransactionStatus @default(PENDING)

  // Parties
  fromUserId  String
  fromUser    User     @relation("TransactionFrom", fields: [fromUserId], references: [id])
  toUserId    String?
  toUser      User?    @relation("TransactionTo", fields: [toUserId], references: [id])

  // Amount
  amount      Float
  currency    Currency @default(ARS)

  // Related entities
  contractId  String?
  contract    Contract? @relation(fields: [contractId], references: [id])
  propertyId  String?
  property    Property? @relation(fields: [propertyId], references: [id])

  // Payment
  paymentMethod String?
  mercadopagoId String?

  // Splits
  parentTransactionId String?
  parentTransaction   Transaction? @relation("TransactionSplits", fields: [parentTransactionId], references: [id])
  splits              Transaction[] @relation("TransactionSplits")

  // Metadata
  metadata    Json?
  createdAt   DateTime @default(now())
  processedAt DateTime?

  @@index([type, status])
  @@index([mercadopagoId])
}

model Insurance {
  id          String   @id @default(cuid())
  tenantId    String
  tenant      User     @relation(fields: [tenantId], references: [id])
  contractId  String?
  contract    Contract?

  // Provider
  provider    String
  policyNumber String?

  // Coverage
  coverageType CoverageType
  coverageAmount Float
  deductible   Float

  // Pricing
  monthlyPremium Float
  paymentMethod  PaymentMethod

  // Status
  status      InsuranceStatus @default(QUOTED)
  startDate   DateTime?
  endDate     DateTime?

  // Claims
  claims      Claim[]

  // Metadata
  quoteData   Json
  policyData  Json?
  createdAt   DateTime @default(now())
  activatedAt DateTime?

  @@index([status, provider])
}

4.2 Data Storage
Database Selection and Rationale
Supabase (PostgreSQL) was chosen for the following reasons:
Real-time Capabilities: Built-in real-time subscriptions for live updates
Row-Level Security: Native RLS for multi-tenant data isolation
Scalability: Managed PostgreSQL with automatic scaling
Geographic Replication: Low latency for Latin American users
Cost-Effective: Generous free tier and predictable pricing
Data Persistence Strategies
Transactional Integrity

 // Using Prisma transactions for financial operations
export async function processRentPayment(
  paymentData: PaymentData
) {
  return await prisma.$transaction(async (tx) => {
    // Create main transaction
    const transaction = await tx.transaction.create({
      data: {
        type: 'RENT',
        amount: paymentData.amount,
        fromUserId: paymentData.tenantId,
        toUserId: paymentData.ownerId,
        status: 'PROCESSING'
      }
    });

    // Update contract payment status
    await tx.contract.update({
      where: { id: paymentData.contractId },
      data: {
        lastPaymentDate: new Date(),
        nextPaymentDue: addMonths(new Date(), 1)
      }
    });

    // Create commission splits
    const splits = await createCommissionSplits(
      tx,
      transaction.id,
      paymentData
    );

    // Update user balances
    await updateUserBalances(tx, transaction, splits);

    return { transaction, splits };
  });
}


Optimistic Locking

 model Property {
  // ... other fields
  version Int @default(0)
}
 export async function updateProperty(
  id: string,
  data: PropertyUpdate,
  expectedVersion: number
) {
  const result = await prisma.property.updateMany({
    where: {
      id,
      version: expectedVersion
    },
    data: {
      ...data,
      version: { increment: 1 }
    }
  });

  if (result.count === 0) {
    throw new Error('Concurrent modification detected');
  }
}


Caching Mechanisms
Redis Cache Layer

 import { Redis } from '@upstash/redis';

const redis = Redis.fromEnv();

export class CacheService {
  private static readonly TTL = {
    USER_PROFILE: 3600,      // 1 hour
    PROPERTY_LIST: 300,      // 5 minutes
    PROPERTY_DETAIL: 600,    // 10 minutes
    TRUST_SCORE: 86400,      // 24 hours
  };

  async getOrSet<T>(
    key: string,
    fetcher: () => Promise<T>,
    ttl: number
  ): Promise<T> {
    // Try cache first
    const cached = await redis.get<T>(key);
    if (cached) return cached;

    // Fetch and cache
    const data = await fetcher();
    await redis.setex(key, ttl, data);

    return data;
  }

  async invalidate(pattern: string) {
    const keys = await redis.keys(pattern);
    if (keys.length > 0) {
      await redis.del(...keys);
    }
  }
}


Query Result Caching

 export async function getPropertyWithCache(id: string) {
  return cacheService.getOrSet(
    `property:${id}`,
    async () => {
      return prisma.property.findUnique({
        where: { id },
        include: {
          images: { orderBy: { order: 'asc' } },
          owner: {
            select: {
              id: true,
              firstName: true,
              lastName: true,
              trustScore: true
            }
          }
        }
      });
    },
    CacheService.TTL.PROPERTY_DETAIL
  );
}


Incremental Static Regeneration

 // In Next.js page
export const revalidate = 60; // Revalidate every minute

export async function generateStaticParams() {
  const properties = await prisma.property.findMany({
    where: { status: 'ACTIVE' },
    select: { id: true },
    take: 100,
    orderBy: { views: 'desc' }
  });

  return properties.map(p => ({ id: p.id }));
}


Backup and Recovery
Automated Backups


Daily automated backups via Supabase
Point-in-time recovery for 7 days (free tier) / 30 days (pro)
Cross-region backup replication
Manual Backup Strategy

 // Scheduled job for critical data export
export async function exportCriticalData() {
  const date = new Date().toISOString().split('T')[0];

  // Export contracts
  const contracts = await prisma.contract.findMany({
    where: { status: 'ACTIVE' },
    include: { signatures: true }
  });

  await uploadToS3(
    `backups/${date}/contracts.json`,
    JSON.stringify(contracts)
  );

  // Export transactions
  const transactions = await prisma.transaction.findMany({
    where: {
      createdAt: { gte: subDays(new Date(), 30) }
    }
  });

  await uploadToS3(
    `backups/${date}/transactions.json`,
    JSON.stringify(transactions)
  );
}


Recovery Procedures

 export async function restoreFromBackup(date: string) {
  // Download backup files
  const contracts = await downloadFromS3(
    `backups/${date}/contracts.json`
  );

  // Validate data integrity
  const validated = validateBackupData(contracts);

  // Restore using transactions
  await prisma.$transaction(async (tx) => {
    for (const contract of validated) {
      await tx.contract.upsert({
        where: { id: contract.id },
        create: contract,
        update: contract
      });
    }
  });
}


5. API Specifications
5.1 Internal APIs (Server Actions)
User Management APIs
// server/actions/users.ts
'use server';

import { z } from 'zod';
import { auth } from '@clerk/nextjs';

// Schema definitions
const RegisterUserSchema = z.object({
  email: z.string().email(),
  phone: z.string().regex(/^\+54\d{10}$/),
  role: z.enum(['OWNER', 'TENANT', 'MARTILLERO', 'ALP']),
  firstName: z.string().min(2),
  lastName: z.string().min(2),
  dni: z.string().regex(/^\d{7,8}$/)
});

/**
 * Register a new user
 * @param data User registration data
 * @returns {Promise<{success: boolean, userId?: string, error?: string}>}
 */
export async function registerUser(data: z.infer<typeof RegisterUserSchema>) {
  try {
    const validated = RegisterUserSchema.parse(data);

    // Create Clerk user
    const clerkUser = await clerk.users.createUser({
      emailAddress: [validated.email],
      phoneNumber: [validated.phone],
      firstName: validated.firstName,
      lastName: validated.lastName,
      publicMetadata: {
        role: validated.role,
        dni: validated.dni
      }
    });

    // Create database record
    const user = await prisma.user.create({
      data: {
        clerkId: clerkUser.id,
        ...validated,
        verificationStatus: 'UNVERIFIED'
      }
    });

    // Trigger async verification
    await triggerVerification(user.id);

    return { success: true, userId: user.id };
  } catch (error) {
    return {
      success: false,
      error: error instanceof Error ? error.message : 'Registration failed'
    };
  }
}

/**
 * Update user trust score
 * @param userId User ID
 * @param component Trust score component to update
 * @param value New value
 * @requires Admin role
 */
export async function updateTrustScore(
  userId: string,
  component: 'identity' | 'credit' | 'payment' | 'review',
  value: number
) {
  const { userId: adminId } = auth();

  // Check admin permissions
  const admin = await prisma.user.findUnique({
    where: { clerkId: adminId }
  });

  if (admin?.role !== 'ADMIN') {
    throw new Error('Unauthorized');
  }

  // Update component and recalculate overall
  const trustScore = await prisma.trustScore.update({
    where: { userId },
    data: {
      [`${component}Score`]: value,
      overall: {
        // Recalculate based on weighted average
      }
    }
  });

  return { success: true, trustScore };
}

Property Management APIs
// server/actions/properties.ts
'use server';

const CreatePropertySchema = z.object({
  title: z.string().min(10).max(100),
  description: z.string().min(50).max(2000),
  type: z.enum(['HOUSE', 'APARTMENT', 'STUDIO']),
  bedrooms: z.number().min(0).max(10),
  bathrooms: z.number().min(1).max(5),
  surfaceArea: z.number().min(20).max(1000),
  address: z.object({
    street: z.string(),
    number: z.string(),
    city: z.string(),
    province: z.string(),
    postalCode: z.string()
  }),
  basePrice: z.number().positive(),
  amenities: z.array(z.string()),
  petFriendly: z.boolean(),
  furnished: z.boolean()
});

/**
 * Create a new property listing
 * @param data Property data
 * @param images FormData containing property images
 * @returns {Promise<{success: boolean, propertyId?: string, error?: string}>}
 */
export async function createProperty(
  data: z.infer<typeof CreatePropertySchema>,
  images: FormData
) {
  const { userId } = auth();
  if (!userId) throw new Error('Unauthorized');

  const user = await prisma.user.findUnique({
    where: { clerkId: userId }
  });

  if (!user || !['OWNER', 'MARTILLERO'].includes(user.role)) {
    throw new Error('Only owners and martilleros can create properties');
  }

  try {
    const validated = CreatePropertySchema.parse(data);

    // Geocode address
    const coordinates = await geocodeAddress(validated.address);

    // Start transaction
    const result = await prisma.$transaction(async (tx) => {
      // Create property
      const property = await tx.property.create({
        data: {
          ...validated,
          ownerId: user.id,
          coordinates,
          status: 'DRAFT'
        }
      });

      // Process and upload images
      const imageUrls = await processPropertyImages(
        property.id,
        images
      );

      // Create image records
      await tx.propertyImage.createMany({
        data: imageUrls.map((img, index) => ({
          propertyId: property.id,
          url: img.url,
          thumbnailUrl: img.thumbnailUrl,
          order: index
        }))
      });

      return property;
    });

    // Get AI pricing suggestion (async)
    getPricingSuggestion(result.id).then(price => {
      prisma.property.update({
        where: { id: result.id },
        data: { suggestedPrice: price }
      });
    });

    return { success: true, propertyId: result.id };
  } catch (error) {
    return {
      success: false,
      error: error instanceof Error ? error.message : 'Failed to create property'
    };
  }
}

/**
 * Search properties with filters
 * @param filters Search filters
 * @param pagination Pagination options
 */
export async function searchProperties(
  filters: PropertyFilters,
  pagination: { page: number; limit: number }
) {
  const where: Prisma.PropertyWhereInput = {
    status: 'ACTIVE',
    ...(filters.type && { type: filters.type }),
    ...(filters.minPrice && { basePrice: { gte: filters.minPrice } }),
    ...(filters.maxPrice && { basePrice: { lte: filters.maxPrice } }),
    ...(filters.bedrooms && { bedrooms: { gte: filters.bedrooms } }),
    ...(filters.neighborhood && { neighborhood: filters.neighborhood }),
    ...(filters.amenities && {
      amenities: { hasEvery: filters.amenities }
    })
  };

  // If location search, use PostGIS
  if (filters.location) {
    where.AND = {
      coordinates: {
        path: ['lat'],
        gte: filters.location.bounds.south,
        lte: filters.location.bounds.north
      }
    };
  }

  const [properties, total] = await prisma.$transaction([
    prisma.property.findMany({
      where,
      include: {
        images: {
          take: 1,
          orderBy: { order: 'asc' }
        },
        owner: {
          select: {
            firstName: true,
            trustScore: { select: { overall: true } }
          }
        }
      },
      skip: pagination.page * pagination.limit,
      take: pagination.limit,
      orderBy: filters.sortBy || { createdAt: 'desc' }
    }),
    prisma.property.count({ where })
  ]);

  return {
    properties,
    pagination: {
      ...pagination,
      total,
      pages: Math.ceil(total / pagination.limit)
    }
  };
}

Visit Coordination APIs
// server/actions/visits.ts
'use server';

/**
 * Schedule a property visit
 * @param propertyId Property to visit
 * @param requestedDates Array of preferred dates/times
 */
export async function scheduleVisit(
  propertyId: string,
  requestedDates: Date[]
) {
  const { userId } = auth();
  if (!userId) throw new Error('Unauthorized');

  const user = await prisma.user.findUnique({
    where: { clerkId: userId }
  });

  if (user?.role !== 'TENANT') {
    throw new Error('Only tenants can schedule visits');
  }

  // Create visit request
  const visit = await prisma.visit.create({
    data: {
      propertyId,
      tenantId: user.id,
      requestedDate: requestedDates[0],
      alternativeDates: requestedDates.slice(1),
      status: 'REQUESTED'
    }
  });

  // Trigger ALP assignment
  await assignALPToVisit(visit.id);

  // Send notifications
  await notifyPropertyOwner(visit.id);

  return { success: true, visitId: visit.id };
}

/**
 * Assign an ALP to a visit
 * @param visitId Visit ID
 * @internal
 */
async function assignALPToVisit(visitId: string) {
  const visit = await prisma.visit.findUnique({
    where: { id: visitId },
    include: { property: true }
  });

  if (!visit) throw new Error('Visit not found');

  // Find available ALPs
  const alps = await prisma.user.findMany({
    where: {
      role: 'ALP',
      verificationStatus: 'VERIFIED',
      availability: {
        some: {
          date: visit.requestedDate,
          isAvailable: true
        }
      }
    },
    include: {
      location: true,
      alpStats: true
    }
  });

  // Score and rank ALPs
  const scoredAlps = alps.map(alp => ({
    alp,
    score: calculateALPScore({
      distance: getDistance(
        alp.location,
        visit.property.coordinates
      ),
      rating: alp.alpStats.averageRating,
      completedVisits: alp.alpStats.totalVisits,
      responseRate: alp.alpStats.responseRate
    })
  }));

  // Sort by score
  scoredAlps.sort((a, b) => b.score - a.score);

  if (scoredAlps.length === 0) {
    throw new Error('No ALPs available');
  }

  // Assign top ALP
  await prisma.visit.update({
    where: { id: visitId },
    data: {
      alpId: scoredAlps[0].alp.id,
      scheduledDate: visit.requestedDate,
      status: 'SCHEDULED'
    }
  });

  // Send notification to ALP
  await sendALPNotification(scoredAlps[0].alp.id, visit);
}

/**
 * Submit visit report (ALP only)
 * @param visitId Visit ID
 * @param report Visit report data
 */
export async function submitVisitReport(
  visitId: string,
  report: VisitReportInput
) {
  const { userId } = auth();
  const user = await prisma.user.findUnique({
    where: { clerkId: userId }
  });

  // Verify ALP and visit assignment
  const visit = await prisma.visit.findUnique({
    where: {
      id: visitId,
      alpId: user?.id
    }
  });

  if (!visit) {
    throw new Error('Unauthorized or visit not found');
  }

  // Create report
  const visitReport = await prisma.visitReport.create({
    data: {
      visitId,
      ...report,
      submittedAt: new Date()
    }
  });

  // Update visit status
  await prisma.visit.update({
    where: { id: visitId },
    data: {
      status: 'COMPLETED',
      checkOutTime: new Date()
    }
  });

  // Update ALP stats
  await updateALPStats(user.id, 'COMPLETED');

  // Notify tenant and owner
  await notifyVisitCompleted(visitId);

  return { success: true, reportId: visitReport.id };
}

Contract & Financial APIs
// server/actions/contracts.ts
'use server';

/**
 * Generate a rental contract
 * @param data Contract data
 */
export async function generateContract(
  data: ContractGenerationInput
) {
  const { userId } = auth();

  // Validate all parties
  const [property, owner, tenant] = await Promise.all([
    prisma.property.findUnique({
      where: { id: data.propertyId },
      include: { owner: true }
    }),
    prisma.user.findUnique({ where: { id: data.ownerId } }),
    prisma.user.findUnique({ where: { id: data.tenantId } })
  ]);

  // Verify authorization
  if (!canCreateContract(userId, property, owner)) {
    throw new Error('Unauthorized to create contract');
  }

  // Calculate stamp tax
  const stampTax = await calculateStampTax({
    province: property.province,
    monthlyRent: data.monthlyRent,
    duration: data.durationMonths
  });

  // Generate contract content
  const content = await generateContractContent({
    template: data.templateId,
    property,
    owner,
    tenant,
    terms: data,
    stampTax
  });

  // Create contract record
  const contract = await prisma.contract.create({
    data: {
      propertyId: data.propertyId,
      ownerId: data.ownerId,
      tenantId: data.tenantId,
      martilleroId: data.martilleroId,
      startDate: data.startDate,
      endDate: data.endDate,
      monthlyRent: data.monthlyRent,
      depositAmount: data.depositAmount,
      templateId: data.templateId,
      content,
      stampTax,
      status: 'DRAFT'
    }
  });

  // Initialize signatures
  await initializeSignatures(contract.id, [
    { userId: owner.id, role: 'OWNER' },
    { userId: tenant.id, role: 'TENANT' },
    ...(data.martilleroId ? [{
      userId: data.martilleroId,
      role: 'MARTILLERO'
    }] : [])
  ]);

  return { success: true, contractId: contract.id };
}

/**
 * Process rent payment
 * @param contractId Contract ID
 * @param paymentData Payment information
 */
export async function processRentPayment(
  contractId: string,
  paymentData: PaymentInput
) {
  const { userId } = auth();

  const contract = await prisma.contract.findUnique({
    where: { id: contractId },
    include: {
      tenant: true,
      owner: true,
      property: { include: { martillero: true } }
    }
  });

  if (!contract || contract.tenant.clerkId !== userId) {
    throw new Error('Unauthorized');
  }

  // Create MercadoPago preference
  const preference = await createPaymentPreference({
    amount: contract.monthlyRent,
    description: `Alquiler ${contract.property.title}`,
    payer: {
      email: contract.tenant.email,
      identification: {
        type: 'DNI',
        number: contract.tenant.dni
      }
    },
    metadata: {
      contractId,
      type: 'RENT',
      month: paymentData.month
    }
  });

  // Create pending transaction
  const transaction = await prisma.transaction.create({
    data: {
      type: 'RENT',
      amount: contract.monthlyRent,
      fromUserId: contract.tenantId,
      toUserId: contract.ownerId,
      contractId,
      status: 'PENDING',
      mercadopagoId: preference.id
    }
  });

  return {
    success: true,
    paymentUrl: preference.init_point,
    transactionId: transaction.id
  };
}

/**
 * Handle payment webhook from MercadoPago
 * @param notification Webhook notification
 * @internal
 */
export async function handlePaymentWebhook(
  notification: MercadoPagoNotification
) {
  // Verify webhook signature
  if (!verifyWebhookSignature(notification)) {
    throw new Error('Invalid webhook signature');
  }

  if (notification.type === 'payment') {
    const payment = await mercadopago.payment.findById(
      notification.data.id
    );

    if (payment.status === 'approved') {
      const transaction = await prisma.transaction.findUnique({
        where: { mercadopagoId: payment.external_reference }
      });

      if (!transaction) {
        throw new Error('Transaction not found');
      }

      // Update transaction status
      await prisma.transaction.update({
        where: { id: transaction.id },
        data: {
          status: 'COMPLETED',
          processedAt: new Date()
        }
      });

      // Process commission splits
      await processCommissionSplits(transaction.id);

      // Update contract payment status
      await prisma.contract.update({
        where: { id: transaction.contractId },
        data: {
          lastPaymentDate: new Date(),
          nextPaymentDue: addMonths(new Date(), 1)
        }
      });

      // Send payment confirmations
      await sendPaymentConfirmations(transaction.id);
    }
  }
}

5.2 External Integrations
BCRA/Veraz Credit Check Integration
// lib/api/credit-check.ts

interface CreditCheckProvider {
  checkCredit(params: CreditCheckParams): Promise<CreditCheckResult>;
}

class BCRAClient implements CreditCheckProvider {
  private apiKey: string;
  private baseUrl = 'https://api.bcra.gob.ar/v1';

  constructor() {
    this.apiKey = process.env.BCRA_API_KEY!;
  }

  async checkCredit(params: CreditCheckParams): Promise<CreditCheckResult> {
    const response = await fetch(`${this.baseUrl}/credit-check`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        cuit: params.cuit,
        period: 'last-12-months'
      })
    });

    if (!response.ok) {
      throw new Error(`BCRA API error: ${response.status}`);
    }

    const data = await response.json();

    return {
      score: this.calculateScore(data),
      debts: data.debts || [],
      creditHistory: data.history || [],
      lastChecked: new Date()
    };
  }

  private calculateScore(data: any): number {
    // Score calculation based on BCRA data
    let score = 100;

    // Deduct for active debts
    score -= (data.activeDebts || 0) * 5;

    // Deduct for payment delays
    score -= (data.delays || 0) * 10;

    // Minimum score of 0
    return Math.max(0, score);
  }
}

class VerazClient implements CreditCheckProvider {
  private apiKey: string;
  private baseUrl = 'https://api.veraz.com.ar/v2';

  async checkCredit(params: CreditCheckParams): Promise<CreditCheckResult> {
    const response = await fetch(`${this.baseUrl}/score`, {
      method: 'POST',
      headers: {
        'X-API-Key': this.apiKey,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        documentType: 'DNI',
        documentNumber: params.dni,
        includeDetails: true
      })
    });

    const data = await response.json();

    return {
      score: data.score,
      riskLevel: this.mapRiskLevel(data.score),
      factors: data.scoringFactors,
      lastChecked: new Date()
    };
  }

  private mapRiskLevel(score: number): 'LOW' | 'MEDIUM' | 'HIGH' {
    if (score >= 700) return 'LOW';
    if (score >= 500) return 'MEDIUM';
    return 'HIGH';
  }
}

// Aggregator service
export class CreditCheckService {
  private providers: CreditCheckProvider[];

  constructor() {
    this.providers = [
      new BCRAClient(),
      new VerazClient()
    ];
  }

  async performComprehensiveCheck(
    userId: string,
    params: CreditCheckParams
  ): Promise<ComprehensiveCreditResult> {
    // Run checks in parallel
    const results = await Promise.allSettled(
      this.providers.map(provider => provider.checkCredit(params))
    );

    // Aggregate results
    const successfulChecks = results
      .filter(r => r.status === 'fulfilled')
      .map(r => (r as PromiseFulfilledResult<CreditCheckResult>).value);

    // Calculate combined score
    const combinedScore = this.calculateCombinedScore(successfulChecks);

    // Store result
    await prisma.creditCheck.create({
      data: {
        userId,
        providers: successfulChecks.map(r => r.provider),
        score: combinedScore,
        rawData: successfulChecks,
        performedAt: new Date()
      }
    });

    return {
      overallScore: combinedScore,
      individualScores: successfulChecks,
      recommendation: this.getRecommendation(combinedScore)
    };
  }
}

MercadoPago Payment Integration
// lib/api/mercadopago.ts

import { MercadoPagoConfig, Preference, Payment } from 'mercadopago';

const client = new MercadoPagoConfig({
  accessToken: process.env.MERCADOPAGO_ACCESS_TOKEN!
});

export class MercadoPagoService {
  private preference: Preference;
  private payment: Payment;

  constructor() {
    this.preference = new Preference(client);
    this.payment = new Payment(client);
  }

  async createPaymentPreference(data: PaymentPreferenceData) {
    const preference = await this.preference.create({
      body: {
        items: [{
          id: data.id,
          title: data.title,
          quantity: 1,
          unit_price: data.amount,
          currency_id: 'ARS'
        }],
        payer: {
          name: data.payer.name,
          email: data.payer.email,
          identification: {
            type: data.payer.identificationType,
            number: data.payer.identificationNumber
          }
        },
        payment_methods: {
          excluded_payment_types: [
            { id: 'ticket' } // Exclude cash payments
          ],
          installments: data.allowInstallments ? 12 : 1
        },
        statement_descriptor: 'NEO-RENT',
        external_reference: data.externalReference,
        notification_url: `${process.env.NEXT_PUBLIC_URL}/api/webhooks/mercadopago`,
        back_urls: {
          success: `${process.env.NEXT_PUBLIC_URL}/payments/success`,
          failure: `${process.env.NEXT_PUBLIC_URL}/payments/failure`,
          pending: `${process.env.NEXT_PUBLIC_URL}/payments/pending`
        },
        auto_return: 'approved',
        binary_mode: true // Immediate approval/rejection
      }
    });

    return {
      id: preference.id!,
      init_point: preference.init_point!,
      sandbox_init_point: preference.sandbox_init_point!
    };
  }

  async getPayment(paymentId: string) {
    const payment = await this.payment.get({ id: paymentId });
    return payment;
  }

  async refundPayment(paymentId: string, amount?: number) {
    const refund = await this.payment.refund({
      id: paymentId,
      body: amount ? { amount } : {}
    });

    return refund;
  }

  verifyWebhookSignature(
    headers: Record<string, string>,
    body: any
  ): boolean {
    const xSignature = headers['x-signature'];
    const xRequestId = headers['x-request-id'];

    if (!xSignature || !xRequestId) return false;

    // Parse signature
    const parts = xSignature.split(',');
    const ts = parts.find(p => p.startsWith('ts='))?.split('=')[1];
    const v1 = parts.find(p => p.startsWith('v1='))?.split('=')[1];

    // Recreate signature
    const manifest = `id:${body.data.id};request-id:${xRequestId};ts:${ts};`;
    const hmac = crypto.createHmac('sha256', process.env.MERCADOPAGO_WEBHOOK_SECRET!);
    hmac.update(manifest);
    const signature = hmac.digest('hex');

    return signature === v1;
  }
}

// Webhook handler
export async function POST(request: Request) {
  const body = await request.json();
  const headers = Object.fromEntries(request.headers);

  const mercadoPago = new MercadoPagoService();

  // Verify signature
  if (!mercadoPago.verifyWebhookSignature(headers, body)) {
    return new Response('Invalid signature', { status: 401 });
  }

  // Process notification
  if (body.type === 'payment') {
    await processPaymentNotification(body.data.id);
  }

  return new Response('OK', { status: 200 });
}

WhatsApp Business API Integration
// lib/api/whatsapp.ts

interface WhatsAppMessage {
  to: string;
  type: 'text' | 'template' | 'document';
  content: any;
}

export class WhatsAppService {
  private apiUrl = 'https://graph.facebook.com/v17.0';
  private phoneNumberId: string;
  private accessToken: string;

  constructor() {
    this.phoneNumberId = process.env.WHATSAPP_PHONE_NUMBER_ID!;
    this.accessToken = process.env.WHATSAPP_ACCESS_TOKEN!;
  }

  async sendMessage(message: WhatsAppMessage) {
    const response = await fetch(
      `${this.apiUrl}/${this.phoneNumberId}/messages`,
      {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${this.accessToken}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          messaging_product: 'whatsapp',
          to: message.to,
          type: message.type,
          ...this.formatMessage(message)
        })
      }
    );

    if (!response.ok) {
      throw new Error(`WhatsApp API error: ${response.status}`);
    }

    return response.json();
  }

  async sendVisitReminder(visit: VisitWithDetails) {
    return this.sendMessage({
      to: visit.tenant.phone,
      type: 'template',
      content: {
        name: 'visit_reminder',
        language: { code: 'es_AR' },
        components: [{
          type: 'body',
          parameters: [
            { type: 'text', text: visit.property.title },
            { type: 'text', text: formatDate(visit.scheduledDate) },
            { type: 'text', text: visit.alp.firstName }
          ]
        }]
      }
    });
  }

  async sendPaymentConfirmation(
    transaction: TransactionWithDetails
  ) {
    return this.sendMessage({
      to: transaction.fromUser.phone,
      type: 'template',
      content: {
        name: 'payment_confirmation',
        language: { code: 'es_AR' },
        components: [{
          type: 'body',
          parameters: [
            { type: 'currency', currency: {
              fallback_value: `$${transaction.amount}`,
              code: 'ARS',
              amount_1000: transaction.amount * 1000
            }},
            { type: 'text', text: transaction.contract.property.title }
          ]
        }]
      }
    });
  }

  private formatMessage(message: WhatsAppMessage) {
    switch (message.type) {
      case 'text':
        return { text: { body: message.content } };

      case 'template':
        return { template: message.content };

      case 'document':
        return {
          document: {
            link: message.content.url,
            filename: message.content.filename,
            caption: message.content.caption
          }
        };

      default:
        throw new Error(`Unsupported message type: ${message.type}`);
    }
  }
}

// Notification service
export class NotificationService {
  private whatsapp: WhatsAppService;
  private email: EmailService;

  constructor() {
    this.whatsapp = new WhatsAppService();
    this.email = new EmailService();
  }

  async notifyUser(
    userId: string,
    notification: NotificationData
  ) {
    const user = await prisma.user.findUnique({
      where: { id: userId },
      include: { preferences: true }
    });

    if (!user) throw new Error('User not found');

    // Check notification preferences
    const methods = [];

    if (user.preferences.whatsapp && user.phone) {
      methods.push(this.whatsapp.sendMessage({
        to: user.phone,
        type: notification.type,
        content: notification.content
      }));
    }

    if (user.preferences.email) {
      methods.push(this.email.send({
        to: user.email,
        subject: notification.subject,
        template: notification.template,
        data: notification.data
      }));
    }

    // Send via all preferred methods
    await Promise.allSettled(methods);

    // Log notification
    await prisma.notification.create({
      data: {
        userId,
        type: notification.type,
        channels: methods.map(m => m.constructor.name),
        content: notification.content,
        sentAt: new Date()
      }
    });
  }
}

6. Security & Privacy
6.1 Authentication & Authorization
Authentication Mechanism and Flow
NEO-RENT uses Clerk for authentication with custom role-based access control:
// middleware.ts
import { authMiddleware } from '@clerk/nextjs';
import { NextResponse } from 'next/server';

export default authMiddleware({
  publicRoutes: ['/', '/properties', '/about', '/api/webhooks(.*)'],

  afterAuth(auth, req) {
    // Handle unauthenticated users
    if (!auth.userId && !auth.isPublicRoute) {
      const signInUrl = new URL('/sign-in', req.url);
      signInUrl.searchParams.set('redirect_url', req.url);
      return NextResponse.redirect(signInUrl);
    }

    // Handle role-based routing
    if (auth.userId && req.nextUrl.pathname.startsWith('/admin')) {
      const user = await getUserRole(auth.userId);
      if (user.role !== 'ADMIN') {
        return NextResponse.redirect(new URL('/unauthorized', req.url));
      }
    }

    // Handle onboarding flow
    if (auth.userId && !auth.sessionClaims?.onboarded) {
      const onboardingUrl = new URL('/onboarding', req.url);
      if (!req.url.includes('/onboarding')) {
        return NextResponse.redirect(onboardingUrl);
      }
    }
  }
});

export const config = {
  matcher: ['/((?!.+\\.[\\w]+$|_next).*)', '/', '/(api|trpc)(.*)'],
};

Authorization Strategies and Role Definitions
// lib/auth/roles.ts

export enum Permission {
  // Property permissions
  PROPERTY_CREATE = 'property:create',
  PROPERTY_EDIT_OWN = 'property:edit:own',
  PROPERTY_EDIT_ANY = 'property:edit:any',
  PROPERTY_DELETE_OWN = 'property:delete:own',
  PROPERTY_DELETE_ANY = 'property:delete:any',
  PROPERTY_VALIDATE = 'property:validate',

  // User permissions
  USER_VIEW_OWN = 'user:view:own',
  USER_VIEW_ANY = 'user:view:any',
  USER_EDIT_OWN = 'user:edit:own',
  USER_EDIT_ANY = 'user:edit:any',

  // Visit permissions
  VISIT_SCHEDULE = 'visit:schedule',
  VISIT_MANAGE_OWN = 'visit:manage:own',
  VISIT_MANAGE_ANY = 'visit:manage:any',
  VISIT_EXECUTE = 'visit:execute',

  // Contract permissions
  CONTRACT_CREATE = 'contract:create',
  CONTRACT_VIEW_OWN = 'contract:view:own',
  CONTRACT_VIEW_ANY = 'contract:view:any',
  CONTRACT_SIGN = 'contract:sign',
  CONTRACT_APPROVE = 'contract:approve',

  // Financial permissions
  PAYMENT_MAKE = 'payment:make',
  PAYMENT_RECEIVE = 'payment:receive',
  PAYMENT_VIEW_OWN = 'payment:view:own',
  PAYMENT_VIEW_ANY = 'payment:view:any',
}

export const ROLE_PERMISSIONS: Record<UserRole, Permission[]> = {
  OWNER: [
    Permission.PROPERTY_CREATE,
    Permission.PROPERTY_EDIT_OWN,
    Permission.PROPERTY_DELETE_OWN,
    Permission.USER_VIEW_OWN,
    Permission.USER_EDIT_OWN,
    Permission.VISIT_MANAGE_OWN,
    Permission.CONTRACT_CREATE,
    Permission.CONTRACT_VIEW_OWN,
    Permission.CONTRACT_SIGN,
    Permission.PAYMENT_RECEIVE,
    Permission.PAYMENT_VIEW_OWN,
  ],

  TENANT: [
    Permission.USER_VIEW_OWN,
    Permission.USER_EDIT_OWN,
    Permission.VISIT_SCHEDULE,
    Permission.CONTRACT_VIEW_OWN,
    Permission.CONTRACT_SIGN,
    Permission.PAYMENT_MAKE,
    Permission.PAYMENT_VIEW_OWN,
  ],

  MARTILLERO: [
    Permission.PROPERTY_CREATE,
    Permission.PROPERTY_EDIT_ANY,
    Permission.USER_VIEW_ANY,
    Permission.VISIT_MANAGE_ANY,
    Permission.CONTRACT_CREATE,
    Permission.CONTRACT_VIEW_ANY,
    Permission.CONTRACT_APPROVE,
    Permission.PAYMENT_VIEW_ANY,
  ],

  ALP: [
    Permission.USER_VIEW_OWN,
    Permission.USER_EDIT_OWN,
    Permission.VISIT_EXECUTE,
    Permission.PROPERTY_VALIDATE,
    Permission.PAYMENT_VIEW_OWN,
  ],

  ADMIN: [
    // Admins have all permissions
    ...Object.values(Permission)
  ]
};

// Authorization hook
export function useAuthorization() {
  const { user } = useUser();

  const hasPermission = useCallback((permission: Permission): boolean => {
    if (!user) return false;

    const userRole = user.publicMetadata.role as UserRole;
    return ROLE_PERMISSIONS[userRole]?.includes(permission) ?? false;
  }, [user]);

  const hasAnyPermission = useCallback((permissions: Permission[]): boolean => {
    return permissions.some(permission => hasPermission(permission));
  }, [hasPermission]);

  const hasAllPermissions = useCallback((permissions: Permission[]): boolean => {
    return permissions.every(permission => hasPermission(permission));
  }, [hasPermission]);

  return { hasPermission, hasAnyPermission, hasAllPermissions };
}

// Server-side authorization
export async function authorize(
  userId: string,
  permission: Permission,
  resourceId?: string
): Promise<boolean> {
  const user = await prisma.user.findUnique({
    where: { clerkId: userId }
  });

  if (!user) return false;

  // Check basic role permission
  if (!ROLE_PERMISSIONS[user.role].includes(permission)) {
    return false;
  }

  // Check resource ownership if needed
  if (resourceId && permission.includes(':own')) {
    return await checkResourceOwnership(user.id, permission, resourceId);
  }

  return true;
}

Session Management
// lib/auth/session.ts

export interface SessionData {
  userId: string;
  role: UserRole;
  permissions: Permission[];
  trustScore: number;
  lastActivity: Date;
}

export class SessionManager {
  private static readonly SESSION_DURATION = 30 * 60 * 1000; // 30 minutes
  private static readonly REFRESH_THRESHOLD = 5 * 60 * 1000; // 5 minutes

  static async createSession(userId: string): Promise<SessionData> {
    const user = await prisma.user.findUnique({
      where: { clerkId: userId },
      include: { trustScore: true }
    });

    if (!user) throw new Error('User not found');

    const session: SessionData = {
      userId: user.id,
      role: user.role,
      permissions: ROLE_PERMISSIONS[user.role],
      trustScore: user.trustScore?.overall || 0,
      lastActivity: new Date()
    };

    // Store in Redis
    await redis.setex(
      `session:${userId}`,
      this.SESSION_DURATION / 1000,
      JSON.stringify(session)
    );

    return session;
  }

  static async refreshSession(userId: string): Promise<SessionData | null> {
    const sessionKey = `session:${userId}`;
    const sessionData = await redis.get(sessionKey);

    if (!sessionData) return null;

    const session: SessionData = JSON.parse(sessionData);

    // Check if refresh is needed
    const timeSinceActivity = Date.now() - new Date(session.lastActivity).getTime();

    if (timeSinceActivity > this.REFRESH_THRESHOLD) {
      session.lastActivity = new Date();

      // Extend session
      await redis.setex(
        sessionKey,
        this.SESSION_DURATION / 1000,
        JSON.stringify(session)
      );
    }

    return session;
  }

  static async invalidateSession(userId: string): Promise<void> {
    await redis.del(`session:${userId}`);
  }
}

Token Handling and Refresh
// lib/auth/tokens.ts

interface APIToken {
  id: string;
  userId: string;
  name: string;
  token: string;
  permissions: Permission[];
  expiresAt?: Date;
  lastUsed?: Date;
}

export class APITokenManager {
  static async generateToken(
    userId: string,
    name: string,
    permissions: Permission[],
    expiresIn?: number
  ): Promise<APIToken> {
    // Generate secure token
    const token = this.generateSecureToken();

    // Hash token for storage
    const hashedToken = await bcrypt.hash(token, 10);

    // Create token record
    const apiToken = await prisma.apiToken.create({
      data: {
        userId,
        name,
        token: hashedToken,
        permissions,
        expiresAt: expiresIn
          ? new Date(Date.now() + expiresIn)
          : null
      }
    });

    return {
      ...apiToken,
      token // Return unhashed token to user once
    };
  }

  static async validateToken(token: string): Promise<APIToken | null> {
    // Get all active tokens (this could be optimized with a bloom filter)
    const tokens = await prisma.apiToken.findMany({
      where: {
        OR: [
          { expiresAt: null },
          { expiresAt: { gt: new Date() } }
        ]
      }
    });

    // Check each token
    for (const dbToken of tokens) {
      const isValid = await bcrypt.compare(token, dbToken.token);

      if (isValid) {
        // Update last used
        await prisma.apiToken.update({
          where: { id: dbToken.id },
          data: { lastUsed: new Date() }
        });

        return dbToken;
      }
    }

    return null;
  }

  private static generateSecureToken(): string {
    return crypto.randomBytes(32).toString('base64url');
  }
}

// API Route middleware
export async function withAPIAuth(
  handler: (req: Request, token: APIToken) => Promise<Response>
) {
  return async (req: Request) => {
    const authHeader = req.headers.get('Authorization');

    if (!authHeader?.startsWith('Bearer ')) {
      return new Response('Unauthorized', { status: 401 });
    }

    const token = authHeader.substring(7);
    const apiToken = await APITokenManager.validateToken(token);

    if (!apiToken) {
      return new Response('Invalid token', { status: 401 });
    }

    return handler(req, apiToken);
  };
}

6.2 Data Security
Encryption Strategies
// lib/security/encryption.ts

export class EncryptionService {
  private static algorithm = 'aes-256-gcm';
  private static keyDerivationIterations = 100000;

  /**
   * Encrypt sensitive data at rest
   */
  static async encryptData(
    plaintext: string,
    context?: string
  ): Promise<EncryptedData> {
    // Generate random IV
    const iv = crypto.randomBytes(16);

    // Derive key from master key and context
    const key = await this.deriveKey(
      process.env.MASTER_ENCRYPTION_KEY!,
      context
    );

    // Create cipher
    const cipher = crypto.createCipheriv(this.algorithm, key, iv);

    // Encrypt data
    let encrypted = cipher.update(plaintext, 'utf8', 'base64');
    encrypted += cipher.final('base64');

    // Get auth tag
    const authTag = cipher.getAuthTag();

    return {
      encrypted,
      iv: iv.toString('base64'),
      authTag: authTag.toString('base64'),
      algorithm: this.algorithm,
      context
    };
  }

  /**
   * Decrypt data
   */
  static async decryptData(
    encryptedData: EncryptedData
  ): Promise<string> {
    // Derive key
    const key = await this.deriveKey(
      process.env.MASTER_ENCRYPTION_KEY!,
      encryptedData.context
    );

    // Create decipher
    const decipher = crypto.createDecipheriv(
      encryptedData.algorithm,
      key,
      Buffer.from(encryptedData.iv, 'base64')
    );

    // Set auth tag
    decipher.setAuthTag(
      Buffer.from(encryptedData.authTag, 'base64')
    );

    // Decrypt
    let decrypted = decipher.update(
      encryptedData.encrypted,
      'base64',
      'utf8'
    );
    decrypted += decipher.final('utf8');

    return decrypted;
  }

  /**
   * Derive encryption key from master key
   */
  private static async deriveKey(
    masterKey: string,
    context?: string
  ): Promise<Buffer> {
    const salt = context
      ? crypto.createHash('sha256').update(context).digest()
      : Buffer.from('neo-rent-default-salt');

    return crypto.pbkdf2Sync(
      masterKey,
      salt,
      this.keyDerivationIterations,
      32,
      'sha256'
    );
  }

  /**
   * Encrypt file for storage
   */
  static async encryptFile(
    file: Buffer,
    metadata: FileMetadata
  ): Promise<EncryptedFile> {
    // Generate file key
    const fileKey = crypto.randomBytes(32);

    // Encrypt file
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv(this.algorithm, fileKey, iv);

    const encrypted = Buffer.concat([
      cipher.update(file),
      cipher.final()
    ]);

    const authTag = cipher.getAuthTag();

    // Encrypt file key with user key
    const encryptedKey = await this.encryptData(
      fileKey.toString('base64'),
      `file:${metadata.userId}`
    );

    return {
      encrypted,
      encryptedKey,
      iv: iv.toString('base64'),
      authTag: authTag.toString('base64'),
      metadata
    };
  }
}

// Database field encryption
export function EncryptedField(context?: string) {
  return (target: any, propertyName: string) => {
    let value: string;

    const getter = () => {
      return value;
    };

    const setter = async (newValue: string) => {
      if (newValue) {
        const encrypted = await EncryptionService.encryptData(
          newValue,
          context || `${target.constructor.name}.${propertyName}`
        );
        value = JSON.stringify(encrypted);
      } else {
        value = newValue;
      }
    };

    Object.defineProperty(target, propertyName, {
      get: getter,
      set: setter,
      enumerable: true,
      configurable: true
    });
  };
}

PII Handling and Protection
// lib/security/pii.ts

export interface PIIField {
  field: string;
  type: 'dni' | 'cuit' | 'phone' | 'email' | 'address';
  encrypted: boolean;
  hashed?: boolean;
}

export class PIIProtection {
  private static piiFields: Map<string, PIIField[]> = new Map([
    ['User', [
      { field: 'dni', type: 'dni', encrypted: true, hashed: true },
      { field: 'cuit', type: 'cuit', encrypted: true },
      { field: 'phone', type: 'phone', encrypted: true },
      { field: 'email', type: 'email', encrypted: false, hashed: true }
    ]],
    ['Contract', [
      { field: 'ownerDni', type: 'dni', encrypted: true },
      { field: 'tenantDni', type: 'dni', encrypted: true }
    ]]
  ]);

  /**
   * Mask PII for display
   */
  static maskPII(value: string, type: PIIField['type']): string {
    switch (type) {
      case 'dni':
        // Show first 2 and last 2 digits
        return value.replace(/^(\d{2})\d+(\d{2})$/, '$1****$2');

      case 'cuit':
        // Show first 2 and last digit
        return value.replace(/^(\d{2})\d+(\d)$/, '$1-*******-$2');

      case 'phone':
        // Show area code and last 4 digits
        return value.replace(/^(\+\d{2})?\d+(\d{4})$/, '$1****$2');

      case 'email':
        // Show first 2 chars and domain
        const [user, domain] = value.split('@');
        return `${user.substring(0, 2)}***@${domain}`;

      case 'address':
        // Show street name only
        return value.replace(/\d+/g, '***');

      default:
        return '***';
    }
  }

  /**
   * Audit PII access
   */
  static async auditPIIAccess(
    userId: string,
    entity: string,
    entityId: string,
    fields: string[],
    purpose: string
  ): Promise<void> {
    await prisma.piiAuditLog.create({
      data: {
        userId,
        entity,
        entityId,
        fields,
        purpose,
        ipAddress: getClientIP(),
        userAgent: getUserAgent(),
        timestamp: new Date()
      }
    });
  }

  /**
   * Middleware for PII access control
   */
  static async protectPII<T>(
    data: T,
    userId: string,
    options: {
      entity: string;
      purpose: string;
      allowedFields?: string[];
    }
  ): Promise<T> {
    const piiFields = this.piiFields.get(options.entity);
    if (!piiFields) return data;

    const protectedData = { ...data };
    const accessedFields: string[] = [];

    for (const field of piiFields) {
      if (field.field in protectedData) {
        // Check if field is allowed
        if (
          options.allowedFields &&
          !options.allowedFields.includes(field.field)
        ) {
          // Mask the field
          (protectedData as any)[field.field] = this.maskPII(
            (protectedData as any)[field.field],
            field.type
          );
        } else {
          // Field is accessed
          accessedFields.push(field.field);
        }
      }
    }

    // Audit access
    if (accessedFields.length > 0) {
      await this.auditPIIAccess(
        userId,
        options.entity,
        (data as any).id,
        accessedFields,
        options.purpose
      );
    }

    return protectedData;
  }
}

// Prisma middleware for automatic PII protection
prisma.$use(async (params, next) => {
  const result = await next(params);

  // Encrypt PII on create/update
  if (
    ['create', 'update', 'upsert'].includes(params.action) &&
    PIIProtection.hasPIIFields(params.model)
  ) {
    const encrypted = await PIIProtection.encryptPIIFields(
      result,
      params.model
    );
    return encrypted;
  }

  // Decrypt PII on read
  if (
    ['findUnique', 'findFirst', 'findMany'].includes(params.action) &&
    PIIProtection.hasPIIFields(params.model)
  ) {
    const decrypted = await PIIProtection.decryptPIIFields(
      result,
      params.model
    );
    return decrypted;
  }

  return result;
});

Compliance Requirements
// lib/security/compliance.ts

export class ComplianceService {
  /**
   * GDPR - Right to be forgotten
   */
  static async deleteUserData(userId: string): Promise<void> {
    // Start transaction
    await prisma.$transaction(async (tx) => {
      // Anonymize user record
      await tx.user.update({
        where: { id: userId },
        data: {
          email: `deleted-${userId}@neo-rent.ar`,
          phone: null,
          dni: crypto.randomBytes(4).toString('hex'),
          firstName: 'Deleted',
          lastName: 'User',
          deletedAt: new Date()
        }
      });

      // Delete sensitive documents
      await tx.document.deleteMany({
        where: { userId }
      });

      // Anonymize contracts
      await tx.contract.updateMany({
        where: { OR: [{ ownerId: userId }, { tenantId: userId }] },
        data: {
          content: 'REDACTED - User data deleted'
        }
      });

      // Delete payment methods
      await tx.paymentMethod.deleteMany({
        where: { userId }
      });

      // Create deletion audit log
      await tx.dataRetentionLog.create({
        data: {
          userId,
          action: 'USER_DELETION',
          affectedEntities: [
            'User',
            'Document',
            'Contract',
            'PaymentMethod'
          ],
          timestamp: new Date()
        }
      });
    });

    // Delete from external services
    await this.deleteFromExternalServices(userId);
  }

  /**
   * GDPR - Data portability
   */
  static async exportUserData(userId: string): Promise<Buffer> {
    const userData = await prisma.user.findUnique({
      where: { id: userId },
      include: {
        properties: true,
        contracts: true,
        transactions: true,
        reviews: true,
        documents: {
          select: {
            id: true,
            type: true,
            createdAt: true
            // Don't include actual document content
          }
        }
      }
    });

    // Remove sensitive fields
    const sanitized = this.sanitizeForExport(userData);

    // Create ZIP with JSON data
    const zip = new JSZip();
    zip.file('user-data.json', JSON.stringify(sanitized, null, 2));

    // Add documents folder info
    zip.file('documents/README.txt',
      'Document files available upon separate request'
    );

    return zip.generateAsync({ type: 'nodebuffer' });
  }

  /**
   * CCPA - Opt-out of data sale
   */
  static async optOutDataSharing(
    userId: string,
    categories: string[]
  ): Promise<void> {
    await prisma.privacyPreference.upsert({
      where: { userId },
      create: {
        userId,
        shareWithPartners: false,
        analyticsEnabled: false,
        marketingEnabled: false,
        categories
      },
      update: {
        shareWithPartners: false,
        categories
      }
    });
  }

  /**
   * Data retention policy enforcement
   */
  static async enforceDataRetention(): Promise<void> {
    // Delete old unverified accounts (30 days)
    await prisma.user.deleteMany({
      where: {
        verificationStatus: 'UNVERIFIED',
        createdAt: { lt: subDays(new Date(), 30) }
      }
    });

    // Archive old contracts (3 years)
    const oldContracts = await prisma.contract.findMany({
      where: {
        endDate: { lt: subYears(new Date(), 3) },
        archived: false
      }
    });

    for (const contract of oldContracts) {
      // Archive to cold storage
      await this.archiveContract(contract);

      // Remove from active database
      await prisma.contract.update({
        where: { id: contract.id },
        data: {
          archived: true,
          content: 'ARCHIVED',
          archivedAt: new Date()
        }
      });
    }

    // Delete old audit logs (1 year)
    await prisma.auditLog.deleteMany({
      where: {
        createdAt: { lt: subYears(new Date(), 1) }
      }
    });
  }
}

// Scheduled job for compliance
export async function complianceJob() {
  // Run daily at 2 AM
  cron.schedule('0 2 * * *', async () => {
    await ComplianceService.enforceDataRetention();

    // Check for deletion requests
    const deletionRequests = await prisma.deletionRequest.findMany({
      where: {
        status: 'PENDING',
        requestedAt: { lt: subDays(new Date(), 30) }
      }
    });

    for (const request of deletionRequests) {
      await ComplianceService.deleteUserData(request.userId);

      await prisma.deletionRequest.update({
        where: { id: request.id },
        data: {
          status: 'COMPLETED',
          completedAt: new Date()
        }
      });
    }
  });
}

6.3 Application Security
Input Validation and Sanitization
// lib/security/validation.ts

import DOMPurify from 'isomorphic-dompurify';
import { z } from 'zod';

export class ValidationService {
  /**
   * Sanitize HTML content
   */
  static sanitizeHTML(input: string): string {
    return DOMPurify.sanitize(input, {
      ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a', 'p', 'br'],
      ALLOWED_ATTR: ['href', 'target']
    });
  }

  /**
   * Validate and sanitize file uploads
   */
  static async validateFileUpload(
    file: File,
    options: {
      maxSize?: number;
      allowedTypes?: string[];
      scanForMalware?: boolean;
    }
  ): Promise<ValidationResult> {
    const errors: string[] = [];

    // Check file size
    const maxSize = options.maxSize || 10 * 1024 * 1024; // 10MB default
    if (file.size > maxSize) {
      errors.push(`File size exceeds ${maxSize / 1024 / 1024}MB limit`);
    }

    // Check file type
    const allowedTypes = options.allowedTypes || [
      'image/jpeg',
      'image/png',
      'image/webp',
      'application/pdf'
    ];

    if (!allowedTypes.includes(file.type)) {
      errors.push(`File type ${file.type} not allowed`);
    }

    // Verify file magic numbers
    const buffer = await file.arrayBuffer();
    const header = new Uint8Array(buffer).slice(0, 4);

    if (!this.verifyFileMagicNumber(header, file.type)) {
      errors.push('File content does not match declared type');
    }

    // Scan for malware if enabled
    if (options.scanForMalware) {
      const scanResult = await this.scanFile(buffer);
      if (!scanResult.clean) {
        errors.push('File contains potential security threat');
      }
    }

    return {
      valid: errors.length === 0,
      errors
    };
  }

  /**
   * Validate Argentine DNI
   */
  static validateDNI(dni: string): boolean {
    // Remove any non-numeric characters
    const cleaned = dni.replace(/\D/g, '');

    // DNI should be 7 or 8 digits
    if (!/^\d{7,8}$/.test(cleaned)) {
      return false;
    }

    // Additional validation rules
    // Check for obviously invalid patterns
    if (/^0+$/.test(cleaned) || /^(\d)\1+$/.test(cleaned)) {
      return false;
    }

    return true;
  }

  /**
   * Validate CUIT/CUIL
   */
  static validateCUIT(cuit: string): boolean {
    // Remove any non-numeric characters
    const cleaned = cuit.replace(/\D/g, '');

    // CUIT should be 11 digits
    if (!/^\d{11}$/.test(cleaned)) {
      return false;
    }

    // Validate check digit
    const digits = cleaned.split('').map(Number);
    const multipliers = [5, 4, 3, 2, 7, 6, 5, 4, 3, 2];

    let sum = 0;
    for (let i = 0; i < 10; i++) {
      sum += digits[i] * multipliers[i];
    }


   const checkDigit = 11 - (sum % 11);
    const expectedDigit = checkDigit === 11 ? 0 : checkDigit === 10 ? 9 : checkDigit;

    return digits[10] === expectedDigit;
  }

  /**
   * SQL injection prevention for raw queries
   */
  static sanitizeSQL(input: string): string {
    // Escape special characters
    return input
      .replace(/[\0\x08\x09\x1a\n\r"'\\\%]/g, (char) => {
        switch (char) {
          case "\0": return "\\0";
          case "\x08": return "\\b";
          case "\x09": return "\\t";
          case "\x1a": return "\\z";
          case "\n": return "\\n";
          case "\r": return "\\r";
          case "\"":
          case "'":
          case "\\":
          case "%":
            return "\\" + char;
          default:
            return char;
        }
      });
  }

  private static verifyFileMagicNumber(
    header: Uint8Array,
    mimeType: string
  ): boolean {
    const magicNumbers: Record<string, number[][]> = {
      'image/jpeg': [[0xFF, 0xD8, 0xFF]],
      'image/png': [[0x89, 0x50, 0x4E, 0x47]],
      'image/gif': [[0x47, 0x49, 0x46, 0x38]],
      'image/webp': [[0x52, 0x49, 0x46, 0x46]],
      'application/pdf': [[0x25, 0x50, 0x44, 0x46]]
    };

    const validHeaders = magicNumbers[mimeType];
    if (!validHeaders) return true; // Unknown type, skip check

    return validHeaders.some(valid =>
      valid.every((byte, index) => header[index] === byte)
    );
  }
}

// Zod schemas for common validations
export const argentinePhoneSchema = z.string()
  .regex(/^\+54\s?9?\s?\d{2,4}\s?\d{6,8}$/, 'Invalid Argentine phone number');

export const addressSchema = z.object({
  street: z.string().min(3).max(100),
  number: z.string().regex(/^\d+(\s?[A-Za-z])?$/),
  floor: z.string().optional(),
  apartment: z.string().optional(),
  city: z.string().min(2).max(50),
  province: z.enum([
    'BUENOS_AIRES',
    'CABA',
    'NEUQUEN',
    'RIO_NEGRO',
    // ... other provinces
  ]),
  postalCode: z.string().regex(/^[A-Z]\d{4}[A-Z]{3}$/)
});

export const priceSchema = z.number()
  .positive()
  .max(10000000) // Reasonable max for Argentine market
  .refine(val => Number.isInteger(val * 100), {
    message: 'Price can have at most 2 decimal places'
  });

OWASP Compliance Measures
// lib/security/owasp.ts

export class OWASPSecurity {
  /**
   * A1: Injection Prevention
   */
  static preventInjection = {
    // Already using Prisma ORM which prevents SQL injection
    // Additional measures for raw queries
    validateQuery: (query: string, params: any[]) => {
      // Check for suspicious patterns
      const suspiciousPatterns = [
        /(\b(DELETE|DROP|EXEC(UTE)?|INSERT|SELECT|UNION|UPDATE)\b)/gi,
        /(--|#|\/\*|\*\/)/g,
        /(\bOR\b\s*\d+\s*=\s*\d+)/gi
      ];

      for (const pattern of suspiciousPatterns) {
        if (pattern.test(query)) {
          throw new Error('Potentially malicious query detected');
        }
      }

      return true;
    }
  };

  /**
   * A2: Broken Authentication Prevention
   */
  static authenticationSecurity = {
    // Password requirements
    passwordPolicy: z.string()
      .min(12, 'Password must be at least 12 characters')
      .regex(/[A-Z]/, 'Password must contain uppercase letter')
      .regex(/[a-z]/, 'Password must contain lowercase letter')
      .regex(/[0-9]/, 'Password must contain number')
      .regex(/[^A-Za-z0-9]/, 'Password must contain special character'),

    // Account lockout after failed attempts
    async checkAccountLockout(userId: string): Promise<boolean> {
      const attempts = await redis.get(`failed_login:${userId}`);
      return parseInt(attempts || '0') >= 5;
    },

    async recordFailedAttempt(userId: string): Promise<void> {
      const key = `failed_login:${userId}`;
      const current = await redis.get(key);
      const attempts = parseInt(current || '0') + 1;

      await redis.setex(key, 900, attempts.toString()); // 15 min expiry

      if (attempts >= 5) {
        await this.notifyAccountLockout(userId);
      }
    }
  };

  /**
   * A3: Sensitive Data Exposure Prevention
   */
  static dataExposurePrevention = {
    // Response filtering
    filterSensitiveData<T>(data: T, role: UserRole): T {
      const filtered = { ...data };

      // Define sensitive fields per role
      const sensitiveFields: Record<string, string[]> = {
        TENANT: ['cuit', 'internalNotes', 'trustScoreDetails'],
        OWNER: ['creditScore', 'bankDetails'],
        ALP: ['earnings', 'performanceMetrics']
      };

      const fieldsToRemove = sensitiveFields[role] || [];

      for (const field of fieldsToRemove) {
        delete (filtered as any)[field];
      }

      return filtered;
    }
  };

  /**
   * A4: XXE Prevention
   */
  static xxePrevention = {
    parseXMLSafely: (xml: string) => {
      // Disable external entities
      const parser = new DOMParser();
      const doc = parser.parseFromString(xml, 'text/xml', {
        doctype: false,
        entities: false,
        resolveExternals: false
      });

      return doc;
    }
  };

  /**
   * A5: Broken Access Control Prevention
   */
  static accessControl = {
    async checkResourceAccess(
      userId: string,
      resource: string,
      resourceId: string,
      action: string
    ): Promise<boolean> {
      // Implement resource-based access control
      const accessRule = await prisma.accessRule.findFirst({
        where: {
          userId,
          resource,
          resourceId,
          action
        }
      });

      if (!accessRule) {
        // Check ownership
        return this.checkOwnership(userId, resource, resourceId);
      }

      return accessRule.allowed;
    },

    async checkOwnership(
      userId: string,
      resource: string,
      resourceId: string
    ): Promise<boolean> {
      switch (resource) {
        case 'property':
          const property = await prisma.property.findUnique({
            where: { id: resourceId }
          });
          return property?.ownerId === userId;

        case 'contract':
          const contract = await prisma.contract.findUnique({
            where: { id: resourceId }
          });
          return [contract?.ownerId, contract?.tenantId].includes(userId);

        default:
          return false;
      }
    }
  };

  /**
   * A7: XSS Prevention
   */
  static xssPrevention = {
    // Content Security Policy headers
    getCSPHeaders(): Record<string, string> {
      return {
        'Content-Security-Policy': [
          "default-src 'self'",
          "script-src 'self' 'unsafe-inline' 'unsafe-eval' https://js.mercadopago.com",
          "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
          "font-src 'self' https://fonts.gstatic.com",
          "img-src 'self' data: https: blob:",
          "connect-src 'self' https://api.mercadopago.com wss://supabase.com",
          "frame-src 'self' https://www.mercadopago.com",
          "object-src 'none'",
          "base-uri 'self'",
          "form-action 'self'",
          "frame-ancestors 'none'",
          "upgrade-insecure-requests"
        ].join('; ')
      };
    },

    // React component for safe HTML rendering
    SafeHTML: ({ content, className }: { content: string; className?: string }) => {
      const sanitized = DOMPurify.sanitize(content);
      return (
        <div
          className={className}
          dangerouslySetInnerHTML={{ __html: sanitized }}
        />
      );
    }
  };

  /**
   * A8: Insecure Deserialization Prevention
   */
  static deserializationSecurity = {
    safeJSONParse: (json: string) => {
      try {
        // Validate JSON structure before parsing
        if (json.length > 1024 * 1024) { // 1MB limit
          throw new Error('JSON too large');
        }

        const parsed = JSON.parse(json);

        // Check for prototype pollution
        if ('__proto__' in parsed || 'constructor' in parsed || 'prototype' in parsed) {
          throw new Error('Potential prototype pollution detected');
        }

        return parsed;
      } catch (error) {
        throw new Error('Invalid JSON');
      }
    }
  };

  /**
   * A10: Insufficient Logging & Monitoring
   */
  static securityLogging = {
    async logSecurityEvent(event: SecurityEvent): Promise<void> {
      await prisma.securityLog.create({
        data: {
          type: event.type,
          severity: event.severity,
          userId: event.userId,
          ipAddress: event.ipAddress,
          userAgent: event.userAgent,
          details: event.details,
          timestamp: new Date()
        }
      });

      // Alert on high severity events
      if (event.severity === 'HIGH') {
        await this.alertSecurityTeam(event);
      }
    },

    async detectAnomalies(userId: string): Promise<AnomalyResult> {
      const recentLogs = await prisma.securityLog.findMany({
        where: {
          userId,
          timestamp: { gte: subHours(new Date(), 1) }
        }
      });

      // Check for suspicious patterns
      const failedLogins = recentLogs.filter(
        log => log.type === 'FAILED_LOGIN'
      ).length;

      const differentIPs = new Set(
        recentLogs.map(log => log.ipAddress)
      ).size;

      return {
        suspicious: failedLogins > 3 || differentIPs > 5,
        reasons: []
      };
    }
  };
}

Security Headers and Policies
// middleware/security.ts

export function securityMiddleware(req: Request): Headers {
  const headers = new Headers();

  // OWASP recommended headers
  headers.set('X-Frame-Options', 'DENY');
  headers.set('X-Content-Type-Options', 'nosniff');
  headers.set('X-XSS-Protection', '1; mode=block');
  headers.set('Referrer-Policy', 'strict-origin-when-cross-origin');
  headers.set('Permissions-Policy',
    'geolocation=(self), microphone=(), camera=(self)'
  );

  // HSTS
  headers.set('Strict-Transport-Security',
    'max-age=31536000; includeSubDomains; preload'
  );

  // CSP
  headers.set('Content-Security-Policy',
    OWASPSecurity.xssPrevention.getCSPHeaders()['Content-Security-Policy']
  );

  // Additional security headers
  headers.set('X-DNS-Prefetch-Control', 'off');
  headers.set('Expect-CT', 'max-age=86400, enforce');

  return headers;
}

// Rate limiting configuration
export const rateLimitConfig = {
  api: {
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: 'Too many requests from this IP'
  },
  auth: {
    windowMs: 15 * 60 * 1000,
    max: 5, // stricter for auth endpoints
    skipSuccessfulRequests: false
  },
  upload: {
    windowMs: 60 * 60 * 1000, // 1 hour
    max: 50, // 50 uploads per hour
    keyGenerator: (req: Request) => {
      // Rate limit by user ID for uploads
      return req.userId || req.ip;
    }
  }
};

// CORS configuration
export const corsConfig = {
  origin: process.env.NODE_ENV === 'production'
    ? ['https://neo-rent.ar', 'https://www.neo-rent.ar']
    : ['http://localhost:3000'],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: [
    'Content-Type',
    'Authorization',
    'X-Requested-With',
    'X-CSRF-Token'
  ],
  exposedHeaders: ['X-Total-Count', 'X-Page-Count'],
  maxAge: 86400 // 24 hours
};

7. User Interface Specifications
7.1 Design System
The NEO-RENT design system emphasizes bold simplicity with intuitive navigation, creating frictionless experiences through strategic use of whitespace and color accents.
Visual Design Principles
Clarity First: Every element serves a purpose, reducing cognitive load
Spatial Hierarchy: Strategic negative space guides user attention
Motion with Meaning: Physics-based transitions provide spatial continuity
Accessible by Default: WCAG 2.1 AA compliance as baseline
Mobile-First Responsive: Optimized for touch interactions
Component Library Structure
// components/ui/index.ts

export * from './primitives'; // Base components
export * from './forms';      // Form components
export * from './feedback';   // Feedback components
export * from './layout';     // Layout components
export * from './navigation'; // Navigation components
export * from './data';       // Data display components
export * from './surfaces';   // Cards, modals, etc.

7.2 Design Foundations
7.2.1 Color System
// styles/tokens/colors.ts

export const colors = {
  // Primary Colors
  primary: {
    blue: '#1A73E8',      // Primary brand color
    white: '#FFFFFF',     // Clean surfaces
  },

  // Secondary Colors
  secondary: {
    blueLight: '#4285F4', // Hover states
    bluePale: '#E8F0FE',  // Subtle backgrounds
    gray: '#5F6368',      // Secondary text
  },

  // Accent Colors
  accent: {
    teal: '#00ACC1',      // Property highlights
    purple: '#7B1FA2',    // Premium features
    orange: '#F57C00',    // Time-sensitive items
  },

  // Functional Colors
  functional: {
    success: '#1E8E3E',   // Positive states
    error: '#D93025',     // Errors
    warning: '#F9AB00',   // Warnings
    info: '#1967D2',      // Information
  },

  // Background Colors
  background: {
    primary: '#FFFFFF',   // Main content
    secondary: '#F8F9FA', // App background
    tertiary: '#F1F3F4',  // Disabled states
    dark: '#202124',      // Dark mode
  },

  // Semantic Mappings
  semantic: {
    text: {
      primary: '#202124',
      secondary: '#5F6368',
      disabled: '#9AA0A6',
      inverse: '#FFFFFF',
    },
    border: {
      default: '#DADCE0',
      focus: '#1A73E8',
      error: '#D93025',
    },
    surface: {
      raised: '#FFFFFF',
      sunken: '#F8F9FA',
      overlay: 'rgba(32, 33, 36, 0.6)',
    }
  }
} as const;

// Dark mode variants
export const darkColors = {
  ...colors,
  background: {
    primary: '#202124',
    secondary: '#292A2D',
    tertiary: '#35363A',
    dark: '#000000',
  },
  semantic: {
    text: {
      primary: '#E8EAED',
      secondary: '#9AA0A6',
      disabled: '#5F6368',
      inverse: '#202124',
    },
    border: {
      default: '#3C4043',
      focus: '#8AB4F8',
      error: '#F28B82',
    }
  }
};

7.2.2 Typography
// styles/tokens/typography.ts

export const typography = {
  // Font Families
  fontFamily: {
    primary: 'Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif',
    secondary: '"SF Pro Display", -apple-system, BlinkMacSystemFont, sans-serif',
    mono: '"JetBrains Mono", "SF Mono", Monaco, monospace',
  },

  // Font Weights
  fontWeight: {
    light: 300,
    regular: 400,
    medium: 500,
    semibold: 600,
    bold: 700,
  },

  // Type Scale
  typeScale: {
    h1: {
      fontSize: '32px',
      lineHeight: '40px',
      fontWeight: 700,
      letterSpacing: '-0.5px',
    },
    h2: {
      fontSize: '28px',
      lineHeight: '36px',
      fontWeight: 600,
      letterSpacing: '-0.3px',
    },
    h3: {
      fontSize: '24px',
      lineHeight: '32px',
      fontWeight: 600,
      letterSpacing: '-0.2px',
    },
    h4: {
      fontSize: '20px',
      lineHeight: '28px',
      fontWeight: 500,
      letterSpacing: '-0.1px',
    },
    bodyLarge: {
      fontSize: '18px',
      lineHeight: '28px',
      fontWeight: 400,
      letterSpacing: '0px',
    },
    body: {
      fontSize: '16px',
      lineHeight: '24px',
      fontWeight: 400,
      letterSpacing: '0px',
    },
    bodySmall: {
      fontSize: '14px',
      lineHeight: '20px',
      fontWeight: 400,
      letterSpacing: '0.1px',
    },
    caption: {
      fontSize: '12px',
      lineHeight: '16px',
      fontWeight: 500,
      letterSpacing: '0.3px',
    },
    button: {
      fontSize: '16px',
      lineHeight: '24px',
      fontWeight: 500,
      letterSpacing: '0.5px',
      textTransform: 'none' as const,
    },
    price: {
      fontSize: '24px',
      lineHeight: '32px',
      fontWeight: 700,
      letterSpacing: '-0.2px',
    },
  },
};

7.2.3 Spacing & Layout
// styles/tokens/spacing.ts

export const spacing = {
  // Base unit: 4px
  base: 4,

  // Spacing scale
  scale: {
    micro: 4,    // 4px - inline elements
    xs: 8,       // 8px - compact spacing
    sm: 12,      // 12px - list items
    md: 16,      // 16px - default padding
    lg: 24,      // 24px - section spacing
    xl: 32,      // 32px - major sections
    xxl: 48,     // 48px - page margins
    xxxl: 64,    // 64px - hero sections
  },

  // Container widths
  container: {
    xs: 360,
    sm: 640,
    md: 768,
    lg: 1024,
    xl: 1280,
    xxl: 1440,
  },

  // Breakpoints
  breakpoints: {
    mobile: 0,
    tablet: 768,
    desktop: 1024,
    wide: 1440,
  },

  // Grid system
  grid: {
    columns: 12,
    gap: {
      mobile: 16,
      tablet: 24,
      desktop: 32,
    },
    margin: {
      mobile: 16,
      tablet: 32,
      desktop: 48,
    },
  },
};

// Layout utilities
export const layout = {
  // Max content width
  maxWidth: {
    content: '1200px',
    narrow: '800px',
    wide: '1440px',
  },

  // Common patterns
  stack: (space: keyof typeof spacing.scale) => ({
    display: 'flex',
    flexDirection: 'column' as const,
    gap: spacing.scale[space],
  }),

  row: (space: keyof typeof spacing.scale) => ({
    display: 'flex',
    flexDirection: 'row' as const,
    alignItems: 'center',
    gap: spacing.scale[space],
  }),

  center: {
    display: 'flex',
    alignItems: 'center',
    justifyContent: 'center',
  },
};

7.2.4 Interactive Elements
// styles/tokens/interactive.ts

export const interactive = {
  // Transition timing
  transition: {
    duration: {
      instant: 0,
      fast: 100,
      normal: 200,
      slow: 300,
      slower: 400,
    },
    timing: {
      standard: 'cubic-bezier(0.4, 0.0, 0.2, 1)',
      deceleration: 'cubic-bezier(0.0, 0.0, 0.2, 1)',
      acceleration: 'cubic-bezier(0.4, 0.0, 1, 1)',
      sharp: 'cubic-bezier(0.4, 0.0, 0.6, 1)',
    },
  },

  // Button styles
  button: {
    primary: {
      background: colors.primary.blue,
      color: colors.primary.white,
      height: 48,
      borderRadius: 24,
      padding: '12px 24px',
      boxShadow: '0 1px 2px rgba(60,64,67,0.3), 0 1px 3px rgba(60,64,67,0.15)',
      hover: {
        background: colors.secondary.blueLight,
        transform: 'translateY(-1px)',
        boxShadow: '0 2px 4px rgba(60,64,67,0.3), 0 2px 6px rgba(60,64,67,0.15)',
      },
      active: {
        transform: 'translateY(0)',
        boxShadow: '0 1px 2px rgba(60,64,67,0.3)',
      },
    },
    secondary: {
      background: 'transparent',
      color: colors.primary.blue,
      height: 48,
      borderRadius: 24,
      border: `1px solid ${colors.secondary.gray}`,
      padding: '12px 24px',
      hover: {
        borderColor: colors.primary.blue,
        background: colors.secondary.bluePale,
      },
    },
    ghost: {
      background: 'transparent',
      color: colors.primary.blue,
      height: 40,
      padding: '8px 16px',
      hover: {
        background: 'rgba(26, 115, 232, 0.08)',
      },
    },
  },

  // Form elements
  input: {
    height: 56,
    borderRadius: 8,
    border: `1px solid ${colors.semantic.border.default}`,
    padding: '16px',
    fontSize: '16px',
    focus: {
      borderColor: colors.primary.blue,
      borderWidth: 2,
      outline: 'none',
    },
    error: {
      borderColor: colors.functional.error,
    },
  },

  // Focus styles
  focus: {
    outline: `2px solid ${colors.primary.blue}`,
    outlineOffset: 2,
  },

  // Loading states
  skeleton: {
    background: `linear-gradient(
      90deg,
      ${colors.background.tertiary} 0%,
      ${colors.background.secondary} 50%,
      ${colors.background.tertiary} 100%
    )`,
    backgroundSize: '200% 100%',
    animation: 'shimmer 1.5s ease-in-out infinite',
  },
};

7.2.5 Component Specifications
// components/ui/primitives/Button.tsx

import { forwardRef } from 'react';
import { cva, type VariantProps } from 'class-variance-authority';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  'inline-flex items-center justify-center font-medium transition-all focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        primary:
          'bg-primary-blue text-white shadow-button hover:bg-secondary-blueLight hover:-translate-y-0.5 hover:shadow-button-hover active:translate-y-0 active:shadow-button-active',
        secondary:
          'border border-secondary-gray bg-white text-primary-blue hover:border-primary-blue hover:bg-secondary-bluePale',
        ghost:
          'text-primary-blue hover:bg-primary-blue/8',
        danger:
          'bg-functional-error text-white hover:bg-functional-error/90',
      },
      size: {
        sm: 'h-9 px-3 text-sm',
        md: 'h-12 px-6',
        lg: 'h-14 px-8 text-lg',
        icon: 'h-10 w-10',
      },
      rounded: {
        none: 'rounded-none',
        sm: 'rounded',
        md: 'rounded-lg',
        lg: 'rounded-xl',
        full: 'rounded-full',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
      rounded: 'full',
    },
  }
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  loading?: boolean;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
}

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({
    className,
    variant,
    size,
    rounded,
    loading,
    leftIcon,
    rightIcon,
    children,
    disabled,
    ...props
  }, ref) => {
    return (
      <button
        className={cn(buttonVariants({ variant, size, rounded, className }))}
        ref={ref}
        disabled={disabled || loading}
        {...props}
      >
        {loading ? (
          <Spinner className="mr-2" />
        ) : leftIcon ? (
          <span className="mr-2">{leftIcon}</span>
        ) : null}
        {children}
        {rightIcon && <span className="ml-2">{rightIcon}</span>}
      </button>
    );
  }
);

Button.displayName = 'Button';

// Card component
export const Card = forwardRef
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & {
    interactive?: boolean;
    elevated?: boolean;
  }
>(({ className, interactive, elevated, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'rounded-2xl bg-white p-6',
      elevated && 'shadow-card',
      interactive && 'transition-all hover:shadow-card-hover hover:scale-[1.02] cursor-pointer',
      className
    )}
    {...props}
  />
));

Card.displayName = 'Card';

7.3 User Experience Flows
Property Search Flow
// components/features/search/SearchFlow.tsx

export function PropertySearchFlow() {
  const [searchParams, setSearchParams] = useState<SearchParams>({
    location: null,
    priceRange: { min: 0, max: Infinity },
    bedrooms: null,
    propertyType: null,
  });

  const [viewMode, setViewMode] = useState<'map' | 'list'>('map');

  return (
    <div className="flex h-screen">
      {/* Search Bar */}
      <SearchBar
        className="absolute top-4 left-4 right-4 z-10 max-w-2xl mx-auto"
        onSearch={setSearchParams}
      />

      {/* Map View */}
      {viewMode === 'map' && (
        <MapView
          className="flex-1"
          searchParams={searchParams}
          onPropertySelect={(property) => {
            // Show property preview
          }}
        />
      )}

      {/* Results Panel */}
      <ResultsPanel
        className={cn(
          'w-full md:w-96 bg-white',
          viewMode === 'map' && 'absolute right-0 top-0 bottom-0'
        )}
        searchParams={searchParams}
        onViewModeChange={setViewMode}
      />
    </div>
  );
}

// Search bar with filters
function SearchBar({ onSearch, className }: SearchBarProps) {
  return (
    <Card className={cn('backdrop-blur-lg bg-white/95', className)}>
      <div className="flex items-center gap-2">
        <Input
          placeholder="Buscar por zona, barrio o dirección..."
          className="flex-1"
          leftIcon={<SearchIcon />}
        />

        <FilterPopover>
          <Button variant="secondary" size="icon">
            <FilterIcon />
          </Button>
        </FilterPopover>

        <Button onClick={() => onSearch(filters)}>
          Buscar
        </Button>
      </div>
    </Card>
  );
}

// Property card for search results
function PropertyCard({ property }: { property: Property }) {
  const { ref, inView } = useInView({
    threshold: 0.1,
    triggerOnce: true,
  });

  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0, y: 20 }}
      animate={inView ? { opacity: 1, y: 0 } : {}}
      transition={{ duration: 0.3 }}
    >
      <Card interactive className="overflow-hidden">
        {/* Image Carousel */}
        <div className="relative -m-6 mb-4">
          <ImageCarousel images={property.images} />

          {/* Price Badge */}
          <div className="absolute bottom-4 left-4">
            <Badge className="bg-white/95 backdrop-blur">
              <span className="text-2xl font-bold">
                ${property.basePrice.toLocaleString('es-AR')}
              </span>
              <span className="text-sm text-secondary-gray">/mes</span>
            </Badge>
          </div>

          {/* Match Score */}
          {property.matchScore && (
            <div className="absolute top-4 right-4">
              <MatchScoreBadge score={property.matchScore} />
            </div>
          )}
        </div>

        {/* Property Info */}
        <div className="space-y-2">
          <h3 className="text-lg font-semibold line-clamp-1">
            {property.title}
          </h3>

          <p className="text-secondary-gray flex items-center gap-1">
            <LocationIcon className="w-4 h-4" />
            {property.neighborhood}, {property.city}
          </p>

          <div className="flex items-center gap-4 text-sm">
            <span className="flex items-center gap-1">
              <BedIcon className="w-4 h-4" />
              {property.bedrooms} hab
            </span>
            <span className="flex items-center gap-1">
              <BathIcon className="w-4 h-4" />
              {property.bathrooms} baño
            </span>
            <span className="flex items-center gap-1">
              <RulerIcon className="w-4 h-4" />
              {property.surfaceArea} m²
            </span>
          </div>

          {/* Quick Actions */}
          <div className="flex gap-2 pt-2">
            <Button variant="secondary" size="sm" className="flex-1">
              Ver detalles
            </Button>
            <Button size="sm" className="flex-1">
              Agendar visita
            </Button>
          </div>
        </div>
      </Card>
    </motion.div>
  );
}

Visit Scheduling Flow
// components/features/visits/SchedulingFlow.tsx

export function VisitSchedulingFlow({ propertyId }: { propertyId: string }) {
  const [step, setStep] = useState<'calendar' | 'confirm' | 'success'>(
    'calendar'
  );
  const [selectedSlots, setSelectedSlots] = useState<TimeSlot[]>([]);

  return (
    <AnimatePresence mode="wait">
      {step === 'calendar' && (
        <motion.div
          key="calendar"
          initial={{ opacity: 0, x: 20 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -20 }}
        >
          <CalendarView
            propertyId={propertyId}
            onSlotsSelected={(slots) => {
              setSelectedSlots(slots);
              setStep('confirm');
            }}
          />
        </motion.div>
      )}

      {step === 'confirm' && (
        <motion.div
          key="confirm"
          initial={{ opacity: 0, x: 20 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -20 }}
        >
          <ConfirmationView
            slots={selectedSlots}
            onConfirm={async () => {
              await scheduleVisits(selectedSlots);
              setStep('success');
            }}
            onBack={() => setStep('calendar')}
          />
        </motion.div>
      )}

      {step === 'success' && (
        <motion.div
          key="success"
          initial={{ opacity: 0, scale: 0.9 }}
          animate={{ opacity: 1, scale: 1 }}
        >
          <SuccessView />
        </motion.div>
      )}
    </AnimatePresence>
  );
}

// Calendar component with available slots
function CalendarView({ propertyId, onSlotsSelected }: CalendarViewProps) {
  const { data: availability } = usePropertyAvailability(propertyId);
  const [selectedDate, setSelectedDate] = useState<Date | null>(null);

  return (
    <div className="grid md:grid-cols-2 gap-6">
      {/* Calendar */}
      <div>
        <h3 className="text-lg font-semibold mb-4">
          Seleccioná una fecha
        </h3>
        <Calendar
          mode="single"
          selected={selectedDate}
          onSelect={setSelectedDate}
          disabled={(date) => !isDateAvailable(date, availability)}
          className="rounded-lg border"
        />
      </div>

      {/* Time Slots */}
      {selectedDate && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          className="space-y-4"
        >
          <h3 className="text-lg font-semibold">
            Horarios disponibles
          </h3>

          <div className="grid grid-cols-2 gap-2">
            {getTimeSlotsForDate(selectedDate, availability).map((slot) => (
              <TimeSlotButton
                key={slot.id}
                slot={slot}
                selected={selectedSlots.includes(slot)}
                onToggle={() => toggleSlot(slot)}
              />
            ))}
          </div>

          {selectedSlots.length > 0 && (
            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
            >
              <Button
                className="w-full"
                onClick={() => onSlotsSelected(selectedSlots)}
              >
                Continuar con {selectedSlots.length} visita
                {selectedSlots.length > 1 ? 's' : ''}
              </Button>
            </motion.div>
          )}
        </motion.div>
      )}
    </div>
  );
}

8. Infrastructure & Deployment
8.1 Infrastructure Requirements
Hosting Environment Specifications
Production Environment
Platform: Vercel Pro/Enterprise
Regions: Primary - São Paulo (South America), Secondary - US East
Compute: Serverless Functions with 3GB memory, 10s timeout
CDN: Vercel Edge Network with 100+ PoPs globally
Database Infrastructure
Provider: Supabase Pro
Database: PostgreSQL 15 with PostGIS extension
Specifications:
8GB RAM minimum
100GB SSD storage
Daily automated backups
Point-in-time recovery (30 days)
Read replicas for high availability
Storage Infrastructure
Primary: Supabase Storage
Backup: AWS S3 compatible storage
CDN: Integrated with Vercel Edge Network
Specifications:
500GB initial capacity
Automatic image optimization
Global distribution
Network Architecture
graph TB
    subgraph "Edge Network"
        CDN[Vercel CDN]
        EF[Edge Functions]
    end

    subgraph "Application Layer"
        WEB[NextJS App]
        API[API Routes]
        SA[Server Actions]
    end

    subgraph "Data Layer"
        DB[(PostgreSQL)]
        CACHE[(Redis)]
        STORAGE[Object Storage]
    end

    subgraph "External Services"
        AUTH[Clerk]
        PAY[MercadoPago]
        SMS[WhatsApp]
        APIS[Third-party APIs]
    end

    CDN --> WEB
    EF --> API
    WEB --> SA
    SA --> DB
    SA --> CACHE
    WEB --> STORAGE
    API --> AUTH
    API --> PAY
    API --> SMS
    API --> APIS

Server Requirements
Since the application is serverless, traditional server requirements are replaced by:
Function Requirements
Memory: 1GB-3GB per function
Timeout: 10s for API routes, 5m for background jobs
Concurrency: 1000 concurrent executions
Cold start optimization via Edge Functions
Database Connection Pooling
// lib/db/connection-pool.ts
import { Pool } from 'pg';
import { PrismaPg } from '@prisma/adapter-pg';

const connectionPool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20,                    // Maximum connections
  idleTimeoutMillis: 30000,   // 30 seconds
  connectionTimeoutMillis: 2000,
  ssl: {
    rejectUnauthorized: false
  }
});

// Prisma with connection pooling
export const prisma = new PrismaClient({
  adapter: new PrismaPg(connectionPool),
  datasources: {
    db: {
      url: process.env.DATABASE_URL
    }
  }
});

8.2 Deployment Strategy
CI/CD Pipeline Configuration
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  DATABASE_URL: ${{ secrets.DATABASE_URL }}
  NEXT_PUBLIC_SUPABASE_URL: ${{ secrets.NEXT_PUBLIC_SUPABASE_URL }}
  NEXT_PUBLIC_SUPABASE_ANON_KEY: ${{ secrets.NEXT_PUBLIC_SUPABASE_ANON_KEY }}

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run type checking
        run: npm run type-check

      - name: Run linting
        run: npm run lint

      - name: Run tests
        run: npm run test:ci

      - name: Run E2E tests
        run: npm run test:e2e

  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Run security audit
        run: npm audit --audit-level=high

      - name: Run OWASP dependency check
        uses: dependency-check/Dependency-Check_Action@main
        with:
          project: 'neo-rent'
          path: '.'
          format: 'HTML'

  build:
    needs: [test, security]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build application
        run: npm run build
        env:
          NEXT_TELEMETRY_DISABLED: 1

      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-files
          path: .next

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'

Environment Management
// config/environments.ts

export const environments = {
  development: {
    name: 'Development',
    url: 'http://localhost:3000',
    database: process.env.DATABASE_URL_DEV,
    features: {
      debugMode: true,
      mockPayments: true,
      emailPreview: true,
    }
  },

  staging: {
    name: 'Staging',
    url: 'https://staging.neo-rent.ar',
    database: process.env.DATABASE_URL_STAGING,
    features: {
      debugMode: false,
      mockPayments: true,
      emailPreview: false,
    }
  },

  production: {
    name: 'Production',
    url: 'https://neo-rent.ar',
    database: process.env.DATABASE_URL,
    features: {
      debugMode: false,
      mockPayments: false,
      emailPreview: false,
    }
  }
} as const;

// Environment-specific configuration
export function getConfig() {
  const env = process.env.NEXT_PUBLIC_ENV || 'development';
  return environments[env as keyof typeof environments];
}

Database Migration Strategy
// scripts/migrate.ts

import { execSync } from 'child_process';
import { PrismaClient } from '@prisma/client';

async function migrate() {
  const environment = process.env.NODE_ENV || 'development';

  console.log(`Running migrations for ${environment}`);

  try {
    // Run Prisma migrations
    execSync('npx prisma migrate deploy', {
      stdio: 'inherit',
      env: {
        ...process.env,
        DATABASE_URL: getDbUrl(environment)
      }
    });

    // Run data migrations
    await runDataMigrations();

    // Verify database state
    await verifyDatabaseIntegrity();

    console.log('Migrations completed successfully');
  } catch (error) {
    console.error('Migration failed:', error);

    // Rollback if needed
    if (environment === 'production') {
      await rollbackMigration();
    }

    process.exit(1);
  }
}

async function runDataMigrations() {
  const prisma = new PrismaClient();

  // Example: Update trust scores for existing users
  const version = await prisma.migration.findFirst({
    where: { name: 'add_trust_scores' }
  });

  if (!version) {
    await prisma.$transaction(async (tx) => {
      // Create trust scores for users without them
      const users = await tx.user.findMany({
        where: { trustScore: null }
      });

      for (const user of users) {
        await tx.trustScore.create({
          data: {
            userId: user.id,
            overall: 0,
            identityScore: 0,
            creditScore: 0,
            paymentScore: 0,
            reviewScore: 0
          }
        });
      }

      // Record migration
      await tx.migration.create({
        data: { name: 'add_trust_scores' }
      });
    });
  }
}

Deployment Procedures
Pre-deployment Checklist

 # scripts/pre-deploy.sh
#!/bin/bash

echo "Running pre-deployment checks..."

# Check for uncommitted changes
if [[ -n $(git status -s) ]]; then
  echo "Error: Uncommitted changes found"
  exit 1
fi

# Run tests
npm run test:ci

# Check bundle size
npm run analyze

# Verify environment variables
node scripts/verify-env.js

# Database backup
npm run db:backup

echo "Pre-deployment checks passed ✓"


Deployment Script

 // scripts/deploy.ts

import { deploy } from '@vercel/client';

async function deployToProduction() {
  // Create deployment
  const deployment = await deploy({
    name: 'neo-rent',
    env: {
      NEXT_PUBLIC_ENV: 'production',
      // Other env vars from .env.production
    },
    regions: ['gru1'], // São Paulo
    functions: {
      'api/*': {
        maxDuration: 10,
        memory: 3008
      }
    }
  });

  // Wait for deployment
  await waitForDeployment(deployment.id);

  // Run post-deployment tasks
  await runPostDeploymentTasks(deployment.url);

  // Notify team
  await notifyDeployment(deployment);
}

async function runPostDeploymentTasks(url: string) {
  // Warm up functions
  await warmUpFunctions(url);

  // Verify critical paths
  await verifyCriticalPaths(url);

  // Update monitoring
  await updateMonitoring(url);
}


Rollback Strategy

 // scripts/rollback.ts

async function rollbackDeployment(deploymentId: string) {
  // Get previous deployment
  const previous = await getPreviousDeployment();

  // Rollback database if needed
  if (hasDatabaseChanges(deploymentId)) {
    await rollbackDatabase(previous.migrationVersion);
  }

  // Revert to previous deployment
  await vercel.deployments.promote(previous.id);

  // Clear caches
  await clearCaches();

  // Notify team
  await notifyRollback(deploymentId, previous.id);
}


Configuration Management
// lib/config/index.ts

import { z } from 'zod';

// Configuration schema
const configSchema = z.object({
  app: z.object({
    name: z.string(),
    url: z.string().url(),
    env: z.enum(['development', 'staging', 'production']),
  }),

  database: z.object({
    url: z.string(),
    poolSize: z.number().default(20),
    ssl: z.boolean().default(true),
  }),

  auth: z.object({
    clerkPublishableKey: z.string(),
    clerkSecretKey: z.string(),
    sessionDuration: z.number().default(30 * 24 * 60 * 60 * 1000),
  }),

  storage: z.object({
    supabaseUrl: z.string().url(),
    supabaseAnonKey: z.string(),
    maxFileSize: z.number().default(10 * 1024 * 1024),
  }),

  integrations: z.object({
    mercadopago: z.object({
      accessToken: z.string(),
      publicKey: z.string(),
      webhookSecret: z.string(),
    }),
    whatsapp: z.object({
      phoneNumberId: z.string(),
      accessToken: z.string(),
    }),
  }),

  features: z.object({
    maintenanceMode: z.boolean().default(false),
    debugMode: z.boolean().default(false),
    rateLimit: z.object({
      enabled: z.boolean().default(true),
      maxRequests: z.number().default(100),
      windowMs: z.number().default(15 * 60 * 1000),
    }),
  }),
});

// Load and validate configuration
export const config = configSchema.parse({
  app: {
    name: 'NEO-RENT',
    url: process.env.NEXT_PUBLIC_APP_URL,
    env: process.env.NEXT_PUBLIC_ENV,
  },

  database: {
    url: process.env.DATABASE_URL,
    poolSize: parseInt(process.env.DB_POOL_SIZE || '20'),
    ssl: process.env.DB_SSL !== 'false',
  },

  // ... rest of configuration
});

// Feature flags
export const featureFlags = {
  newSearchUI: process.env.NEXT_PUBLIC_FF_NEW_SEARCH === 'true',
  aiPricing: process.env.NEXT_PUBLIC_FF_AI_PRICING === 'true',
  instantBooking: process.env.NEXT_PUBLIC_FF_INSTANT_BOOKING === 'true',
};

Summary
This technical specification provides a comprehensive blueprint for building NEO-RENT, a revolutionary rental platform for the Argentine market. The specification covers:
Architecture: Modern serverless architecture using NextJS, Supabase, and Vercel
Features: Complete implementation details for all five major features
Data Models: Comprehensive database schema with relationships and indexes
APIs: Both internal Server Actions and external integrations
Security: Multi-layered security approach following OWASP guidelines
UI/UX: Detailed design system and component specifications
Infrastructure: Scalable deployment strategy optimized for Latin America
The platform is designed to handle the unique requirements of the Argentine rental market while providing a modern, secure, and user-friendly experience for all stakeholders.
