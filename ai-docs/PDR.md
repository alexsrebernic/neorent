Elevator Pitch
NEO-RENT is the first integrated rental marketplace in Argentina that leverages the DNU 70/2023 deregulation to offer transparent, lower-cost rentals by connecting verified owners and tenants with licensed real estate professionals, while replacing traditional guarantors with modern insurance solutions and automating the entire rental lifecycle.
Problem Statement
The Argentine rental market suffers from:
High transaction costs (traditionally 1 month commission + guarantor requirements)
Lack of trust between parties due to insufficient verification systems
Complex bureaucratic processes requiring multiple in-person interactions
Extreme difficulty finding property guarantors (traditional Argentine requirement)
Poor post-rental management leading to disputes
Fragmented communication between all stakeholders
Limited professional support for self-managing owners
Inefficient property showing logistics
Target Audience
Primary Users:
Property Owners in Neuquén/Río Negro (especially those managing independently)
Tenants (professionals in Vaca Muerta ecosystem, students, families)
Licensed Real Estate Agents (Martilleros) seeking lead generation
Secondary Users:
Logistic Support Agents (ALPs) - freelance contractors for property visits
Insurance Companies (caucion bond providers)
USP
We're the only platform that combines:
30-60% lower closing costs than traditional agencies
Instant insurance-based guarantor replacement
AI-verified tenant scoring with BCRA integration
On-demand property showing through contractor network
Legally compliant digital contract execution
Transparent commission structure post-DNU 70/2023
Target Platforms
[ ] Web Application (responsive, mobile-first)
[ ] Progressive Web App for ALPs (offline capability essential)
[ ] Native Mobile Apps (Phase 2 - post MVP validation)
Features List
User Management & Verification
[ ] Multi-role registration (Owner, Tenant, Martillero, ALP)
[ ] DNI/CUIT validation through OCR
[ ] Professional license validation for Martilleros
[ ] Monotributista validation for ALPs
[ ] Automated BCRA credit check integration
[ ] Optional Veraz/Nosis integration (paid by applicant)
[ ] Trust score algorithm based on:
[ ] Credit history (BCRA/Bureaus)
[ ] Income verification
[ ] Platform behavior history
[ ] Document completeness
Property Management
[ ] Streamlined property listing wizard
[ ] Photo upload with compression
[ ] AI-suggested pricing based on zone data
[ ] Availability calendar
[ ] Property validation by ALP on first listing
Search & Matching
[ ] Location-based search with map view
[ ] Price range and basic filters (bedrooms, pet-friendly)
[ ] AI matching algorithm prioritizing:
[ ] Tenant score vs owner requirements
[ ] Price affordability (income/rent ratio)
[ ] Location preferences
[ ] "Express Interest" one-click application
Visit Coordination (Core MVP Feature)
[ ] ALP assignment algorithm based on:
[ ] Geographic proximity
[ ] Availability windows
[ ] Performance rating
[ ] Visit scheduling interface
[ ] ALP mobile app features:
[ ] Offline mode for areas with poor connectivity
[ ] Visit checklist
[ ] Photo/video documentation
[ ] Digital visit confirmation
[ ] Automated visit fee calculation for ALPs
Insurance Integration (Critical MVP Feature)
[ ] API integration with caucion insurance partner
[ ] Real-time quote generation
[ ] In-flow insurance purchase
[ ] Policy document storage
[ ] Automatic policy validation for contracts
Contract & Legal
[ ] Template-based contract generation
[ ] Automatic stamp tax calculation (Neuquén/Río Negro)
[ ] Mandatory Martillero review workflow
[ ] Electronic signature integration (Docusign/Clicky/ZapSign)
[ ] Contract storage with version control
[ ] Clear fee disclosure (closing costs breakdown)
Financial Management
[ ] MercadoPago exclusive integration
[ ] Transparent fee collection:
[ ] Closing fee (30-60% of one month's rent)
[ ] Monthly management fee (Premium plan)
[ ] Automated commission distribution:
[ ] Platform fee
[ ] Martillero commission
[ ] ALP visit fees
[ ] Monthly rent collection and distribution
[ ] Late payment notifications
Communication Hub
[ ] In-app messaging between all parties
[ ] WhatsApp notification integration
[ ] Document sharing with role-based access
[ ] Automated status updates
Post-Rental Management
[ ] Maintenance request system
[ ] ALP dispatch for inspections
[ ] Issue documentation with photos
[ ] Basic dispute logging
Analytics & Reporting
[ ] Owner dashboard:
[ ] Occupancy status
[ ] Payment history
[ ] Upcoming renewals
[ ] Basic revenue reports
[ ] Tenant payment tracking
UX/UI Considerations
[ ] Onboarding flows by user type:
[ ] Owner: Property listing focus
[ ] Tenant: Search and application focus
[ ] Martillero: Lead management focus
[ ] ALP: Task management focus
[ ] Mobile-first design principles
[ ] Progressive disclosure for complex forms
[ ] Status-based UI states:
[ ] Document pending verification
[ ] Credit check in progress
[ ] Insurance quote ready
[ ] Contract awaiting signature
[ ] Trust indicators throughout:
[ ] Verification badges
[ ] Credit scores (when available)
[ ] Platform history metrics
Non-Functional Requirements
[ ] Performance: <3 second load times on 3G
[ ] Offline capability for ALP app
[ ] Security:
[ ] SSL/TLS encryption
[ ] Secure document storage
[ ] Role-based access control
[ ] Compliance:
[ ] Clear Terms & Conditions defining platform role
[ ] Privacy policy aligned with Argentine law
[ ] KYC data collection per UIF requirements
[ ] Scalability: Architecture ready for multi-province expansion
Monetization
Revenue Model:
Closing Fee: 30-60% of first month's rent (significantly less than traditional 100%)


Platform retains majority
Martillero receives their portion
Service Plans:


Basic (Freemium): One-time setup fee for self-managing owners
Premium: 5-8% monthly management fee
Ancillary Revenue:


Insurance commission share with partners
Express verification fees (Veraz/Nosis pass-through + markup)
Future: Featured listings, virtual staging
Cost Structure:
ALP payments (per visit)
Payment gateway fees (MercadoPago)
Third-party integrations (signatures, credit checks)
Infrastructure and development
Critical Implementation Notes
Legal Foundation:
Company structure consultation required immediately
Terms & Conditions must explicitly state:
NEO-RENT is a technology platform
Martilleros are the licensed professionals conducting real estate activities
Fee structure and transparency
Data usage and privacy
ALP Network Building:
Create simple online application form
Develop standardized training materials (video/PDF)
Require proof of:
Monotributista registration
Personal liability insurance
Identity verification
Payment per successful visit via MercadoPago
Insurance Partnership:
Priority #1 after legal structure
Negotiate API access and commission structure
Ensure instant quote generation capability
Handle policy issuance within platform flow
MVP Development Priorities:
Core user registration and verification
Property listing and search
Insurance integration
Visit coordination system
Contract generation and signature
Payment processing
Out of Scope for MVP:
AFIP integration
Multiple payment gateways
Commercial properties
Utility transfers
Complex maintenance workflows
Mobile native apps
Success Metrics:
Properties listed
Successful rentals closed
Average closing time reduction
Cost savings vs traditional model
User satisfaction scores
ALP network size and utilization
