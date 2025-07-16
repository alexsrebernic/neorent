NEO-RENT Design System
Color Palette
Primary Colors
Primary Blue - #1A73E8 (Primary brand color for key actions and brand identity)
Primary White - #FFFFFF (Clean surfaces and content areas)
Secondary Colors
Secondary Blue Light - #4285F4 (Hover states and interactive elements)
Secondary Blue Pale - #E8F0FE (Subtle backgrounds and selection states)
Secondary Gray - #5F6368 (Secondary text and icons)
Accent Colors
Accent Teal - #00ACC1 (Property highlights and special features)
Accent Purple - #7B1FA2 (Premium features and insurance elements)
Accent Orange - #F57C00 (Notifications and time-sensitive items)
Functional Colors
Success Green - #1E8E3E (Confirmations and positive states)
Error Red - #D93025 (Errors and critical alerts)
Warning Amber - #F9AB00 (Warnings and cautions)
Info Blue - #1967D2 (Informational messages)
Background Colors
Background Primary - #FFFFFF (Main content areas)
Background Secondary - #F8F9FA (App background and subtle separation)
Background Tertiary - #F1F3F4 (Disabled states and inactive areas)
Background Dark - #202124 (Dark mode primary)
Typography
Font Family
Primary Font: Inter (Universal web/app font)
Secondary Font: SF Pro Display (iOS) / Roboto (Android)
Monospace: JetBrains Mono (Contract numbers, IDs)
Font Weights
Light: 300
Regular: 400
Medium: 500
Semibold: 600
Bold: 700
Text Styles
Headings
H1: 32px/40px, Bold, Letter spacing -0.5px
Screen titles and major section headers
H2: 28px/36px, Semibold, Letter spacing -0.3px
Property titles and page headers
H3: 24px/32px, Semibold, Letter spacing -0.2px
Card headers and subsections
H4: 20px/28px, Medium, Letter spacing -0.1px
List headers and minor sections
Body Text
Body Large: 18px/28px, Regular, Letter spacing 0px
Property descriptions and key information
Body: 16px/24px, Regular, Letter spacing 0px
Standard UI text and content
Body Small: 14px/20px, Regular, Letter spacing 0.1px
Supporting information and metadata
Special Text
Caption: 12px/16px, Medium, Letter spacing 0.3px
Labels, timestamps, and micro-copy
Button Text: 16px/24px, Medium, Letter spacing 0.5px
All interactive button labels
Link Text: 16px/24px, Medium, Primary Blue (#1A73E8)
Inline links and clickable elements
Price Display: 24px/32px, Bold, Letter spacing -0.2px
Rental prices and financial figures
Component Styling
Buttons
Primary Button
Background: Primary Blue (#1A73E8)
Text: White (#FFFFFF)
Height: 48dp
Corner Radius: 24dp
Padding: 24dp horizontal, 12dp vertical
Shadow: 0 1px 2px rgba(60,64,67,0.3), 0 1px 3px rgba(60,64,67,0.15)
Secondary Button
Border: 1dp Secondary Gray (#5F6368)
Text: Primary Blue (#1A73E8)
Background: White (#FFFFFF)
Height: 48dp
Corner Radius: 24dp
Padding: 24dp horizontal, 12dp vertical
Ghost Button
Text: Primary Blue (#1A73E8)
Background: Transparent
Height: 40dp
Padding: 16dp horizontal
Cards
Property Card
Background: White (#FFFFFF)
Shadow: 0 1px 2px rgba(60,64,67,0.1), 0 2px 6px rgba(60,64,67,0.05)
Corner Radius: 16dp
Padding: 16dp
Border: 1dp transparent (2dp Primary Blue on hover)
Information Card
Background: Background Secondary (#F8F9FA)
Corner Radius: 12dp
Padding: 20dp
No shadow
Input Fields
Standard Input
Height: 56dp
Corner Radius: 8dp
Border: 1dp #DADCE0
Active Border: 2dp Primary Blue (#1A73E8)
Background: White (#FFFFFF)
Padding: 16dp horizontal
Label: 14px, Medium, Secondary Gray (#5F6368)
Text Area
Min Height: 120dp
Corner Radius: 8dp
Border: 1dp #DADCE0
Padding: 16dp
Icons
Icon Sizes
Extra Small: 16dp x 16dp (inline icons)
Small: 20dp x 20dp (list items)
Default: 24dp x 24dp (standard UI)
Large: 32dp x 32dp (feature icons)
Extra Large: 48dp x 48dp (empty states)
Icon Colors
Interactive: Primary Blue (#1A73E8)
Inactive: Secondary Gray (#5F6368)
Success: Success Green (#1E8E3E)
Error: Error Red (#D93025)
Spacing System
4dp - Micro (inline elements, icon-to-text)
8dp - Small (compact spacing within components)
12dp - Compact (list items, form fields)
16dp - Default (standard component padding)
24dp - Medium (section spacing)
32dp - Large (major section breaks)
48dp - Extra Large (page margins on tablet)
64dp - Jumbo (hero sections)
Motion & Animation
Timing Functions
Standard Ease: cubic-bezier(0.4, 0.0, 0.2, 1)
Deceleration: cubic-bezier(0.0, 0.0, 0.2, 1)
Acceleration: cubic-bezier(0.4, 0.0, 1, 1)
Sharp: cubic-bezier(0.4, 0.0, 0.6, 1)
Duration
Micro: 100ms (hover states, ripples)
Short: 200ms (standard transitions)
Medium: 300ms (page transitions, modals)
Long: 400ms (complex animations)
Animation Types
Fade: opacity transitions for content changes
Scale: 0.95 to 1.0 for cards and buttons
Slide: translateY for dropdowns and panels
Spring: tension 200, friction 20 for delightful interactions
Dark Mode Variants
Backgrounds
Dark Primary: #202124
Dark Surface: #292A2D
Dark Elevated: #35363A
Colors
Dark Primary Blue: #8AB4F8
Dark Text Primary: #E8EAED
Dark Text Secondary: #9AA0A6
Dark Borders: #3C4043
Shadows
Dark shadows: Use transparency overlays instead of drop shadows
Elevation: Lighter background colors for raised surfaces

I'll create comprehensive design briefs for each feature of NEO-RENT, following your specified format and incorporating the design system guidelines.
User Management & Verification
Landing Page with Role Selection
Landing Page - Initial Load State
Hero section with animated gradient background transitioning between Secondary Blue Pale (#E8F0FE) to white
Headline "Alquilá sin garantía, con transparencia total" in H1 (32px, Bold, #202124)
Subheadline in Body Large (18px, Regular, #5F6368) explaining DNU 70/2023 benefits
Four role cards arranged in a 2x2 grid on desktop, vertical stack on mobile
Each card has white background (#FFFFFF) with subtle shadow (0 1px 2px rgba(60,64,67,0.1))
Card contents: Extra Large icon (48dp), role title in H4 (20px, Medium), brief description in Body Small (14px)
Cards have 16dp corner radius and 24dp internal padding
Micro-animation: cards fade in sequentially with 100ms delay between each
Footer CTA "¿Ya tenés cuenta? Iniciá sesión" in Link Text style
Landing Page - Card Hover State
Card scales to 1.02x with Spring animation (tension 200, friction 20)
Shadow deepens to (0 2px 6px rgba(60,64,67,0.2))
Border appears: 2dp Primary Blue (#1A73E8)
Icon color transitions from Secondary Gray to Primary Blue with 200ms ease
Three benefit bullets slide up from bottom with opacity fade (0 to 1)
Background subtle gradient overlay appears (white to Secondary Blue Pale)
Cursor changes to pointer
Landing Page - Card Clicked State
Card scales down to 0.98x momentarily (100ms)
Ripple effect emanates from click point in Primary Blue at 15% opacity
Other cards fade to 60% opacity and blur slightly (2px)
Clicked card expands to show role-specific value propositions
Expansion animation: 300ms with deceleration curve
Primary Button appears: "Comenzar registro" with standard button styling
Secondary Ghost Button: "Ver más información"
Registration Flow
Registration - Step 1 (Basic Information)
Clean white form container with 32dp padding
Progress indicator at top: three dots connected by line, first dot Primary Blue, others Secondary Gray
"Paso 1 de 3: Información básica" in Caption style above progress
Form title "Creá tu cuenta" in H2 (28px, Semibold)
Input fields with floating labels (transform and scale animation on focus)
Email field with real-time validation (green checkmark appears on valid email)
Phone field with country code selector (+54 default)
Password field with strength indicator (colored bar below: red/amber/green)
Toggle for "Verificar con WhatsApp" with Switch component
Primary Button "Continuar" disabled until all fields valid
Fields animate in with staggered fade (50ms between each)
Registration - Step 2 (Document Capture)
Camera viewfinder interface for DNI/CUIT capture
Overlay guide showing document placement outline
"Posicioná tu DNI dentro del marco" instruction in white text with dark overlay
Capture button (64dp) with camera icon, Primary Blue background
After capture: document preview with extracted data below
OCR-filled fields highlighted in Secondary Blue Pale (#E8F0FE)
Editable fields with edit icon (16dp) on the right
"Verificando documento..." loader with spinning Progress indicator
Success state: Success Green checkmark with subtle bounce animation
Registration - Step 3 (Role-Specific Requirements)
Dynamic form based on selected role
For Owners: "¿Cuántas propiedades querés publicar?" with numeric stepper
For Tenants: Income verification upload area with drag-drop zone
For Martilleros: License number input with automatic format mask
Drag-drop zones with dashed border (2dp, #DADCE0)
On drag: border becomes solid Primary Blue, background shifts to Secondary Blue Pale
Upload progress with linear progress bar below each file
Completion celebration: confetti animation with "¡Bienvenido a NEO-RENT!"
Verification Dashboard
Dashboard - Overview State
Grid layout with verification status cards
Each card shows circular progress ring (120dp diameter)
Progress animation: ring draws clockwise over 1000ms on load
Color coding: Error Red (pending), Warning Amber (processing), Success Green (verified)
Percentage in center of ring (H3 size, Bold)
Status label below in Caption style
Pending items have pulsing dot animation (scale 1 to 1.2, 2s loop)
Action buttons appear on hover with slide-up animation
Dashboard - Document Upload State
Modal overlay with frosted glass effect (backdrop-filter: blur(8px))
Upload zone expands when files dragged near (scale 1 to 1.05)
Multiple file preview grid with 4dp spacing
Each file shows thumbnail, name, size, and status
Processing files show circular progress overlay on thumbnail
Success files get checkmark badge with pop animation
Failed files show error icon with shake animation (translateX ±4px)
Batch actions bar slides up when multiple files selected
Property Management
Property Listing Wizard
Listing Wizard - Address Entry
Full-screen map as background with 70% white overlay
Floating form card (max-width 480px) centered
Search input with location icon, expands on focus
Autocomplete dropdown with hover states (Secondary Blue Pale background)
Map dynamically zooms and pans to selected location
Pin drops with bounce animation (translateY with spring physics)
"Confirmá la ubicación" Primary Button appears after pin placement
Fine-tune controls: "Mover pin" ghost button with drag cursor
Listing Wizard - Property Details
Segmented control for property type (Casa/Departamento)
Visual selector cards with illustrations for each type
Selected card gets Primary Blue border and scale 1.05
Room count with large +/- buttons (48dp) and number display
Animated number transitions (flip animation like mechanical counter)
Amenities grid with icon tiles (3 columns on mobile, 6 on desktop)
Tile press: background fills with Primary Blue, icon turns white
Surface area input with m² suffix, number keyboard on mobile
Progress saved indicator with cloud icon animation
Listing Wizard - Photo Upload
Large drop zone with camera icon and instructional text
Grid preview (3 columns) with drag handles for reordering
First photo labeled "Foto principal" with star badge
Drag animation: photo lifts with shadow, others shift smoothly
Upload progress overlay with percentage and cancel button
AI enhancement toggle with "before/after" preview slider
Warning for less than 5 photos: Warning Amber banner slides down
Photo editor modal: crop, rotate, brightness controls
Listing Wizard - AI Pricing
Split screen: left shows your inputs, right shows AI suggestion
Animated chart building: bars grow from bottom over 800ms
Price slider with neighborhood comparison overlay
"Sweet spot" indicator pulses with Success Green glow
Tooltip on hover shows "83% de ocupación esperada"
Price adjustment: real-time demand curve update
Historical data toggle: chart morphs with smooth transition
"Aceptar sugerencia" Primary Button, "Ajustar manualmente" Secondary
Listing Wizard - Availability Calendar
Calendar grid with touch-friendly date cells (44dp minimum)
Today highlighted with Primary Blue border
Blocked dates with diagonal line pattern
Date range selection: drag from start to end
Selected range fills with Secondary Blue Pale
Quick presets: "Disponible ahora", "Desde el próximo mes"
Recurring availability patterns with repeat icon
Save state: calendar fades slightly with checkmark overlay
Portfolio Management
Portfolio - Grid View
Responsive grid: 1 column mobile, 2 tablet, 3-4 desktop
Property cards with 16:9 image ratio
Image carousel on hover with dot indicators
Status badge overlay: "Activo" (Success Green), "Pausado" (Warning Amber)
Quick stats bar: views, inquiries, occupancy rate
Star rating display with filled/empty stars
Action menu (three dots) reveals on hover/touch
Bulk select mode: checkboxes appear with slide animation
Selected cards get Primary Blue border
Portfolio - Performance Analytics
Tab navigation: "Vista general", "Rendimiento", "Inquilinos"
Animated line charts with touch tooltips
Chart lines draw from left to right on load (1000ms)
Data points pulse when near cursor/touch
Key metrics cards with trend arrows (up/down)
Metric animations: numbers count up from 0
Comparison mode: overlay previous period in gray
Export button generates PDF with progress animation
Search & Matching
Map-Based Search
Search - Initial Map View
Full-screen map with search overlay bar at top
Search bar with glass morphism effect (blur + transparency)
Filter pills below search: "Precio", "Habitaciones", "Más filtros"
Property pins cluster when zoomed out with count badges
Cluster tap: smooth zoom to reveal individual pins
Pin design: teardrop shape with price inside
Selected pin: scales up with spring animation, shows preview card
Map controls: zoom, location, draw area tool
Search - Results Panel Slide
Panel slides in from right (desktop) or bottom (mobile)
Handle bar for drag to resize/dismiss
Results count with animated number transition
Sort dropdown with options: "Relevancia", "Precio", "Más nuevo"
Property cards with lazy-loaded images
Skeleton screens while loading (animated shimmer effect)
Infinite scroll with spinner at bottom
Pull-to-refresh on mobile with NEO-RENT logo animation
Search - Property Match Cards
Match percentage badge with circular progress visualization
Color gradient: Success Green (80%+), Warning Amber (60-79%), Error Red (<60%)
Match factors on hover: location ✓, price ⚠, size ✓
"Express interest" Primary Button with heart icon
Button press: heart fills and scales with haptic feedback
"23 interesados" counter increments in real-time
Save button: bookmark icon with fill transition
Quick preview: long press shows modal with key details
Advanced Filtering
Filter Modal - Expanded State
Full-screen modal with "X" close button
Grouped sections: "Básicos", "Ubicación", "Características", "Extras"
Price range slider with dual handles
Histogram showing property distribution behind slider
Room selector with visual icons (bed, bath, parking)
Toggle switches for boolean filters (pet-friendly, furnished)
Commute time calculator with work address input
Interactive map for area selection with polygon drawing
"Ver X propiedades" sticky Primary Button updates live
Reset button appears when filters applied
Visit Coordination
Visit Scheduling
Schedule - Calendar Interface
Month view with available dates highlighted
Available slots shown as green dots under dates
Selected date expands to show time slots
Time slots in 30-minute increments with ALP avatar
Booked slots grayed out with strikethrough
Multiple selection mode: checkboxes appear
Selected visits appear in floating cart with count badge
"Confirmar visitas" Primary Button in cart
Route optimization suggestion banner when 3+ selected
Schedule - ALP Assignment View
Split view: calendar left, ALP list right
ALP cards with photo, rating, completed visits count
Availability indicator: green (available), amber (busy), gray (offline)
Distance from property shown with route preview
Auto-assignment algorithm visualization
Manual override with drag-drop ALP to time slot
Confirmation modal with visit details summary
SMS/WhatsApp notification preview
ALP Mobile Interface
ALP App - Today View
Pull-down header with earnings summary
Visit cards in chronological order with time badges
Swipe actions: navigate, call, cancel
Current visit highlighted with pulsing border
Map button shows optimized route
Offline indicator banner when no connection
Sync status with progress bar when reconnecting
Check-in button becomes active when near property
ALP App - Visit Execution
Step-by-step checklist interface
Camera integration for required photos
Photo requirements with example images
Green checkmarks appear as tasks complete
Timer showing visit duration
Notes field with voice-to-text option
Damage report mode with annotation tools
Submit button disabled until all required tasks done
Insurance Integration
Insurance Quote Flow
Insurance - Coverage Selection
Three coverage tiers in card layout
"Básico", "Completo", "Premium" with feature lists
Recommended tier highlighted with "Más popular" badge
Interactive slider to adjust coverage amount
Price updates in real-time with smooth transitions
"¿Qué cubre?" expandable sections with animations
Comparison toggle: "vs. Garantía tradicional"
Monthly payment breakdown with calculator icon
"Continuar" Primary Button leads to payment
Insurance - Payment Integration
In-line payment form, no external redirect
Card input with real-time validation and brand detection
Security badges with SSL and encryption icons
Monthly vs. annual payment toggle with savings badge
Price animation when switching payment terms
Processing state: form disabled with spinner overlay
Success state: checkmark animation with confetti
Policy document appears with download button
"Volver a la propiedad" Secondary Button
Contract & Legal
Digital Contract Flow
Contract - Generation Preview
Split-screen layout: form left, preview right
Live preview updates as fields are edited
Highlighted editable fields in preview (blue glow)
Martillero assignment dropdown with professional badges
Fee breakdown pie chart with hover details
Interactive tooltips explaining each fee
"Vista previa completa" link opens modal
"Generar contrato" Primary Button at bottom
Contract - Review & Signature
Document viewer with zoom controls
Comment sidebar with threaded discussions
Text highlighting tool with color options
Version history dropdown with change diff view
Signature pad with pressure sensitivity
"Practicar firma" option with reset button
Sequential signature workflow with status icons
Progress bar showing signature completion
Final confirmation modal with summary
Contract - Completion State
Success animation with document icon
"Contrato firmado exitosamente" message
Download options: PDF, original, certificate
Email confirmation preview
Next steps checklist with linked actions
"Ver en mis contratos" Primary Button
Share options for other party
Calendar integration for important dates
Each screen state includes thoughtful micro-interactions, accessibility considerations, and follows the NEO-RENT design system consistently throughout the user journey.
