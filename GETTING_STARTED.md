# Getting Started with Routine.md

## Table of Contents

1. [Quick Start](#quick-start)
2. [Installation](#installation)
3. [Basic Setup](#basic-setup)
4. [Your First Routine](#your-first-routine)
5. [Working with Tasks](#working-with-tasks)
6. [Scheduling & Automation](#scheduling--automation)
7. [Analytics & Tracking](#analytics--tracking)
8. [Advanced Features](#advanced-features)
9. [Best Practices](#best-practices)
10. [Troubleshooting](#troubleshooting)

## Quick Start

Get up and running with Routine.md in 5 minutes:

### 1. Install the Package

```bash
npm install routine-md
# or
yarn add routine-md
```

### 2. Basic Setup

```jsx
import React from 'react';
import { RoutineProvider, RoutineCard, TaskList } from 'routine-md';
import 'routine-md/dist/styles.css';

function App() {
  return (
    <RoutineProvider apiKey="your-api-key">
      <div className="app">
        <h1>My Routines</h1>
        {/* Your components here */}
      </div>
    </RoutineProvider>
  );
}

export default App;
```

### 3. Create Your First Routine

```jsx
import { useState } from 'react';
import { RoutineForm, useRoutines } from 'routine-md';

function CreateRoutine() {
  const { createRoutine } = useRoutines();
  const [showForm, setShowForm] = useState(false);

  const handleSubmit = async (routineData) => {
    await createRoutine(routineData);
    setShowForm(false);
  };

  return (
    <div>
      <button onClick={() => setShowForm(true)}>
        Create New Routine
      </button>
      
      {showForm && (
        <RoutineForm
          onSubmit={handleSubmit}
          onCancel={() => setShowForm(false)}
        />
      )}
    </div>
  );
}
```

## Installation

### Package Manager Installation

**npm:**
```bash
npm install routine-md
```

**yarn:**
```bash
yarn add routine-md
```

**pnpm:**
```bash
pnpm add routine-md
```

### CDN Installation

For quick prototyping, you can use the CDN version:

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="https://unpkg.com/routine-md@latest/dist/styles.css">
</head>
<body>
  <div id="root"></div>
  <script src="https://unpkg.com/routine-md@latest/dist/routine-md.umd.js"></script>
</body>
</html>
```

### Peer Dependencies

Routine.md requires the following peer dependencies:

```json
{
  "react": ">=16.8.0",
  "react-dom": ">=16.8.0"
}
```

Install them if not already present:

```bash
npm install react react-dom
```

## Basic Setup

### 1. Provider Setup

Wrap your application with the `RoutineProvider` to enable global state management:

```jsx
import React from 'react';
import { RoutineProvider } from 'routine-md';
import 'routine-md/dist/styles.css';

function App() {
  return (
    <RoutineProvider 
      apiKey="your-api-key"
      baseURL="https://api.routine.md/v1"
      options={{
        autoSync: true,
        offline: true,
        theme: 'light'
      }}
    >
      <MainApp />
    </RoutineProvider>
  );
}

function MainApp() {
  // Your app components
  return <div>Your app content</div>;
}

export default App;
```

### 2. API Key Configuration

Get your API key from the Routine.md dashboard and configure it:

**Environment Variable (Recommended):**
```bash
# .env
REACT_APP_ROUTINE_API_KEY=your-api-key-here
```

```jsx
<RoutineProvider apiKey={process.env.REACT_APP_ROUTINE_API_KEY}>
  {/* Your app */}
</RoutineProvider>
```

**Direct Configuration:**
```jsx
<RoutineProvider apiKey="sk-1234567890abcdef">
  {/* Your app */}
</RoutineProvider>
```

### 3. TypeScript Setup (Optional)

If using TypeScript, add type definitions:

```bash
npm install @types/routine-md
```

Create a `types.d.ts` file:

```typescript
declare module 'routine-md' {
  export interface Routine {
    id: string;
    name: string;
    description?: string;
    tasks: Task[];
    schedule: Schedule;
    // ... other properties
  }
  
  // ... other type definitions
}
```

## Your First Routine

Let's create a complete example of building a morning routine:

### Step 1: Create the Routine Structure

```jsx
import { useState } from 'react';
import { useRoutines } from 'routine-md';

function MorningRoutineExample() {
  const { createRoutine, routines, loading } = useRoutines();
  const [creating, setCreating] = useState(false);

  const createMorningRoutine = async () => {
    setCreating(true);
    
    try {
      const morningRoutine = await createRoutine({
        name: "Morning Routine",
        description: "Start the day with energy and focus",
        schedule: {
          type: "daily",
          time: "07:00",
          timezone: "America/New_York"
        },
        tasks: [
          {
            name: "Meditation",
            description: "10 minutes of mindfulness",
            duration: 10,
            order: 1
          },
          {
            name: "Exercise",
            description: "Quick workout or stretch",
            duration: 20,
            order: 2
          },
          {
            name: "Healthy Breakfast",
            description: "Nutritious meal to fuel the day",
            duration: 15,
            order: 3
          },
          {
            name: "Review Daily Goals",
            description: "Plan the day ahead",
            duration: 5,
            order: 4
          }
        ],
        tags: ["morning", "health", "productivity"],
        active: true
      });
      
      console.log('Created routine:', morningRoutine);
    } catch (error) {
      console.error('Failed to create routine:', error);
    } finally {
      setCreating(false);
    }
  };

  if (loading) {
    return <div>Loading routines...</div>;
  }

  return (
    <div>
      <h2>Morning Routine Setup</h2>
      <button 
        onClick={createMorningRoutine}
        disabled={creating}
      >
        {creating ? 'Creating...' : 'Create Morning Routine'}
      </button>
      
      {routines.length > 0 && (
        <div>
          <h3>Your Routines:</h3>
          {routines.map(routine => (
            <div key={routine.id}>
              <h4>{routine.name}</h4>
              <p>{routine.description}</p>
              <p>Tasks: {routine.tasks.length}</p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

### Step 2: Display with RoutineCard

```jsx
import { RoutineCard, RoutineGrid } from 'routine-md';

function RoutineDisplay() {
  const { routines } = useRoutines();
  
  const handleEdit = (routine) => {
    console.log('Editing routine:', routine.name);
  };
  
  const handleDelete = (routine) => {
    if (confirm(`Delete "${routine.name}"?`)) {
      // Delete logic here
    }
  };

  return (
    <div>
      <h2>My Routines</h2>
      <RoutineGrid
        routines={routines}
        columns={2}
        gap={20}
        renderRoutine={(routine) => (
          <RoutineCard
            key={routine.id}
            routine={routine}
            onEdit={handleEdit}
            onDelete={handleDelete}
            showStats={true}
          />
        )}
      />
    </div>
  );
}
```

## Working with Tasks

### Managing Tasks within Routines

```jsx
import { TaskList, TaskForm } from 'routine-md';

function RoutineTaskManager({ routine }) {
  const [editingTask, setEditingTask] = useState(null);
  const [showAddTask, setShowAddTask] = useState(false);

  const handleTaskComplete = async (taskId) => {
    try {
      await completeTask(taskId, {
        completed_at: new Date().toISOString(),
        quality_rating: 5
      });
    } catch (error) {
      console.error('Failed to complete task:', error);
    }
  };

  const handleTaskEdit = (task) => {
    setEditingTask(task);
  };

  const handleTaskSubmit = async (taskData) => {
    if (editingTask) {
      // Update existing task
      await updateTask(editingTask.id, taskData);
    } else {
      // Create new task
      await createTask(routine.id, taskData);
    }
    
    setEditingTask(null);
    setShowAddTask(false);
  };

  return (
    <div>
      <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
        <h3>{routine.name} Tasks</h3>
        <button onClick={() => setShowAddTask(true)}>
          Add Task
        </button>
      </div>

      <TaskList
        tasks={routine.tasks}
        onTaskComplete={handleTaskComplete}
        onTaskEdit={handleTaskEdit}
        editable={true}
        sortable={true}
        showProgress={true}
      />

      {(editingTask || showAddTask) && (
        <TaskForm
          task={editingTask}
          onSubmit={handleTaskSubmit}
          onCancel={() => {
            setEditingTask(null);
            setShowAddTask(false);
          }}
        />
      )}
    </div>
  );
}
```

### Custom Task Components

```jsx
import { TaskItem } from 'routine-md';

function CustomTaskItem({ task, onComplete, onEdit }) {
  return (
    <div className="custom-task-item">
      <div className="task-header">
        <h4>{task.name}</h4>
        <span className="duration">{task.duration}m</span>
      </div>
      
      <p className="task-description">{task.description}</p>
      
      <div className="task-actions">
        <button 
          onClick={() => onComplete(task.id)}
          className={`complete-btn ${task.completed ? 'completed' : ''}`}
        >
          {task.completed ? '✓ Completed' : 'Complete'}
        </button>
        
        <button onClick={() => onEdit(task)}>
          Edit
        </button>
      </div>
    </div>
  );
}

// Use in TaskList
<TaskList
  tasks={tasks}
  onTaskComplete={handleComplete}
  renderTask={(task, handlers) => (
    <CustomTaskItem
      task={task}
      onComplete={handlers.onComplete}
      onEdit={handlers.onEdit}
    />
  )}
/>
```

## Scheduling & Automation

### Setting Up Different Schedule Types

```jsx
import { SchedulePicker } from 'routine-md';

function ScheduleExamples() {
  const [dailySchedule, setDailySchedule] = useState({
    type: 'daily',
    time: '07:00',
    timezone: 'America/New_York'
  });

  const [weeklySchedule, setWeeklySchedule] = useState({
    type: 'weekly',
    time: '09:00',
    weekdays: [1, 3, 5], // Monday, Wednesday, Friday
    timezone: 'America/New_York'
  });

  const [customSchedule, setCustomSchedule] = useState({
    type: 'custom',
    time: '18:00',
    interval: 2, // Every 2 days
    timezone: 'America/New_York'
  });

  return (
    <div>
      <h3>Schedule Configuration Examples</h3>
      
      <div className="schedule-example">
        <h4>Daily Routine (Every day at 7:00 AM)</h4>
        <SchedulePicker
          schedule={dailySchedule}
          onChange={setDailySchedule}
        />
      </div>

      <div className="schedule-example">
        <h4>Weekly Routine (Mon, Wed, Fri at 9:00 AM)</h4>
        <SchedulePicker
          schedule={weeklySchedule}
          onChange={setWeeklySchedule}
        />
      </div>

      <div className="schedule-example">
        <h4>Custom Routine (Every 2 days at 6:00 PM)</h4>
        <SchedulePicker
          schedule={customSchedule}
          onChange={setCustomSchedule}
        />
      </div>
    </div>
  );
}
```

### Timezone Handling

```jsx
import { convertTimezone, getTimezoneOffset } from 'routine-md/utils';

function TimezoneExample() {
  const userTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
  
  const scheduleRoutine = async (routineData) => {
    // Convert user's local time to UTC for storage
    const utcTime = convertTimezone(
      new Date(`2024-01-01T${routineData.schedule.time}:00`),
      userTimezone,
      'UTC'
    );
    
    const scheduleWithUTC = {
      ...routineData.schedule,
      time: utcTime.toISOString().substr(11, 5), // Extract HH:MM
      timezone: userTimezone
    };
    
    return await createRoutine({
      ...routineData,
      schedule: scheduleWithUTC
    });
  };
  
  return (
    <div>
      <p>Your timezone: {userTimezone}</p>
      {/* Schedule picker component */}
    </div>
  );
}
```

## Analytics & Tracking

### Basic Progress Tracking

```jsx
import { useRoutineStats, ProgressRing } from 'routine-md';
import { formatProgress, calculateStreak } from 'routine-md/utils';

function RoutineAnalytics({ routine }) {
  const { stats, loading } = useRoutineStats(routine.id, {
    period: 'month'
  });

  if (loading) {
    return <div>Loading analytics...</div>;
  }

  return (
    <div className="routine-analytics">
      <h3>{routine.name} Analytics</h3>
      
      <div className="stats-grid">
        <div className="stat-card">
          <h4>Completion Rate</h4>
          <ProgressRing 
            progress={stats.completionRate} 
            showText={true}
            size={60}
          />
        </div>
        
        <div className="stat-card">
          <h4>Current Streak</h4>
          <div className="streak-display">
            {stats.currentStreak} days
          </div>
        </div>
        
        <div className="stat-card">
          <h4>Best Streak</h4>
          <div className="streak-display">
            {stats.bestStreak} days
          </div>
        </div>
        
        <div className="stat-card">
          <h4>Total Completions</h4>
          <div className="completion-count">
            {formatProgress(stats.completed, stats.total, { format: 'fraction' })}
          </div>
        </div>
      </div>
    </div>
  );
}
```

### Advanced Analytics Dashboard

```jsx
import { LineChart, BarChart } from 'routine-md/charts';
import { generateProgressReport } from 'routine-md/utils';

function AnalyticsDashboard() {
  const { routines } = useRoutines();
  const [dateRange, setDateRange] = useState({
    startDate: '2024-01-01',
    endDate: '2024-01-31'
  });
  
  const report = useMemo(() => 
    generateProgressReport(routines, dateRange),
    [routines, dateRange]
  );

  return (
    <div className="analytics-dashboard">
      <h2>Progress Dashboard</h2>
      
      <div className="date-range-picker">
        <input 
          type="date" 
          value={dateRange.startDate}
          onChange={(e) => setDateRange(prev => ({
            ...prev,
            startDate: e.target.value
          }))}
        />
        <input 
          type="date" 
          value={dateRange.endDate}
          onChange={(e) => setDateRange(prev => ({
            ...prev,
            endDate: e.target.value
          }))}
        />
      </div>

      <div className="overview-stats">
        <div className="overview-card">
          <h3>Overall Completion Rate</h3>
          <div className="big-number">
            {report.overallCompletionRate.toFixed(1)}%
          </div>
        </div>
        
        <div className="overview-card">
          <h3>Active Routines</h3>
          <div className="big-number">
            {report.activeRoutines}
          </div>
        </div>
        
        <div className="overview-card">
          <h3>Total Tasks Completed</h3>
          <div className="big-number">
            {report.totalTasksCompleted}
          </div>
        </div>
      </div>

      <div className="charts-section">
        <div className="chart-container">
          <h3>Daily Completion Trend</h3>
          <LineChart 
            data={report.dailyTrend}
            xAxis="date"
            yAxis="completionRate"
          />
        </div>
        
        <div className="chart-container">
          <h3>Routine Performance</h3>
          <BarChart 
            data={report.routinePerformance}
            xAxis="routineName"
            yAxis="completionRate"
          />
        </div>
      </div>
    </div>
  );
}
```

## Advanced Features

### Custom Hooks for Complex Logic

```jsx
import { useState, useEffect } from 'react';
import { useRoutines } from 'routine-md';

// Custom hook for routine filtering and searching
function useRoutineSearch() {
  const { routines } = useRoutines();
  const [searchQuery, setSearchQuery] = useState('');
  const [filters, setFilters] = useState({
    active: null,
    tags: [],
    completionRate: null
  });
  
  const filteredRoutines = useMemo(() => {
    let result = routines;
    
    // Search by name/description
    if (searchQuery) {
      result = result.filter(routine => 
        routine.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
        routine.description?.toLowerCase().includes(searchQuery.toLowerCase())
      );
    }
    
    // Filter by active status
    if (filters.active !== null) {
      result = result.filter(routine => routine.active === filters.active);
    }
    
    // Filter by tags
    if (filters.tags.length > 0) {
      result = result.filter(routine =>
        filters.tags.some(tag => routine.tags.includes(tag))
      );
    }
    
    return result;
  }, [routines, searchQuery, filters]);
  
  return {
    routines: filteredRoutines,
    searchQuery,
    setSearchQuery,
    filters,
    setFilters
  };
}

// Usage
function RoutineSearchExample() {
  const {
    routines,
    searchQuery,
    setSearchQuery,
    filters,
    setFilters
  } = useRoutineSearch();

  return (
    <div>
      <input
        type="text"
        placeholder="Search routines..."
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
      />
      
      <div className="filters">
        <label>
          <input
            type="checkbox"
            checked={filters.active === true}
            onChange={(e) => setFilters(prev => ({
              ...prev,
              active: e.target.checked ? true : null
            }))}
          />
          Active only
        </label>
      </div>
      
      <RoutineGrid routines={routines} />
    </div>
  );
}
```

### Offline Support

```jsx
import { useOfflineSync } from 'routine-md';

function OfflineExample() {
  const { isOnline, syncStatus, pendingChanges } = useOfflineSync();
  
  return (
    <div className="offline-status">
      <div className={`status-indicator ${isOnline ? 'online' : 'offline'}`}>
        {isOnline ? 'Online' : 'Offline'}
      </div>
      
      {syncStatus === 'syncing' && (
        <div className="sync-indicator">Syncing...</div>
      )}
      
      {pendingChanges > 0 && (
        <div className="pending-changes">
          {pendingChanges} changes pending sync
        </div>
      )}
    </div>
  );
}
```

## Best Practices

### 1. Component Organization

```
src/
├── components/
│   ├── routines/
│   │   ├── RoutineCard.jsx
│   │   ├── RoutineForm.jsx
│   │   └── RoutineList.jsx
│   ├── tasks/
│   │   ├── TaskItem.jsx
│   │   └── TaskForm.jsx
│   └── analytics/
│       └── ProgressChart.jsx
├── hooks/
│   ├── useRoutineSearch.js
│   └── useTaskCompletion.js
├── utils/
│   └── routineHelpers.js
└── App.jsx
```

### 2. State Management

```jsx
// Good: Use provided hooks
function MyComponent() {
  const { routines, createRoutine, updateRoutine } = useRoutines();
  // Component logic
}

// Avoid: Direct API calls without hooks
function MyComponent() {
  const [routines, setRoutines] = useState([]);
  
  useEffect(() => {
    // Manual API calls - not recommended
    fetch('/api/routines').then(/* ... */);
  }, []);
}
```

### 3. Error Handling

```jsx
import { ErrorBoundary } from 'routine-md';

function App() {
  return (
    <ErrorBoundary
      fallback={<div>Something went wrong with your routines</div>}
      onError={(error, errorInfo) => {
        console.error('Routine error:', error, errorInfo);
        // Send to error reporting service
      }}
    >
      <RoutineProvider apiKey={apiKey}>
        <MainApp />
      </RoutineProvider>
    </ErrorBoundary>
  );
}
```

### 4. Performance Optimization

```jsx
import { memo } from 'react';

// Memoize routine cards to prevent unnecessary re-renders
const RoutineCard = memo(({ routine, onEdit, onDelete }) => {
  return (
    <div className="routine-card">
      {/* Card content */}
    </div>
  );
});

// Use callback optimization for event handlers
function RoutineList({ routines }) {
  const handleEdit = useCallback((routine) => {
    // Edit logic
  }, []);
  
  const handleDelete = useCallback((routineId) => {
    // Delete logic
  }, []);
  
  return (
    <div>
      {routines.map(routine => (
        <RoutineCard
          key={routine.id}
          routine={routine}
          onEdit={handleEdit}
          onDelete={handleDelete}
        />
      ))}
    </div>
  );
}
```

## Troubleshooting

### Common Issues and Solutions

#### 1. API Key Not Working

**Problem:** Getting authentication errors

**Solutions:**
```jsx
// Check API key format
const isValidApiKey = (key) => {
  return key && key.startsWith('sk-') && key.length > 20;
};

// Debug API key
console.log('API Key valid:', isValidApiKey(process.env.REACT_APP_ROUTINE_API_KEY));

// Ensure provider is properly configured
<RoutineProvider 
  apiKey={apiKey}
  onError={(error) => console.error('Provider error:', error)}
>
  {/* App */}
</RoutineProvider>
```

#### 2. Components Not Rendering

**Problem:** Components appear empty or don't load

**Solutions:**
```jsx
// Check if provider is wrapping components
function App() {
  return (
    <RoutineProvider apiKey={apiKey}>
      {/* Make sure components are inside provider */}
      <MyRoutineComponent />
    </RoutineProvider>
  );
}

// Add loading states
function MyComponent() {
  const { routines, loading, error } = useRoutines();
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  if (!routines.length) return <div>No routines found</div>;
  
  return <RoutineList routines={routines} />;
}
```

#### 3. Styling Issues

**Problem:** Components don't look right

**Solutions:**
```jsx
// Ensure CSS is imported
import 'routine-md/dist/styles.css';

// Check for CSS conflicts
// Use CSS modules or styled-components if needed
import styles from './MyComponent.module.css';

// Override specific styles
.routine-card {
  /* Your custom styles */
  border: 2px solid #blue !important;
}
```

#### 4. TypeScript Errors

**Problem:** Type errors with components

**Solutions:**
```typescript
// Install type definitions
npm install @types/routine-md

// Define component props properly
interface MyComponentProps {
  routine: Routine;
  onEdit: (routine: Routine) => void;
}

const MyComponent: React.FC<MyComponentProps> = ({ routine, onEdit }) => {
  // Component logic
};
```

### Getting Help

1. **Documentation:** Check the API documentation for detailed parameter information
2. **GitHub Issues:** Report bugs or request features
3. **Community:** Join the Discord server for real-time help
4. **Examples:** Check the examples repository for working code samples

### Debug Mode

Enable debug mode to see detailed logging:

```jsx
<RoutineProvider 
  apiKey={apiKey}
  debug={true} // Enable debug mode
>
  {/* Your app */}
</RoutineProvider>
```

This will log API calls, state changes, and other debugging information to the console.

---

Congratulations! You now have a comprehensive understanding of how to get started with Routine.md. From basic setup to advanced features, you're ready to build powerful routine management applications. Remember to check the API documentation for detailed information about specific functions and components.