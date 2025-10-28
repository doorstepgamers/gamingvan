# Solar Monitoring Dashboard

## Overview
A real-time monitoring dashboard for Victron Smart Solar MPPT 75/15 charge controllers. The application reads data from the charge controller via VE.Direct serial protocol, stores it in memory, and displays live metrics through a WebSocket-connected React dashboard. It includes a web-based setup wizard for easy configuration without CLI, cross-platform compatibility, and an emulation mode for testing. The project aims to provide a user-friendly, real-time visualization of solar power generation and battery status.

## User Preferences
Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React with TypeScript and Vite.
- **UI Component System**: shadcn/ui (Radix UI primitives) with "new-york" style, using Tailwind CSS for styling and supporting light/dark mode.
- **State Management**: React Query for server state, local React state for WebSocket data, and a custom `useSolarData` hook.
- **Routing**: Wouter for client-side routing (`/` for dashboard, `/setup` for configuration).
- **Real-time Updates**: WebSocket client connection to `/ws`.
- **Design System**: Typography using Inter and JetBrains Mono, neutral-based color scheme with solar themes, responsive grid layouts, and Material Design elevation.
- **Chart Library**: Recharts for time-series data visualization.

### Backend Architecture
- **Server Framework**: Express.js with TypeScript.
- **Architecture Pattern**: Monolithic server with modular route registration.
- **Real-time Communication**: WebSocket server (`ws` library) on `/ws` for broadcasting real-time and historical data, supporting multiple clients and automatic reconnection.
- **Data Acquisition**: A Python subprocess (`vedirect_reader.py`) reads VE.Direct serial data, parsing Victron MPPT charge controller information. It supports cross-platform serial ports (Windows COM, Linux `/dev/ttyUSB*`) and includes an emulation mode. The reader restarts automatically on crash or configuration changes.
- **Data Flow**: Python process streams JSON data, Node.js captures and parses it, aggregates data to 1-minute samples for history, and broadcasts real-time data every second and history updates every minute via WebSockets.
- **Configuration System**: JSON file persistence (`config.json`) for serial port settings, managed by a `ConfigManager` class. API endpoints for retrieving, saving configuration, listing, and testing serial ports.
- **Setup Wizard**: Web-based GUI (`/setup`) for first-time configuration, including port selection, connection testing, and real-time error feedback, powered by React Query.

### Data Storage
- **Current Implementation**: In-memory storage (`MemStorage` class) for a 24-hour rolling history with per-minute aggregation. Stores latest solar data snapshot and up to 1440 historical points, ensuring memory efficiency.
- **Schema Definition**: Shared TypeScript types (`shared/schema.ts`) define `SolarData` (real-time metrics) and `ChartDataPoint` (historical data).
- **Future Database Strategy**: Infrastructure is prepared for PostgreSQL via Drizzle ORM, with connection configured via Neon serverless driver and migration setup ready for persistent storage.

## External Dependencies

- **Hardware Integration**:
    - Victron Smart Solar MPPT 75/15 charge controller
    - VE.Direct USB interface cable
    - Serial port communication (e.g., `/dev/ttyUSB0`)
- **Python Libraries**:
    - `pyserial`: For serial port communication and enumeration.
- **Third-party Services**:
    - Google Fonts CDN: For Inter and JetBrains Mono font families.
- **Build Tools**:
    - Vite: Frontend build system.
    - esbuild: Server-side bundling.
    - Drizzle Kit: Database schema management (prepared).
    - TypeScript compiler.
- **UI Libraries**:
    - Radix UI: Headless accessible component primitives.
    - Recharts: Chart rendering.
    - Lucide React: Icon library.
    - `class-variance-authority`: Component variant management.
    - Tailwind CSS: Utility-first styling.
- **Real-time Communication**:
    - `ws`: WebSocket library for Node.js server.
- **Validation & Forms**:
    - Zod: Schema validation.