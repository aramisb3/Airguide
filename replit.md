# AirGuide - Airport Navigation App

## Overview

AirGuide is a React Native mobile application built with Expo that provides 3D airport navigation and wayfinding. Its core purpose is to allow users to browse airports globally, access detailed information, explore interactive 3D maps, and receive AR-enhanced directions within airport layouts. The app features a tab-based navigation system, including airport search, saved airports, and user profile management, offering a comprehensive solution for stress-free air travel.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### UI/UX Decisions

The application utilizes a tab-based navigation system with four main tabs: Explore, Flights, Saved, and Profile, using `@react-navigation/bottom-tabs` and `@react-navigation/native-stack`. A consistent design system based on elevation levels (0-3) provides depth hierarchy, complemented by a theme system supporting light/dark modes. Reusable "Themed" components automatically adapt to color schemes, and custom card components feature spring animations on press interactions. Safe area handling is implemented throughout for optimal layout. An Auth Gate requires PIN authentication for access.

### Technical Implementations

AirGuide is built with React Native and Expo SDK 54, targeting iOS, Android, and Web platforms, and leverages the React Compiler for performance. State management primarily relies on local component state using React hooks, with no global state management library. Animations are handled by React Native Reanimated 4.1.1, enabling spring physics-based interactions and gesture support via React Native Gesture Handler 2.28. The app uses a two-tier airport data system supporting both featured and basic airports, with a graph-based pathfinding algorithm for navigation. Key features include 3D map visualization with gesture controls, floor selection, location markers, and AR navigation using `expo-camera` and `expo-sensors` (Magnetometer).

### Two-Tier Airport System

The app implements a comprehensive global airport coverage with two tiers:

*   **Featured Airports (25+)**: Full 3D navigation data including floor layouts, locations, amenities, and pathfinding connections. These airports display a green "3D Map" badge on the airport card.
*   **Basic Airports (220+)**: Metadata only (name, IATA code, city, country, region, terminal count). These airports show an informational message about 3D navigation coming soon.

**Type System** (`data/airportTypes.ts`):
*   `BaseAirport`: Common properties shared by all airports
*   `FeaturedAirport`: Extends BaseAirport with `hasNavigation: true`, floors, amenities, and locations
*   `BasicAirport`: Extends BaseAirport with `hasNavigation: false`
*   `Airport`: Union type of FeaturedAirport | BasicAirport
*   `isFeaturedAirport()`: Type guard function for discriminating airport types

**Regional Coverage**: North America, Europe, Asia, Middle East, Oceania, Africa, South America, Caribbean, Central America

### Feature Specifications

*   **Airport Exploration**: Search and filter airports, display detailed airport statistics, amenities, and floor information.
*   **3D Map View**: Interactive 3D visualization of airport layouts with pan, zoom, and rotation.
*   **AR Navigation**: Step-by-step augmented reality directions with real-time distance updates and floor change indicators.
*   **Flight Tracking**: Track flights with simulated status updates, local push notifications, and in-app toast alerts. Supports international flights with transfer requirements and multi-language support (11 languages).
*   **User Profiles**: Avatar selection, display name customization, settings for measurement units, default floor view, and rendering quality.
*   **Authentication**: PIN-based and biometric (Face ID/Touch ID) authentication with secure storage of credentials.
*   **Support Pet**: Optional animated mascots providing contextual encouraging messages.
*   **Subscription System**: Free tier (1 flight) and premium tiers (monthly/yearly) for unlimited flight tracking.

### System Design Choices

The application uses Babel module resolver with `@/` aliasing and TypeScript for type safety. Platform-specific logic handles differences between iOS, Android, and Web. Error handling is managed by an ErrorBoundary component with fallback UI. Data persistence for sensitive information (PIN hash, auth state) uses `expo-secure-store`, while non-sensitive preferences and user profile data use `AsyncStorage`.

**Important: Date Handling**
When loading data from AsyncStorage/SecureStore, Date objects are serialized to strings. The `ensureDate()` helper function in `data/flightData.ts` converts these strings back to Date objects before any date operations. Always use `ensureDate()` when working with dates loaded from storage.

### International Flight Components

*   **InternationalArrivalInfo**: Shows detailed arrival info for international flights with two cards:
    *   International Transfer card (globe icon, estimated procedure time, requirement checkmarks)
    *   Arrival Instructions card (numbered steps with blue badges, tool icon)
*   **TransferInfo**: Full transfer requirements for connecting flights with connection time status
*   **FlightTypeBadge**: Visual indicator for domestic/international flights

## External Dependencies

*   **Expo Ecosystem**: `expo`, `expo-camera`, `expo-sensors`, `expo-location`, `expo-blur`, `expo-haptics`, `expo-image`, `expo-splash-screen`, `expo-constants`.
*   **React Navigation**: `@react-navigation/native`, `@react-navigation/native-stack`, `@react-navigation/bottom-tabs`, `@react-navigation/elements`.
*   **Animation and Gestures**: `react-native-reanimated`, `react-native-gesture-handler`, `react-native-worklets`.
*   **UI Components**: `@expo/vector-icons`, `react-native-safe-area-context`, `react-native-screens`, `react-native-keyboard-controller`.
*   **Build Tools**: `babel-plugin-module-resolver`, `eslint`, `prettier`, `typescript`.