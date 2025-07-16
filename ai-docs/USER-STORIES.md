
Features List
User Management & Verification
Multi-role Registration & Verification System
User Stories
As a Property Owner, I want to quickly register and verify my identity, so that I can start listing properties within minutes
As a Tenant, I want to complete my profile with income verification, so that I can build trust and get better property matches
As a Martillero, I want to upload my professional license easily, so that I can start receiving qualified leads
As an ALP, I want to register as a contractor with minimal friction, so that I can start earning from property visits
As a Platform Admin, I want automated verification workflows, so that we maintain trust while scaling operations
UX/UI Considerations
Core Experience
Landing Page with Role Selection


Four prominent cards: "I'm an Owner" | "I'm Looking to Rent" | "I'm a Real Estate Professional" | "Become a Visit Agent"
Each card animates on hover with relevant benefit highlights
Progressive disclosure: clicking reveals role-specific value props before registration
Smart Registration Flow


Step 1: Basic info (email/phone/password) with WhatsApp verification option
Step 2: DNI/CUIT capture via camera with OCR auto-fill
Step 3: Role-specific requirements (income for tenants, property count for owners, license for Martilleros)
Progress indicator showing "2 minutes to complete"
Real-time validation with inline error correction
Verification Status Dashboard


Visual progress rings showing verification stages
Traffic light system: Red (pending), Yellow (processing), Green (verified)
Clear CTAs for missing documents
Estimated completion time for each verification type
Advanced Users & Edge Cases
Bulk Document Upload


Drag-and-drop interface for multiple documents
Automatic document type detection using AI
Batch processing with individual status tracking
Failed Verification Handling


Clear explanation of why verification failed
Direct link to support chat with pre-populated context
Alternative verification methods (manual review option)
Grace period notifications before account limitations
Property Management
Streamlined Property Listing Wizard
User Stories
As an Owner, I want to list my property in under 5 minutes, so that I don't abandon the process
As an Owner, I want AI to suggest optimal pricing, so that I can be competitive without research
As an Owner, I want to manage multiple properties efficiently, so that I can scale my portfolio
As a Tenant, I want to see accurate, complete property information, so that I don't waste time on unsuitable options
As an ALP, I want to access property details offline, so that I can conduct visits in areas with poor connectivity
UX/UI Considerations
Core Experience
Smart Listing Flow


Step 1: Address autocomplete with map pin placement
Step 2: Property basics via visual selector (house/apartment, bedrooms via +/- buttons)
Step 3: Photo upload with automatic compression and enhancement
Step 4: AI-powered pricing suggestion with market comparison visualization
Step 5: Availability calendar with blocked dates clearly marked
Photo Management


Drag-to-reorder functionality
Automatic duplicate detection
Smart cropping suggestions for hero images
Virtual staging AI option (premium feature)
Mandatory minimum of 5 photos with helpful prompts
Pricing Intelligence Display


Interactive slider showing price vs. demand curve
Neighborhood comparison chart
"Sweet spot" indicator for optimal pricing
Historical rental data overlay
Advanced Users & Edge Cases
Portfolio Management View


Grid/list toggle for multiple properties
Bulk editing capabilities
Performance metrics per property
Quick actions: pause/activate listings
Incomplete Listing Recovery


Auto-save every 30 seconds
Email reminder with one-click resume
Mobile handoff - start on desktop, finish on phone
Draft status clearly indicated
Search & Matching
AI-Powered Property Discovery
User Stories
As a Tenant, I want to find properties that match my budget AND location needs, so that I don't waste time on incompatible options
As a Tenant, I want to express interest with one click, so that I can apply to multiple properties quickly
As an Owner, I want to see pre-qualified tenant matches, so that I only review serious applicants
As a Tenant with pets, I want to filter pet-friendly properties immediately, so that I don't get rejected later
As a Vaca Muerta professional, I want to find temporary furnished rentals, so that I can relocate quickly for work
UX/UI Considerations
Core Experience
Search Interface


Map-first view with property pins
Sliding panel with results list
Real-time filter updates without page reload
Smart filters that adapt based on initial selection
"Draw search area" tool for custom boundaries
Match Score Visualization


Percentage match indicator on each card
Color-coded compatibility (green/yellow/orange)
Breakdown on hover: "85% match - Great location, slightly above budget"
"Why this match?" expandable explanation
One-Click Express Interest


