# UR20 Swarm Control Dashboard - Architecture Guide

## System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    User Interface Layer                      │
│  ┌──────────────┬──────────────┬──────────────────────────┐ │
│  │   Sidebar    │ Main Content │   Debug Console          │ │
│  │  Navigation  │   (Sections) │   (Real-time Logs)       │ │
│  └──────────────┴──────────────┴──────────────────────────┘ │
│  ┌───────────────────────────────────────────────────────┐  │
│  │       Control Bar (Start/Stop/E-Stop/Restart)        │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────┐
│                  State Management Layer                      │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  RoboticsDashboard Class (Main State Container)     │   │
│  │  - robots[]      (3 UR20 robots)                    │   │
│  │  - systemMetrics (KPIs & health)                    │   │
│  │  - detectedObjects[] (AI vision data)               │   │
│  │  - logs[] (activity & debug)                        │   │
│  │  - settings (theme, frequency, etc)                 │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────┐
│                  Business Logic Layer                        │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Controllers & Event Handlers                       │   │
│  │  - RobotController (fleet management)               │   │
│  │  - ConsoleManager (logging & debugging)             │   │
│  │  - DataSimulator (real-time mock data)              │   │
│  │  - ThemeManager (dark/light switching)              │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────┐
│              Data & Simulation Layer                         │
│  In-Memory Data Store (No external API yet)                 │
│  - Robot telemetry simulation                              │
│  - AI vision data simulation                               │
│  - System metrics generation                               │
└─────────────────────────────────────────────────────────────┘
```

## Component Architecture

### Core Classes

#### 1. **RoboticsDashboard** (Main Application)
Central state container and orchestrator.

```javascript
class RoboticsDashboard {
    constructor() {
        this.robots = [];              // Fleet data
        this.systemMetrics = {};       // KPIs
        this.detectedObjects = [];     // AI vision
        this.logs = [];                // Activity log
        this.settings = {};            // User preferences
        this.currentSection = 'dashboard';
    }

    initialize()                // Load initial data
    updateMetrics()            // Refresh KPIs
    addLog(message, type)      // Logging
    switchSection(name)        // Navigation
    toggleTheme()              // Dark/light
}
```

#### 2. **RobotController** (Fleet Management)
Manages individual robot states and actions.

```javascript
class RobotController {
    constructor(dashboard)
    getRobots()                // Fetch all robots
    getRobotById(id)           // Get specific robot
    updateRobotStatus()        // Update state
    startAllRobots()           // Execute start
    stopAllRobots()            // Execute stop
    emergencyStop()            // E-Stop action
}
```

#### 3. **ConsoleManager** (Logging System)
Manages debug logging per section.

```javascript
class ConsoleManager {
    constructor(sectionName)
    log(message, type)         // Add log entry
    clearLogs()                // Clear all
    filterLogs(text)           // Search logs
    exportLogs()               // Download logs
    render()                   // Update DOM
}
```

#### 4. **DataSimulator** (Mock Data Generator)
Generates real-time simulation data.

```javascript
class DataSimulator {
    constructor(dashboard)
    simulateRobotActivity()    // Robot state changes
    simulateVisionUpdates()    // AI detection updates
    simulateMetrics()          // KPI changes
    start()                    // Begin simulation
    stop()                     // Stop simulation
}
```

#### 5. **ThemeManager** (Design System)
Handles theme switching and persistence.

```javascript
class ThemeManager {
    constructor()
    toggleTheme()              // Switch light/dark
    applyTheme(theme)          // Apply CSS vars
    getTheme()                 // Get current theme
    savePreference()           // Persist choice
}
```

## Data Flow

```
User Action (click button)
    ↓
Event Listener (click handler)
    ↓
Controller Method (RobotController.startAllRobots())
    ↓
State Update (dashboard.robots[].status = 'ACTIVE')
    ↓
Log Entry (ConsoleManager.log('Robot started'))
    ↓
DOM Re-render (updateUI())
    ↓
Console Update (renderConsole())
    ↓
