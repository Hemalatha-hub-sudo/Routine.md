# Component Documentation - Routine.md

## Table of Contents

1. [Overview](#overview)
2. [Installation](#installation)
3. [Core Components](#core-components)
4. [Form Components](#form-components)
5. [Layout Components](#layout-components)
6. [Styling & Theming](#styling--theming)
7. [Advanced Usage](#advanced-usage)

## Overview

The Routine.md component library provides a comprehensive set of React components for building routine and task management applications. All components are built with accessibility, performance, and customization in mind.

## Installation

```bash
npm install routine-md
# or
yarn add routine-md
```

### Basic Setup

```jsx
import React from 'react';
import { RoutineProvider } from 'routine-md';
import 'routine-md/dist/styles.css';

function App() {
  return (
    <RoutineProvider apiKey="your-api-key">
      {/* Your app components */}
    </RoutineProvider>
  );
}
```

## Core Components

### RoutineCard

A card component for displaying routine information with actions.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `routine` | `Routine` | ✓ | - | Routine data object |
| `onEdit` | `(routine: Routine) => void` | ✗ | - | Edit callback |
| `onDelete` | `(routine: Routine) => void` | ✗ | - | Delete callback |
| `onToggleActive` | `(routine: Routine) => void` | ✗ | - | Toggle active state callback |
| `showStats` | `boolean` | ✗ | `false` | Show completion statistics |
| `compact` | `boolean` | ✗ | `false` | Compact layout mode |
| `className` | `string` | ✗ | - | Additional CSS classes |

#### Usage Examples

**Basic Usage:**
```jsx
import { RoutineCard } from 'routine-md';

const routine = {
  id: 'routine_123',
  name: 'Morning Routine',
  description: 'Start the day right',
  tasks: [...],
  schedule: { type: 'daily', time: '07:00' },
  tags: ['morning', 'health']
};

<RoutineCard routine={routine} />
```

**With Actions:**
```jsx
<RoutineCard
  routine={routine}
  onEdit={(routine) => setEditingRoutine(routine)}
  onDelete={(routine) => handleDelete(routine)}
  onToggleActive={(routine) => toggleActive(routine)}
  showStats={true}
/>
```

**Compact Mode:**
```jsx
<RoutineCard
  routine={routine}
  compact={true}
  className="my-custom-card"
/>
```

#### Styling

```css
.routine-card {
  /* Base card styles */
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 16px;
}

.routine-card.compact {
  /* Compact mode styles */
  padding: 12px;
}

.routine-card__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.routine-card__actions {
  display: flex;
  gap: 8px;
}
```

### TaskList

A component for displaying and managing a list of tasks.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `tasks` | `Task[]` | ✓ | - | Array of task objects |
| `onTaskComplete` | `(taskId: string) => void` | ✓ | - | Task completion callback |
| `onTaskEdit` | `(task: Task) => void` | ✗ | - | Task edit callback |
| `onTaskDelete` | `(taskId: string) => void` | ✗ | - | Task delete callback |
| `editable` | `boolean` | ✗ | `false` | Enable editing features |
| `sortable` | `boolean` | ✗ | `false` | Enable drag-and-drop sorting |
| `showProgress` | `boolean` | ✗ | `true` | Show completion progress |
| `layout` | `'list' \| 'grid'` | ✗ | `'list'` | Layout mode |

#### Usage Examples

**Basic Task List:**
```jsx
import { TaskList } from 'routine-md';

const tasks = [
  {
    id: 'task_1',
    name: 'Meditation',
    duration: 10,
    order: 1,
    completed: false
  },
  {
    id: 'task_2',
    name: 'Exercise',
    duration: 30,
    order: 2,
    completed: true
  }
];

<TaskList
  tasks={tasks}
  onTaskComplete={(taskId) => handleTaskComplete(taskId)}
/>
```

**Editable with Sorting:**
```jsx
<TaskList
  tasks={tasks}
  onTaskComplete={handleTaskComplete}
  onTaskEdit={handleTaskEdit}
  onTaskDelete={handleTaskDelete}
  editable={true}
  sortable={true}
  showProgress={true}
/>
```

**Grid Layout:**
```jsx
<TaskList
  tasks={tasks}
  onTaskComplete={handleTaskComplete}
  layout="grid"
/>
```

### TaskItem

Individual task component used within TaskList.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `task` | `Task` | ✓ | - | Task data object |
| `onComplete` | `() => void` | ✓ | - | Completion callback |
| `onEdit` | `() => void` | ✗ | - | Edit callback |
| `onDelete` | `() => void` | ✗ | - | Delete callback |
| `editable` | `boolean` | ✗ | `false` | Show edit controls |
| `draggable` | `boolean` | ✗ | `false` | Enable drag handle |

#### Usage Example

```jsx
import { TaskItem } from 'routine-md';

<TaskItem
  task={task}
  onComplete={() => handleComplete(task.id)}
  onEdit={() => handleEdit(task)}
  onDelete={() => handleDelete(task.id)}
  editable={true}
  draggable={true}
/>
```

### ProgressRing

A circular progress indicator component.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `progress` | `number` | ✓ | - | Progress value (0-100) |
| `size` | `number` | ✗ | `40` | Ring diameter in pixels |
| `strokeWidth` | `number` | ✗ | `4` | Ring thickness |
| `color` | `string` | ✗ | `'#3b82f6'` | Progress color |
| `backgroundColor` | `string` | ✗ | `'#e5e7eb'` | Background ring color |
| `showText` | `boolean` | ✗ | `false` | Show percentage text |

#### Usage Examples

```jsx
import { ProgressRing } from 'routine-md';

// Basic progress ring
<ProgressRing progress={75} />

// Customized ring
<ProgressRing
  progress={60}
  size={60}
  strokeWidth={6}
  color="#10b981"
  showText={true}
/>

// Small indicator
<ProgressRing
  progress={90}
  size={24}
  strokeWidth={3}
/>
```

## Form Components

### RoutineForm

A comprehensive form for creating and editing routines.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `routine` | `Routine` | ✗ | - | Existing routine for editing |
| `onSubmit` | `(routine: RoutineData) => void` | ✓ | - | Form submission callback |
| `onCancel` | `() => void` | ✗ | - | Cancel callback |
| `loading` | `boolean` | ✗ | `false` | Loading state |
| `errors` | `Record<string, string>` | ✗ | `{}` | Validation errors |

#### Usage Example

```jsx
import { RoutineForm } from 'routine-md';

const [loading, setLoading] = useState(false);
const [errors, setErrors] = useState({});

const handleSubmit = async (routineData) => {
  setLoading(true);
  try {
    await createRoutine(routineData);
    onSuccess();
  } catch (error) {
    setErrors(error.validationErrors || {});
  } finally {
    setLoading(false);
  }
};

<RoutineForm
  onSubmit={handleSubmit}
  onCancel={() => setShowForm(false)}
  loading={loading}
  errors={errors}
/>
```

### TaskForm

Form component for creating and editing individual tasks.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `task` | `Task` | ✗ | - | Existing task for editing |
| `onSubmit` | `(task: TaskData) => void` | ✓ | - | Form submission callback |
| `onCancel` | `() => void` | ✗ | - | Cancel callback |
| `loading` | `boolean` | ✗ | `false` | Loading state |

#### Usage Example

```jsx
import { TaskForm } from 'routine-md';

<TaskForm
  task={editingTask}
  onSubmit={handleTaskSubmit}
  onCancel={() => setEditingTask(null)}
  loading={submitting}
/>
```

### SchedulePicker

Component for selecting routine schedules with various recurrence options.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `schedule` | `Schedule` | ✓ | - | Current schedule configuration |
| `onChange` | `(schedule: Schedule) => void` | ✓ | - | Schedule change callback |
| `disabled` | `boolean` | ✗ | `false` | Disabled state |
| `showTimezone` | `boolean` | ✗ | `true` | Show timezone selector |

#### Usage Examples

**Basic Schedule Picker:**
```jsx
import { SchedulePicker } from 'routine-md';

const [schedule, setSchedule] = useState({
  type: 'daily',
  time: '07:00',
  timezone: 'UTC'
});

<SchedulePicker
  schedule={schedule}
  onChange={setSchedule}
/>
```

**Advanced Schedule:**
```jsx
const [schedule, setSchedule] = useState({
  type: 'weekly',
  time: '08:00',
  timezone: 'America/New_York',
  weekdays: [1, 2, 3, 4, 5], // Monday to Friday
  interval: 1
});

<SchedulePicker
  schedule={schedule}
  onChange={setSchedule}
  showTimezone={true}
/>
```

### TagInput

Component for managing routine tags with autocomplete.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `tags` | `string[]` | ✓ | - | Current tags |
| `onChange` | `(tags: string[]) => void` | ✓ | - | Tags change callback |
| `suggestions` | `string[]` | ✗ | `[]` | Tag suggestions |
| `maxTags` | `number` | ✗ | `10` | Maximum number of tags |
| `placeholder` | `string` | ✗ | `'Add tag...'` | Input placeholder |

#### Usage Example

```jsx
import { TagInput } from 'routine-md';

const [tags, setTags] = useState(['morning', 'health']);
const suggestions = ['morning', 'evening', 'health', 'work', 'personal'];

<TagInput
  tags={tags}
  onChange={setTags}
  suggestions={suggestions}
  maxTags={5}
  placeholder="Add a tag..."
/>
```

## Layout Components

### RoutineGrid

Grid layout component for displaying multiple routines.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `routines` | `Routine[]` | ✓ | - | Array of routines |
| `columns` | `number` | ✗ | `3` | Number of columns |
| `gap` | `number` | ✗ | `16` | Gap between items in pixels |
| `onRoutineClick` | `(routine: Routine) => void` | ✗ | - | Routine click callback |
| `renderRoutine` | `(routine: Routine) => ReactNode` | ✗ | - | Custom routine renderer |

#### Usage Example

```jsx
import { RoutineGrid, RoutineCard } from 'routine-md';

<RoutineGrid
  routines={routines}
  columns={2}
  gap={20}
  onRoutineClick={handleRoutineClick}
  renderRoutine={(routine) => (
    <RoutineCard
      routine={routine}
      showStats={true}
      onEdit={handleEdit}
      onDelete={handleDelete}
    />
  )}
/>
```

### Sidebar

Navigation sidebar component for routine management apps.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `routines` | `Routine[]` | ✓ | - | Array of routines |
| `activeRoutineId` | `string` | ✗ | - | Currently active routine ID |
| `onRoutineSelect` | `(routine: Routine) => void` | ✗ | - | Routine selection callback |
| `onCreateRoutine` | `() => void` | ✗ | - | Create routine callback |
| `collapsed` | `boolean` | ✗ | `false` | Collapsed state |

#### Usage Example

```jsx
import { Sidebar } from 'routine-md';

<Sidebar
  routines={routines}
  activeRoutineId={selectedRoutine?.id}
  onRoutineSelect={setSelectedRoutine}
  onCreateRoutine={() => setShowCreateForm(true)}
  collapsed={sidebarCollapsed}
/>
```

## Styling & Theming

### CSS Custom Properties

The component library uses CSS custom properties for theming:

```css
:root {
  /* Colors */
  --routine-primary: #3b82f6;
  --routine-primary-hover: #2563eb;
  --routine-success: #10b981;
  --routine-warning: #f59e0b;
  --routine-danger: #ef4444;
  --routine-gray-50: #f9fafb;
  --routine-gray-100: #f3f4f6;
  --routine-gray-200: #e5e7eb;
  --routine-gray-300: #d1d5db;
  --routine-gray-400: #9ca3af;
  --routine-gray-500: #6b7280;
  --routine-gray-600: #4b5563;
  --routine-gray-700: #374151;
  --routine-gray-800: #1f2937;
  --routine-gray-900: #111827;
  
  /* Typography */
  --routine-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --routine-font-size-xs: 0.75rem;
  --routine-font-size-sm: 0.875rem;
  --routine-font-size-base: 1rem;
  --routine-font-size-lg: 1.125rem;
  --routine-font-size-xl: 1.25rem;
  --routine-font-size-2xl: 1.5rem;
  
  /* Spacing */
  --routine-spacing-1: 0.25rem;
  --routine-spacing-2: 0.5rem;
  --routine-spacing-3: 0.75rem;
  --routine-spacing-4: 1rem;
  --routine-spacing-5: 1.25rem;
  --routine-spacing-6: 1.5rem;
  --routine-spacing-8: 2rem;
  
  /* Border radius */
  --routine-radius-sm: 0.25rem;
  --routine-radius: 0.375rem;
  --routine-radius-md: 0.5rem;
  --routine-radius-lg: 0.75rem;
  --routine-radius-xl: 1rem;
  
  /* Shadows */
  --routine-shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --routine-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  --routine-shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --routine-shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
}
```

### Dark Theme

```css
[data-theme="dark"] {
  --routine-primary: #60a5fa;
  --routine-primary-hover: #3b82f6;
  --routine-success: #34d399;
  --routine-warning: #fbbf24;
  --routine-danger: #f87171;
  --routine-gray-50: #1f2937;
  --routine-gray-100: #374151;
  --routine-gray-200: #4b5563;
  --routine-gray-300: #6b7280;
  --routine-gray-400: #9ca3af;
  --routine-gray-500: #d1d5db;
  --routine-gray-600: #e5e7eb;
  --routine-gray-700: #f3f4f6;
  --routine-gray-800: #f9fafb;
  --routine-gray-900: #ffffff;
}
```

### Custom Component Styling

```css
/* Custom routine card styling */
.my-routine-card {
  border: 2px solid var(--routine-primary);
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.my-routine-card .routine-card__title {
  font-size: var(--routine-font-size-xl);
  font-weight: 600;
}

/* Custom task list styling */
.my-task-list {
  background: var(--routine-gray-50);
  border-radius: var(--routine-radius-lg);
  padding: var(--routine-spacing-4);
}

.my-task-list .task-item {
  background: white;
  margin-bottom: var(--routine-spacing-2);
  box-shadow: var(--routine-shadow-sm);
}
```

## Advanced Usage

### Custom Hooks

The library provides several custom hooks for advanced usage:

#### useRoutines

```jsx
import { useRoutines } from 'routine-md';

const MyComponent = () => {
  const {
    routines,
    loading,
    error,
    createRoutine,
    updateRoutine,
    deleteRoutine,
    refreshRoutines
  } = useRoutines();
  
  // Component logic
};
```

#### useTaskCompletion

```jsx
import { useTaskCompletion } from 'routine-md';

const TaskComponent = ({ task }) => {
  const {
    completeTask,
    uncompleteTask,
    isCompleting,
    completionHistory
  } = useTaskCompletion(task.id);
  
  // Component logic
};
```

### Custom Renderers

Many components support custom renderers for advanced customization:

```jsx
import { TaskList } from 'routine-md';

const CustomTaskRenderer = ({ task, onComplete, onEdit }) => (
  <div className="custom-task">
    <h3>{task.name}</h3>
    <p>{task.description}</p>
    <button onClick={() => onComplete(task.id)}>
      {task.completed ? 'Undo' : 'Complete'}
    </button>
    <button onClick={() => onEdit(task)}>Edit</button>
  </div>
);

<TaskList
  tasks={tasks}
  onTaskComplete={handleComplete}
  renderTask={CustomTaskRenderer}
/>
```

### Event Handlers

Components emit various events for integration:

```jsx
import { RoutineCard } from 'routine-md';

<RoutineCard
  routine={routine}
  onEdit={handleEdit}
  onDelete={handleDelete}
  onToggleActive={handleToggleActive}
  onStatsClick={handleStatsClick}
  onTagClick={handleTagClick}
  onTaskClick={handleTaskClick}
/>
```

### Accessibility Features

All components include comprehensive accessibility features:

- ARIA labels and descriptions
- Keyboard navigation support
- Focus management
- Screen reader compatibility
- High contrast mode support
- Reduced motion preferences

```jsx
// Components automatically handle accessibility
<TaskList
  tasks={tasks}
  onTaskComplete={handleComplete}
  // Accessibility features are built-in:
  // - Arrow key navigation
  // - Enter/Space key activation
  // - Screen reader announcements
  // - Focus indicators
/>
```

---

This component documentation provides comprehensive information about all available components, their props, usage examples, and customization options. Each component is designed to be flexible, accessible, and easy to integrate into your routine management applications.