# Design Guidelines: 3D Airport Navigation App

## Architecture Decisions

### Authentication
**No Authentication Required**
- This is a utility app focused on airport navigation with locally stored data
- Include a **Profile/Settings screen** with:
  - User-customizable avatar (generate 3 preset aviation-themed avatars: pilot, traveler, flight attendant style)
  - Display name field
  - App preferences:
    - Measurement units (meters/feet)
    - Default floor view (arrival/departure)
    - 3D rendering quality (low/medium/high)
    - Offline map downloads toggle

### Navigation Architecture
**Tab Navigation (3 tabs)**
- **Home Tab**: Airport search and browse
- **Saved Tab**: Bookmarked airports and recent searches (center position)
- **Profile Tab**: Settings and preferences

**Modal/Pushed Screens**:
- Airport Details (pushed from Home/Saved)
- 3D Map View (full-screen modal from Airport Details)
- Navigation Directions (overlay within 3D Map View)

### Screen Specifications

#### 1. Home Screen (Airport Search)
- **Purpose**: Discover and search for airports worldwide
- **Layout**:
  - Transparent header with search bar
  - Top inset: headerHeight + Spacing.xl
  - Bottom inset: tabBarHeight + Spacing.xl
  - Scrollable main content area
- **Components**:
  - Prominent search bar in header (search by name, code, city)
  - Filter chips below search (Region, Size, Services)
  - Airport cards in scrollable list showing:
    - Airport name and IATA code
    - City and country
    - Terminal count indicator
    - Bookmark icon (right side)
  - Pull-to-refresh functionality

#### 2. Airport Details Screen
- **Purpose**: Overview of selected airport with option to view 3D map
- **Layout**:
  - Default navigation header with back button (left) and bookmark button (right)
  - Top inset: Spacing.xl
  - Bottom inset: tabBarHeight + Spacing.xl
  - Scrollable content
  - Floating action button: "View 3D Map" (bottom-right with drop shadow)
- **Components**:
  - Hero section with airport name, code, and location
  - Quick stats cards (terminals, gates, annual passengers)
  - List of available floors/levels
  - Key amenities grid (restaurants, shops, lounges, transportation)

#### 3. 3D Map View Screen (Full-Screen Modal)
- **Purpose**: Interactive 3D navigation of airport terminal
- **Layout**:
  - Custom transparent header with:
    - Close button (left) - X icon
    - Floor selector (center) - dropdown showing current floor
    - Settings button (right) - 3D view options
  - Top inset: headerHeight + Spacing.sm
  - Bottom inset: insets.bottom + Spacing.sm
  - Full-screen 3D viewport (non-scrollable)
  - Floating search bar (top, below header)
  - Floating controls panel (bottom):
    - Zoom controls
    - Reset view button
    - "Get Directions" button
- **Components**:
  - 3D scene with pan, pinch-zoom, and rotate gestures
  - Location markers with icons (gates, restrooms, food, shops, security)
  - Path visualization when navigation is active
  - Location info cards on marker tap

#### 4. Saved Airports Screen
- **Purpose**: Quick access to bookmarked and recently viewed airports
- **Layout**:
  - Transparent header with title "Saved"
  - Top inset: headerHeight + Spacing.xl
  - Bottom inset: tabBarHeight + Spacing.xl
  - Scrollable list
- **Components**:
  - Segmented control: "Bookmarks" / "Recent"
  - Same airport card style as Home screen
  - Swipe-to-delete for bookmarks
  - Empty state with icon and text when no saved airports

#### 5. Profile Screen
- **Purpose**: User customization and app settings
- **Layout**:
  - Transparent header with title "Profile"
  - Top inset: headerHeight + Spacing.xl
  - Bottom inset: tabBarHeight + Spacing.xl
  - Scrollable form
- **Components**:
  - Avatar selector (circular, large) with 3 preset options
  - Display name text input
  - Settings sections:
    - Display preferences (units, default views)
    - 3D rendering quality
    - Offline maps management
    - About section (version, privacy policy, terms)

## Design System

### Color Palette
**Primary**: Aviation Blue (#0066CC) - for primary actions, selected states, navigation paths
**Secondary**: Sky Blue (#4A90E2) - for secondary elements, accents
**Background**: 
- Light mode: White (#FFFFFF) base, Light Blue (#EBF4FC) for cards, Soft Blue (#D6E8F8) for elevated areas
- Dark mode: Dark Gray (#1C1C1E), Charcoal (#2C2C2E) for cards
**Text**: 
- Primary: Black (#000000) / White (#FFFFFF)
- Secondary: Blue-Gray (#5A6A7A) in light mode, Gray (#8E8E93) in dark mode
**Borders**: Light Blue (#C5DDF0) in light mode for a cohesive blue theme
**Semantic Colors**:
- Gate Marker: Green (#34C759)
- Food/Restaurant: Orange (#FF9500)
- Restroom: Blue (#007AFF)
- Security: Red (#FF3B30)
- Shopping: Purple (#AF52DE)

### Typography
- **Headlines**: SF Pro Display (iOS) / Roboto (Android), Bold, 28pt
- **Titles**: SF Pro Text / Roboto, Semibold, 20pt
- **Body**: SF Pro Text / Roboto, Regular, 16pt
- **Captions**: SF Pro Text / Roboto, Regular, 12pt, Secondary color

### Component Specifications

**Airport Cards**:
- White background with subtle border (no shadow)
- 16pt corner radius
- Padding: Spacing.lg
- Touchable with opacity feedback (0.6 when pressed)

**Floating Action Button (View 3D Map)**:
- Background: Primary color
- Size: 56x56pt
- Icon: 3D cube or map icon in white
- Position: 16pt from right edge, 16pt from bottom + tabBarHeight
- Shadow: shadowOffset {width: 0, height: 2}, shadowOpacity: 0.10, shadowRadius: 2

**3D Location Markers**:
- Circular icons with category color background
- White icon from Feather icon set
- Scale animation on tap
- Info card slides up from bottom on selection

**Search Bar**:
- Light gray background (#F5F5F5 in light mode)
- 12pt corner radius
- Magnifying glass icon (left)
- Clear button when text entered (right)

### Interaction Design
- All list items have press opacity (0.7)
- 3D viewport gestures:
  - Single finger drag: pan
  - Pinch: zoom
  - Two finger rotate: rotate view
- Pull-to-refresh on scrollable lists
- Smooth transitions between screens (default push/modal animations)
- Navigation path in 3D animates drawing from start to destination

### Accessibility Requirements
- Minimum touch target: 44x44pt for all interactive elements
- Sufficient color contrast ratios (WCAG AA compliant)
- Support for Dynamic Type scaling
- VoiceOver labels for all interactive elements
- Alternative text for 3D markers describing location type and name

### Critical Assets
1. **Airport Icons** (20-30): Generate simple, recognizable landmarks or terminal layouts for major airports (LAX, JFK, LHR, DXB, CDG, NRT, SIN, etc.) as top-down simplified 3D models or iconic representations
2. **Location Type Icons**: Gate, Restroom, Restaurant, Coffee, Shopping, ATM, Information, Security Checkpoint, Customs, Baggage Claim, Ground Transportation (use Feather icons)
3. **Avatar Presets** (3): Aviation-themed flat design avatars - pilot with cap, business traveler with briefcase, casual traveler with backpack