Visual Feedback (Animation plays)
```

## Section Architecture

Each section follows the same pattern:

```javascript
// Initialize section
const initDashboard = () => {
    renderDashboardContent();  // Render HTML
    attachEventListeners();     // Bind handlers
    updateConsole();            // Show logs
    startDataRefresh();         // Begin updates
}

// Handle user interaction
document.addEventListener('click', (e) => {
    if (e.target.matches('[data-action="start"]')) {
        dashboard.controllers.robot.startAllRobots();
        updateUI();
    }
});

// Data update cycle
setInterval(() => {
    dashboard.updateMetrics();  // Refresh data
    updateDashboard();          // Update DOM
    renderConsole();            // Update logs
}, updateFrequency);
```

## State Management Pattern

### Initial State
```javascript
const initialState = {
    robots: [
        { id: 'UR20-001', status: 'ACTIVE', ... },
        { id: 'UR20-002', status: 'IDLE', ... },
        { id: 'UR20-003', status: 'ERROR', ... }
    ],
    systemMetrics: {
        activeRobots: '3/5',
        throughput: 342,
        latency: 45,
        errorRate: 0.8
    },
    currentUser: 'operator', // or 'developer' or 'business'
    theme: 'light',
    logs: []
}
```

### State Updates
```javascript
// Action: Start robot
dashboard.robots[0].status = 'ACTIVE';
dashboard.addLog('Robot started', 'info');

// Action: Update metrics
dashboard.updateMetrics();

// Trigger re-render
updateUI();
```

## Event Flow

```
┌─────────────────────────────────────┐
│  User Interaction (Button Click)    │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  Event Handler (Bound to Element)   │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  Validation (Check preconditions)   │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  State Update (Change data)         │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  Logging (Add to console)           │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  DOM Update (Render new state)      │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  Console Render (Show logs)         │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│  Animation Feedback (Visual effect) │
└─────────────────────────────────────┘
```

## Performance Considerations

### Rendering Optimization
- Update only changed DOM elements
- Use requestAnimationFrame for animations
- Debounce console updates (100ms throttle)
- Limit log entries to 30 per section

### Memory Management
- Clear old logs automatically
- Reuse element references
- Avoid memory leaks in event listeners
- Use WeakMap for cache if needed

### Animation Performance
- GPU-accelerated transitions (transform, opacity)
- 60 FPS target for smooth scrolling
- Use will-change sparingly
- Minimize repaints with batched updates

## Future API Integration

### Real-time Updates (WebSocket)
```javascript
const ws = new WebSocket('wss://api.robotics.io/swarm');

ws.onmessage = (event) => {
    const update = JSON.parse(event.data);
    dashboard.updateRobotMetrics(update);
    updateUI();
};
```

### REST Endpoints
```
GET  /api/robots              - Get all robots
POST /api/robots/{id}/start   - Start robot
POST /api/robots/{id}/stop    - Stop robot
POST /api/robots/{id}/estop   - Emergency stop
GET  /api/metrics             - Get system metrics
GET  /api/logs                - Get activity logs
POST /api/tasks               - Create task
```

## Code Organization

```
index.html
├── <head>
│   ├── Meta tags
│   ├── <style> (All CSS)
│   │   ├── Design System Variables
│   │   ├── Reset/Normalize
│   │   ├── Typography
│   │   ├── Components
│   │   ├── Layout
│   │   └── Responsive
│   └── Title
├── <body>
│   ├── Navigation Sidebar
│   ├── Main Content Area
│   ├── Control Bar
│   ├── Debug Console
│   └── <script> (All JS)
│       ├── Classes
│       ├── Managers
│       ├── Controllers
│       ├── Event Listeners
│       ├── Initialization
│       └── Data Simulator
```

## Testing Strategy

### Unit Testing (Future)
- Controller methods
- State management
- Data transformations
- Utility functions

### Integration Testing (Future)
- Section switching
- Data flow
- Console logging
- Theme persistence

### E2E Testing (Future)
- Full user workflows
- Multi-user scenarios
- Performance under load
- Browser compatibility

---

**Last Updated**: 2025-11-27  
**Version**: 3.0.0
