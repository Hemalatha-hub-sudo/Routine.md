# API Documentation - Routine.md

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Core APIs](#core-apis)
4. [Components](#components)
5. [Utilities](#utilities)
6. [Error Handling](#error-handling)
7. [Examples](#examples)

## Overview

Routine.md is a comprehensive task and routine management system that provides APIs for creating, managing, and tracking daily routines, habits, and tasks.

### Base URL
```
https://api.routine.md/v1
```

### Content Type
All API requests should include:
```
Content-Type: application/json
```

## Authentication

### API Key Authentication

All API requests require authentication using an API key in the header:

```http
Authorization: Bearer YOUR_API_KEY
```

#### Example
```javascript
const headers = {
  'Authorization': 'Bearer sk-1234567890abcdef',
  'Content-Type': 'application/json'
};
```

## Core APIs

### Routines API

#### Create Routine
Creates a new routine with specified tasks and schedule.

**Endpoint:** `POST /routines`

**Parameters:**
- `name` (string, required): Name of the routine
- `description` (string, optional): Description of the routine
- `schedule` (object, required): Schedule configuration
- `tasks` (array, required): List of tasks in the routine
- `tags` (array, optional): Tags for categorization

**Example Request:**
```javascript
const createRoutine = async (routineData) => {
  const response = await fetch('https://api.routine.md/v1/routines', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: "Morning Routine",
      description: "My daily morning routine",
      schedule: {
        type: "daily",
        time: "07:00",
        timezone: "UTC"
      },
      tasks: [
        {
          name: "Meditation",
          duration: 10,
          order: 1
        },
        {
          name: "Exercise",
          duration: 30,
          order: 2
        }
      ],
      tags: ["morning", "health"]
    })
  });
  
  return response.json();
};
```

**Example Response:**
```json
{
  "id": "routine_123456",
  "name": "Morning Routine",
  "description": "My daily morning routine",
  "schedule": {
    "type": "daily",
    "time": "07:00",
    "timezone": "UTC"
  },
  "tasks": [
    {
      "id": "task_789",
      "name": "Meditation",
      "duration": 10,
      "order": 1
    },
    {
      "id": "task_790",
      "name": "Exercise",
      "duration": 30,
      "order": 2
    }
  ],
  "tags": ["morning", "health"],
  "created_at": "2024-01-15T07:00:00Z",
  "updated_at": "2024-01-15T07:00:00Z"
}
```

#### Get Routine
Retrieves a specific routine by ID.

**Endpoint:** `GET /routines/{routine_id}`

**Example:**
```javascript
const getRoutine = async (routineId) => {
  const response = await fetch(`https://api.routine.md/v1/routines/${routineId}`, {
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY'
    }
  });
  
  return response.json();
};
```

#### List Routines
Retrieves all routines for the authenticated user.

**Endpoint:** `GET /routines`

**Query Parameters:**
- `page` (integer, optional): Page number (default: 1)
- `limit` (integer, optional): Items per page (default: 20, max: 100)
- `tags` (string, optional): Filter by tags (comma-separated)
- `active` (boolean, optional): Filter by active status

**Example:**
```javascript
const listRoutines = async (filters = {}) => {
  const params = new URLSearchParams(filters);
  const response = await fetch(`https://api.routine.md/v1/routines?${params}`, {
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY'
    }
  });
  
  return response.json();
};

