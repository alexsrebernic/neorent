Features (MVP)
User Management & Multi-Role Authentication
Comprehensive user registration and verification system supporting four distinct roles (Owner, Tenant, Martillero, ALP) with document validation, credit checks, and trust scoring algorithms.
Tech Involved
Clerk (authentication & user management)
OCR service (DNI/CUIT validation)
BCRA/Veraz APIs (credit checks)
Prisma/Postgres (user data & trust scores)
Server Actions (validation workflows)
Main Requirements
Role-based access control across entire platform
Secure document upload and storage
Real-time external API integrations for validation
Trust score calculation algorithm
KYC compliance per Argentine regulations
Property Listing & Management
Streamlined property creation workflow with photo optimization, AI-powered pricing suggestions, and professional validation by ALPs.
Tech Involved
NextJS (listing wizard UI)
Cloudinary/S3 (image storage & compression)
AI/ML service (pricing suggestions)
Supabase Storage (property media)
Prisma (property data models)
Main Requirements
Large file upload handling with compression
Zone-based pricing data integration
Calendar availability management
First-listing validation workflow
Mobile-optimized photo capture
Visit Coordination & ALP Management
Core feature enabling automated assignment of property validators (ALPs) based on location, availability, and performance, with offline-capable mobile app.
Tech Involved
Geolocation services (assignment algorithm)
PWA/React Native (ALP mobile app)
IndexedDB/Service Workers (offline capability)
WebSockets/Supabase Realtime (live updates)
Queue system (visit scheduling)
Main Requirements
Offline-first mobile experience for poor connectivity areas
Real-time availability matching
GPS-based proximity calculations
Digital confirmation workflows
Automated fee calculations
Insurance & Financial Integration
Critical integration with caucion insurance providers and MercadoPago for all financial transactions, including automated commission distribution.
Tech Involved
Insurance partner APIs
MercadoPago SDK
Webhook handlers (payment notifications)
Background jobs (commission distribution)
Prisma transactions (financial records)
Main Requirements
Real-time insurance quote generation
Secure payment processing
Automated commission splits
Late payment tracking
Financial compliance reporting
Contract Generation & Legal Compliance
Template-based contract system with Martillero review requirements, electronic signatures, and automated tax calculations.
Tech Involved
Document generation service (PDFKit/Puppeteer)
E-signature integration (DocuSign/ZapSign)
Version control system
Stamp tax calculation APIs
Secure document storage
Main Requirements
Template management system
Mandatory review workflows
Multi-party signature coordination
Regional tax compliance (Neuquén/Río Negro)
Audit trail for all contract changes
System Diagram
graph TB
    subgraph "Client Layer"
        WEB[NextJS Web App]
        PWA[ALP Mobile PWA]
    end

    subgraph "API Gateway"
        VERCEL[Vercel Edge Functions]
        SA[Server Actions]
    end

    subgraph "Authentication"
        CLERK[Clerk Auth]
    end

    subgraph "Core Services"
        USERS[User Service]
        PROPS[Property Service]
        VISITS[Visit Coordination]
        CONTRACTS[Contract Service]
        FINANCE[Financial Service]
        COMM[Communication Hub]
    end

    subgraph "Data Layer"
        SUPABASE[(Supabase Postgres)]
        PRISMA[Prisma ORM]
        STORAGE[Supabase Storage]
        CACHE[Redis Cache]
    end

    subgraph "External Services"
        BCRA[BCRA API]
        VERAZ[Veraz/Nosis]
        MP[MercadoPago]
        INSURANCE[Insurance APIs]
        OCR[OCR Service]
        ESIGN[E-Signature]
        SMS[WhatsApp/SMS]
        MAPS[Maps/Geocoding]
    end

    subgraph "Background Jobs"
        QUEUE[Job Queue]
        WORKERS[Worker Processes]
    end

    WEB --> VERCEL
    PWA --> VERCEL
    VERCEL --> SA
    SA --> CLERK

    CLERK --> USERS

    SA --> USERS
    SA --> PROPS
    SA --> VISITS
    SA --> CONTRACTS
    SA --> FINANCE
    SA --> COMM

    USERS --> PRISMA
    PROPS --> PRISMA
    VISITS --> PRISMA
    CONTRACTS --> PRISMA
    FINANCE --> PRISMA
    COMM --> PRISMA

    PRISMA --> SUPABASE

    USERS --> OCR
    USERS --> BCRA
    USERS --> VERAZ

    PROPS --> STORAGE
    PROPS --> MAPS

    VISITS --> MAPS
    VISITS --> QUEUE

    CONTRACTS --> ESIGN
    CONTRACTS --> STORAGE

    FINANCE --> MP
    FINANCE --> INSURANCE
    FINANCE --> QUEUE

    COMM --> SMS

    QUEUE --> WORKERS
    WORKERS --> SUPABASE

    VISITS --> CACHE
    PROPS --> CACHE