Prominent button with loading state
Instant confirmation animation
Counter showing "23 others interested"
Queue position indicator for high-demand properties
Advanced Users & Edge Cases
Saved Searches & Alerts


Custom alert frequency settings
Multiple saved search management
Price drop notifications
New listing alerts with preview
Advanced Filtering


Commute time calculator from work address
School district boundaries overlay
Crime statistics heat map (if available)
Building age and renovation filters
Visit Coordination
Automated Visit Scheduling System
User Stories
As a Tenant, I want to schedule property visits at convenient times, so that I can view multiple properties efficiently
As an ALP, I want to receive visit assignments based on my location, so that I can maximize earnings with minimal travel
As an Owner, I want to track who visited my property and when, so that I have security and can follow up
As an ALP, I want to complete visit reports offline, so that connectivity issues don't affect my work
As a Platform, I want to ensure visit quality, so that both owners and tenants have good experiences
UX/UI Considerations
Core Experience
Visit Request Flow (Tenant)


Calendar view with available slots
Batch scheduling: "Add another visit" option
Route optimization suggestion: "These 3 properties can be visited in one trip"
Instant confirmation with ALP assignment
ALP Mobile Interface


Today's visits in chronological order
Map view with optimized route
One-tap navigation to property
Offline mode indicator with sync status
Visit Execution Checklist


Pre-visit: Confirm tenant arrival
During: Photo checklist (kitchen, bathrooms, bedrooms, damages)
Post-visit: Quick rating and notes
Automatic fee calculation display
Advanced Users & Edge Cases
Bulk Visit Management (ALP)


Week view with earnings projection
Visit swap requests with other ALPs
Performance metrics dashboard
Preferred zones setting
No-Show Handling


Automated wait time (15 min)
Photo proof of arrival
Cancellation fee logic
Rescheduling workflow
Insurance Integration
Seamless Guarantor Replacement
User Stories
As a Tenant, I want to get instant insurance quotes, so that I can budget accurately
As an Owner, I want to verify insurance validity, so that I'm protected against defaults
As a Tenant, I want to compare different coverage options, so that I can choose what fits my budget
As an Insurance Partner, I want clean API integration, so that quotes are accurate and policies are issued instantly
As a Platform, I want to track insurance adoption rates, so that we can optimize the funnel
UX/UI Considerations
Core Experience
Insurance Quote Flow


Triggered automatically after express interest
Simple slider: "Choose your coverage level"
Real-time price calculation
Side-by-side comparison: Traditional guarantor vs. Insurance
"What's covered?" expandable sections
Purchase Integration


In-line payment form (no redirect)
Monthly payment option prominently displayed
Instant policy generation
Download PDF + email confirmation
Visual confirmation: "You're covered!" with confetti animation
Advanced Users & Edge Cases
Policy Management


Coverage upgrade options
Cancellation flow with refund calculation
Claims initiation interface
Policy transfer between properties
Alternative Insurance Providers


Multiple quote comparison
Provider ratings and reviews
Coverage difference matrix
"Why we recommend" explanations
Contract & Legal
Digital Contract Execution
User Stories
As an Owner, I want contracts generated automatically with correct legal terms, so that I'm protected
As a Martillero, I want to review and approve contracts before signature, so that I fulfill my legal obligations
As a Tenant, I want to understand all costs upfront, so that there are no surprises
As Both Parties, I want secure digital signatures, so that contracts are legally binding without meeting in person
As a Platform, I want to ensure compliance with Neuquén/Río Negro regulations, so that we avoid legal issues
UX/UI Considerations
Core Experience
Contract Generation Wizard


Auto-populated from property and party data
Editable fields highlighted in blue
Mandatory Martillero assignment with explanation
Fee breakdown in visual format (pie chart)
"Generate Contract" with preview before sending
Review & Signature Flow


Split-screen: contract + comments panel
Highlighting tool for discussion points
Version control with change tracking
Sequential signature workflow with status
Mobile-optimized signature pad
Cost Transparency Display


Interactive breakdown: hover for explanations
"What you pay today" vs. "Monthly costs"
Savings comparison vs. traditional rental
No hidden fees guarantee badge
Advanced Users & Edge Cases
Contract Customization


Clause library for special conditions
Template saving for repeat landlords
Bilingual contract option
Addendum workflow
Signature Issues


Identity verification fallback
Witness requirement handling
Manual signature upload option
Legal representation workflow