// Usage
const activeRoutines = await listRoutines({ active: true, tags: 'morning,health' });
```

#### Update Routine
Updates an existing routine.

**Endpoint:** `PUT /routines/{routine_id}`

**Example:**
```javascript
const updateRoutine = async (routineId, updates) => {
  const response = await fetch(`https://api.routine.md/v1/routines/${routineId}`, {
    method: 'PUT',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(updates)
  });
  
  return response.json();
};
```

#### Delete Routine
Deletes a routine.

**Endpoint:** `DELETE /routines/{routine_id}`

**Example:**
```javascript
const deleteRoutine = async (routineId) => {
  const response = await fetch(`https://api.routine.md/v1/routines/${routineId}`, {
    method: 'DELETE',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY'
    }
  });
  
  return response.status === 204;
};
```

### Tasks API

#### Create Task
Creates a new task within a routine.

**Endpoint:** `POST /routines/{routine_id}/tasks`

**Example:**
```javascript
const createTask = async (routineId, taskData) => {
  const response = await fetch(`https://api.routine.md/v1/routines/${routineId}/tasks`, {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: "Read for 15 minutes",
      duration: 15,
      order: 3,
      notes: "Focus on personal development books"
    })
  });
  
  return response.json();
};
```

#### Complete Task
Marks a task as completed for a specific date.

**Endpoint:** `POST /tasks/{task_id}/complete`

**Example:**
```javascript
const completeTask = async (taskId, completionData) => {
  const response = await fetch(`https://api.routine.md/v1/tasks/${taskId}/complete`, {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      completed_at: new Date().toISOString(),
      notes: "Felt great after completing this!",
      quality_rating: 5
    })
  });
  
  return response.json();
};
```

### Analytics API

#### Get Routine Statistics
Retrieves completion statistics for a routine.

**Endpoint:** `GET /routines/{routine_id}/stats`

**Query Parameters:**
- `start_date` (string, optional): Start date in YYYY-MM-DD format
- `end_date` (string, optional): End date in YYYY-MM-DD format
- `period` (string, optional): Aggregation period ('day', 'week', 'month')

**Example:**
```javascript
const getRoutineStats = async (routineId, options = {}) => {
  const params = new URLSearchParams(options);
  const response = await fetch(`https://api.routine.md/v1/routines/${routineId}/stats?${params}`, {
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY'
    }
  });
  
  return response.json();
};

// Get last 30 days stats
const stats = await getRoutineStats('routine_123456', {
  start_date: '2024-01-01',
  end_date: '2024-01-30',
  period: 'day'
});
```

## Components

### RoutineCard Component

A reusable component for displaying routine information.

#### Props
- `routine` (object, required): Routine data object
- `onEdit` (function, optional): Callback for edit action
- `onDelete` (function, optional): Callback for delete action
- `showStats` (boolean, optional): Whether to show completion statistics

#### Usage Example
```jsx
import { RoutineCard } from 'routine-md';

const MyRoutines = ({ routines }) => {
  const handleEdit = (routine) => {
    console.log('Editing routine:', routine.id);
  };
  
  const handleDelete = (routine) => {
    console.log('Deleting routine:', routine.id);
  };
  
  return (
    <div className="routines-grid">
      {routines.map(routine => (
        <RoutineCard
          key={routine.id}
          routine={routine}
          onEdit={handleEdit}
          onDelete={handleDelete}
          showStats={true}
        />
      ))}
    </div>
  );
};
```

### TaskList Component

A component for displaying and managing tasks within a routine.

#### Props
- `tasks` (array, required): Array of task objects
- `onTaskComplete` (function, required): Callback when task is completed
- `onTaskEdit` (function, optional): Callback for task editing
- `editable` (boolean, optional): Whether tasks can be edited

#### Usage Example
```jsx
import { TaskList } from 'routine-md';

const RoutineDetail = ({ routine }) => {
  const handleTaskComplete = async (taskId) => {
    await completeTask(taskId, {
      completed_at: new Date().toISOString(),
      quality_rating: 5
    });
  };
  
  return (
    <div>
      <h2>{routine.name}</h2>
      <TaskList
        tasks={routine.tasks}
        onTaskComplete={handleTaskComplete}
        editable={true}
      />
    </div>
  );
};
```

### SchedulePicker Component

A component for selecting routine schedules.

#### Props
- `schedule` (object, required): Current schedule configuration
- `onChange` (function, required): Callback when schedule changes
- `disabled` (boolean, optional): Whether the picker is disabled

#### Usage Example
```jsx
import { SchedulePicker } from 'routine-md';

