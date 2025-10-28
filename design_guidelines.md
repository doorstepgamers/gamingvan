# Solar Monitoring Dashboard Design Guidelines

## Design Approach

**Selected Approach:** Design System - Material Design adapted for data visualization
**Rationale:** This is a utility-focused monitoring dashboard requiring clear data hierarchy, real-time updates, and information density. Material Design's component system and elevation patterns work well for organizing complex data displays.

**Key Design Principles:**
- Information clarity over decoration
- Scannable data hierarchy
- Real-time status visibility
- Responsive data visualization
- Clean, technical aesthetic suitable for monitoring tools

---

## Typography System

**Font Family:** 
- Primary: Inter or Roboto (via Google Fonts CDN)
- Monospace: JetBrains Mono for numerical data displays

**Hierarchy:**
- Page Title: text-2xl md:text-3xl font-semibold
- Section Headers: text-lg md:text-xl font-semibold
- Metric Labels: text-sm font-medium uppercase tracking-wide
- Metric Values: text-3xl md:text-4xl font-bold (monospace for numbers)
- Secondary Values: text-xl font-semibold
- Body Text: text-base
- Timestamps/Status: text-xs md:text-sm

---

## Layout System

**Spacing Primitives:** Tailwind units of 2, 4, 6, and 8
- Component padding: p-4 md:p-6
- Section gaps: gap-4 md:gap-6
- Grid spacing: space-y-6 md:space-y-8
- Card padding: p-6

**Grid Structure:**
- Main container: max-w-7xl mx-auto px-4
- Dashboard grid: grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6
- Metrics row: grid grid-cols-2 md:grid-cols-4 gap-4

**Viewport Management:**
- Header: Fixed height (h-16)
- Main content: Natural flow, no forced viewport heights
- Scrollable content areas for data tables/logs

---

## Component Library

### Header/Navigation
- Fixed top bar with system title "Solar Monitor"
- Connection status indicator (live/offline)
- Last updated timestamp
- Icon library: Heroicons via CDN

### Primary Metrics Cards (4 key stats)
- Battery Level (%)
- Solar Power (W)
- Battery Voltage (V)
- Charging Status

Each card structure:
- Icon at top-left (sun, battery, bolt symbols)
- Large value display (4xl monospace)
- Small label below
- Rounded borders (rounded-lg)
- Subtle elevation (shadow-md)

### Secondary Data Grid
- Panel Voltage
- Panel Current
- Battery Current
- Daily Yield (kWh)
- Total Yield

Grid layout: 2 columns mobile, 3-4 columns desktop
Each with icon, value, and label

### Real-time Chart Section
- Full-width chart container
- Power production over time (last 24h)
- Chart library: Chart.js via CDN
- Responsive height: h-64 md:h-80
- Grid lines for readability

### Historical Stats Table
- Compact data table
- Columns: Date, Max Power, Energy Produced, Avg Voltage
- Zebra striping for rows
- Sticky header on scroll
- Responsive: Stack on mobile

### Status Timeline/Log
- List of recent events
- Timestamp + status message
- Icons for event types
- Max height with scroll (max-h-96 overflow-y-auto)

### System Information Panel
- Device details (MPPT model)
- Firmware version
- Connection type
- Uptime display

---

## Component Patterns

**Card Pattern:**
```
- Rounded corners (rounded-lg)
- Padding: p-6
- Shadow: shadow-md
- Border: border
- Spacing between elements: space-y-2
```

**Data Display Pattern:**
- Label above value
- Icon inline with label
- Value in large monospace font
- Unit displayed next to value in smaller font

**Status Indicators:**
- Use icon + text combination
- Heroicons: CheckCircleIcon, ExclamationTriangleIcon, XCircleIcon
- Positioned consistently (top-right of cards or inline)

---

## Responsive Behavior

**Mobile (base):**
- Single column layout
- Stacked metrics cards
- Simplified chart with touch interactions
- Collapsible sections for historical data

**Tablet (md:):**
- 2-column grid for metrics
- Side-by-side primary stats
- Full-width chart maintained

**Desktop (lg:):**
- 3-column grid layout
- Chart and table side-by-side where appropriate
- Maximum information density without clutter

---

## Data Visualization Principles

- Use Chart.js for line charts (power over time)
- Area charts for production visualization
- Consistent axis labeling
- Gridlines for easier reading
- Tooltips on hover for precise values
- Responsive canvas sizing

---

## Animations

**Minimal Animation Strategy:**
- Live data updates: Smooth number transitions (transition-all duration-300)
- Card hover: Subtle lift (hover:shadow-lg transition-shadow)
- Connection status: Pulse animation for "connecting" state
- No unnecessary motion

---

## Accessibility

- High contrast for numerical data
- Clear labels for all metrics
- ARIA labels for status indicators
- Keyboard navigation for interactive elements
- Screen reader friendly data tables
- Focus states on all interactive elements

---

## Asset Guidelines

**Icons:** Heroicons via CDN (solar, battery, bolt, chart, clock icons)
**Charts:** Chart.js library via CDN
**Fonts:** Google Fonts (Inter + JetBrains Mono)

No custom image assets needed - this is a data-focused dashboard