const RoutineForm = () => {
  const [schedule, setSchedule] = useState({
    type: 'daily',
    time: '07:00',
    timezone: 'UTC'
  });
  
  return (
    <form>
      <SchedulePicker
        schedule={schedule}
        onChange={setSchedule}
      />
    </form>
  );
};
```

## Utilities

### Date Utilities

#### formatDate(date, format)
Formats a date according to the specified format.

**Parameters:**
- `date` (Date|string): Date to format
- `format` (string): Format string ('YYYY-MM-DD', 'MM/DD/YYYY', etc.)

**Example:**
```javascript
import { formatDate } from 'routine-md/utils';

const formatted = formatDate(new Date(), 'YYYY-MM-DD');
console.log(formatted); // "2024-01-15"
```

#### getWeekDates(date)
Returns an array of dates for the week containing the given date.

**Example:**
```javascript
import { getWeekDates } from 'routine-md/utils';

const weekDates = getWeekDates(new Date());
console.log(weekDates); // [Date, Date, Date, Date, Date, Date, Date]
```

### Validation Utilities

#### validateRoutine(routineData)
Validates routine data structure.

**Example:**
```javascript
import { validateRoutine } from 'routine-md/utils';

const routineData = {
  name: "Morning Routine",
  schedule: { type: "daily", time: "07:00" },
  tasks: [{ name: "Meditation", duration: 10 }]
};

const validation = validateRoutine(routineData);
if (!validation.valid) {
  console.log('Validation errors:', validation.errors);
}
```

#### validateTask(taskData)
Validates task data structure.

**Example:**
```javascript
import { validateTask } from 'routine-md/utils';

const taskData = {
  name: "Exercise",
  duration: 30,
  order: 1
};

const validation = validateTask(taskData);
```

## Error Handling

### Error Response Format

All API errors follow a consistent format:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request data is invalid",
    "details": [
      {
        "field": "name",
        "message": "Name is required"
      }
    ]
  }
}
```

### Common Error Codes

- `AUTHENTICATION_ERROR`: Invalid or missing API key
- `AUTHORIZATION_ERROR`: Insufficient permissions
- `VALIDATION_ERROR`: Invalid request data
- `NOT_FOUND`: Resource not found
- `RATE_LIMIT_EXCEEDED`: Too many requests
- `INTERNAL_ERROR`: Server error

### Error Handling Example

```javascript
const handleApiError = (error) => {
  switch (error.code) {
    case 'AUTHENTICATION_ERROR':
      // Redirect to login
      window.location.href = '/login';
      break;
    case 'VALIDATION_ERROR':
      // Show validation errors
      error.details.forEach(detail => {
        showFieldError(detail.field, detail.message);
      });
      break;
    case 'RATE_LIMIT_EXCEEDED':
      // Show rate limit message
      showNotification('Please slow down your requests');
      break;
    default:
      showNotification('An unexpected error occurred');
  }
};

// Usage with API calls
const createRoutineWithErrorHandling = async (routineData) => {
  try {
    const response = await fetch('https://api.routine.md/v1/routines', {
      method: 'POST',
      headers: {
        'Authorization': 'Bearer YOUR_API_KEY',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(routineData)
    });
    
    if (!response.ok) {
      const error = await response.json();
      handleApiError(error.error);
      return null;
    }
    
    return response.json();
  } catch (error) {
    console.error('Network error:', error);
    showNotification('Network error occurred');
    return null;
  }
};
```

## Examples

### Complete Morning Routine Setup

```javascript
// 1. Create a morning routine
const morningRoutine = await createRoutine({
  name: "Morning Routine",
  description: "Start the day right",
  schedule: {
    type: "daily",
    time: "06:30",
    timezone: "America/New_York"
  },
  tasks: [
    { name: "Meditation", duration: 10, order: 1 },
    { name: "Exercise", duration: 30, order: 2 },
    { name: "Healthy Breakfast", duration: 15, order: 3 },
    { name: "Review Daily Goals", duration: 5, order: 4 }
  ],
  tags: ["morning", "health", "productivity"]
});

console.log('Created routine:', morningRoutine.id);

// 2. Get routine stats
const stats = await getRoutineStats(morningRoutine.id, {
  start_date: '2024-01-01',
  end_date: '2024-01-31',
  period: 'day'
});

console.log('Completion rate:', stats.completion_rate);

// 3. Complete tasks
for (const task of morningRoutine.tasks) {
  await completeTask(task.id, {
    completed_at: new Date().toISOString(),
    quality_rating: 5,
    notes: `Completed ${task.name} successfully`
  });
}
```

### React Component Integration

```jsx
import React, { useState, useEffect } from 'react';
import { RoutineCard, TaskList, SchedulePicker } from 'routine-md';

const RoutineApp = () => {
  const [routines, setRoutines] = useState([]);
  const [selectedRoutine, setSelectedRoutine] = useState(null);
  
  useEffect(() => {
    loadRoutines();
  }, []);
  
  const loadRoutines = async () => {
    try {
      const response = await listRoutines({ active: true });
      setRoutines(response.data);
    } catch (error) {
      console.error('Failed to load routines:', error);
    }
  };
  
  const handleRoutineEdit = (routine) => {
    setSelectedRoutine(routine);
  };
  
  const handleTaskComplete = async (taskId) => {
    try {
      await completeTask(taskId, {
        completed_at: new Date().toISOString(),
        quality_rating: 5
      });
      
      // Refresh routine data
      if (selectedRoutine) {
        const updated = await getRoutine(selectedRoutine.id);
        setSelectedRoutine(updated);
      }
    } catch (error) {
      console.error('Failed to complete task:', error);
    }
  };
  
  return (
    <div className="routine-app">
      <div className="routines-sidebar">
        <h2>My Routines</h2>
        {routines.map(routine => (
          <RoutineCard
            key={routine.id}
            routine={routine}
            onEdit={handleRoutineEdit}
            showStats={true}
          />
        ))}
      </div>
      
      <div className="routine-detail">
        {selectedRoutine && (
          <>
            <h1>{selectedRoutine.name}</h1>
            <p>{selectedRoutine.description}</p>
            <TaskList
              tasks={selectedRoutine.tasks}
              onTaskComplete={handleTaskComplete}
              editable={true}
            />
          </>
        )}
      </div>
    </div>
  );
};

export default RoutineApp;
```

### Node.js Backend Integration

```javascript
const express = require('express');
const { validateRoutine, formatDate } = require('routine-md/utils');

const app = express();
app.use(express.json());

// Middleware for API key validation
const authenticateApiKey = (req, res, next) => {
  const apiKey = req.headers.authorization?.replace('Bearer ', '');
  
  if (!apiKey || !isValidApiKey(apiKey)) {
    return res.status(401).json({
      error: {
        code: 'AUTHENTICATION_ERROR',
        message: 'Invalid or missing API key'
      }
    });
  }
  
  req.userId = getUserIdFromApiKey(apiKey);
  next();
};

// Create routine endpoint
app.post('/api/routines', authenticateApiKey, async (req, res) => {
  try {
    // Validate routine data
    const validation = validateRoutine(req.body);
    if (!validation.valid) {
      return res.status(400).json({
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Invalid routine data',
          details: validation.errors
        }
      });
    }
    
    // Create routine in database
    const routine = await createRoutineInDatabase({
      ...req.body,
      userId: req.userId,
      createdAt: new Date()
    });
    
    res.status(201).json(routine);
  } catch (error) {
    console.error('Error creating routine:', error);
    res.status(500).json({
      error: {
        code: 'INTERNAL_ERROR',
        message: 'Failed to create routine'
      }
    });
  }
});

app.listen(3000, () => {
  console.log('Routine.md API server running on port 3000');
});
```

---

This documentation covers all the major public APIs, components, and utilities for the Routine.md system. Each section includes comprehensive examples and usage instructions to help developers integrate and use the system effectively.