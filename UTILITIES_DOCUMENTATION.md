# Utilities Documentation - Routine.md

## Table of Contents

1. [Overview](#overview)
2. [Date & Time Utilities](#date--time-utilities)
3. [Validation Utilities](#validation-utilities)
4. [Data Formatting Utilities](#data-formatting-utilities)
5. [Storage Utilities](#storage-utilities)
6. [Analytics Utilities](#analytics-utilities)
7. [Helper Functions](#helper-functions)
8. [Constants](#constants)
9. [Type Definitions](#type-definitions)

## Overview

The Routine.md utilities library provides a comprehensive set of helper functions, validators, formatters, and constants to simplify routine and task management operations. All utilities are tree-shakable and can be imported individually.

## Date & Time Utilities

### formatDate(date, format, options?)

Formats a date according to the specified format string.

#### Parameters
- `date` (Date | string | number): Date to format
- `format` (string): Format string pattern
- `options?` (object): Optional formatting options

#### Format Patterns
- `YYYY` - 4-digit year
- `MM` - 2-digit month
- `DD` - 2-digit day
- `HH` - 24-hour format hour
- `mm` - Minutes
- `ss` - Seconds
- `A` - AM/PM

#### Examples
```javascript
import { formatDate } from 'routine-md/utils';

// Basic formatting
formatDate(new Date(), 'YYYY-MM-DD'); // "2024-01-15"
formatDate(new Date(), 'MM/DD/YYYY'); // "01/15/2024"
formatDate(new Date(), 'DD MMM YYYY'); // "15 Jan 2024"

// Time formatting
formatDate(new Date(), 'HH:mm'); // "14:30"
formatDate(new Date(), 'h:mm A'); // "2:30 PM"

// With timezone
formatDate(new Date(), 'YYYY-MM-DD HH:mm', { timezone: 'America/New_York' });
```

### parseDate(dateString, format?)

Parses a date string into a Date object.

#### Parameters
- `dateString` (string): Date string to parse
- `format?` (string): Expected format (optional, auto-detects if not provided)

#### Examples
```javascript
import { parseDate } from 'routine-md/utils';

parseDate('2024-01-15'); // Date object
parseDate('01/15/2024', 'MM/DD/YYYY'); // Date object
parseDate('15 Jan 2024', 'DD MMM YYYY'); // Date object
```

### getWeekDates(date, options?)

Returns an array of dates for the week containing the given date.

#### Parameters
- `date` (Date): Reference date
- `options?` (object): Week configuration options
  - `startOfWeek` ('sunday' | 'monday'): Week start day (default: 'sunday')

#### Examples
```javascript
import { getWeekDates } from 'routine-md/utils';

const weekDates = getWeekDates(new Date());
// Returns: [Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday]

const mondayWeek = getWeekDates(new Date(), { startOfWeek: 'monday' });
// Returns: [Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday]
```

### getMonthDates(date)

Returns an array of all dates in the month containing the given date.

#### Examples
```javascript
import { getMonthDates } from 'routine-md/utils';

const monthDates = getMonthDates(new Date('2024-01-15'));
// Returns array of all dates in January 2024
```

### isToday(date)

Checks if a date is today.

#### Examples
```javascript
import { isToday } from 'routine-md/utils';

isToday(new Date()); // true
isToday(new Date('2024-01-01')); // false (unless today is Jan 1, 2024)
```

### addDays(date, days)

Adds or subtracts days from a date.

#### Examples
```javascript
import { addDays } from 'routine-md/utils';

const tomorrow = addDays(new Date(), 1);
const lastWeek = addDays(new Date(), -7);
```

### getTimezoneOffset(timezone)

Gets the timezone offset for a specific timezone.

#### Examples
```javascript
import { getTimezoneOffset } from 'routine-md/utils';

getTimezoneOffset('America/New_York'); // -300 (minutes)
getTimezoneOffset('Europe/London'); // 0 (minutes)
```

### convertTimezone(date, fromTimezone, toTimezone)

Converts a date from one timezone to another.

#### Examples
```javascript
import { convertTimezone } from 'routine-md/utils';

const utcDate = new Date('2024-01-15T12:00:00Z');
const nyTime = convertTimezone(utcDate, 'UTC', 'America/New_York');
```

## Validation Utilities

### validateRoutine(routineData)

Validates routine data structure and returns validation results.

#### Parameters
- `routineData` (object): Routine data to validate

#### Returns
```typescript
{
  valid: boolean;
  errors: Array<{
    field: string;
    message: string;
    code: string;
  }>;
}
```

#### Examples
```javascript
import { validateRoutine } from 'routine-md/utils';

const routineData = {
  name: "Morning Routine",
  description: "Start the day right",
  schedule: {
    type: "daily",
    time: "07:00"
  },
  tasks: [
    { name: "Meditation", duration: 10 }
  ]
};

const validation = validateRoutine(routineData);
if (!validation.valid) {
  validation.errors.forEach(error => {
    console.log(`${error.field}: ${error.message}`);
  });
}
```

### validateTask(taskData)

Validates task data structure.

#### Examples
```javascript
import { validateTask } from 'routine-md/utils';

const taskData = {
  name: "Exercise",
  duration: 30,
  order: 1
};

const validation = validateTask(taskData);
if (validation.valid) {
  console.log('Task data is valid');
}
```

### validateSchedule(scheduleData)

Validates schedule configuration.

#### Examples
```javascript
import { validateSchedule } from 'routine-md/utils';

const scheduleData = {
  type: "weekly",
  time: "08:00",
  weekdays: [1, 2, 3, 4, 5], // Monday to Friday
  timezone: "America/New_York"
};

const validation = validateSchedule(scheduleData);
```

### validateEmail(email)

Validates email address format.

#### Examples
```javascript
import { validateEmail } from 'routine-md/utils';

validateEmail('user@example.com'); // { valid: true, errors: [] }
validateEmail('invalid-email'); // { valid: false, errors: [...] }
```

### validateTimeString(timeString)

Validates time string format (HH:MM or H:MM).

#### Examples
```javascript
import { validateTimeString } from 'routine-md/utils';

validateTimeString('07:30'); // { valid: true, errors: [] }
validateTimeString('25:00'); // { valid: false, errors: [...] }
```

## Data Formatting Utilities

### formatDuration(minutes, options?)

Formats duration in minutes to human-readable format.

#### Parameters
- `minutes` (number): Duration in minutes
- `options?` (object): Formatting options
  - `format` ('short' | 'long'): Format style (default: 'short')
  - `precision` ('minutes' | 'hours'): Precision level

#### Examples
```javascript
import { formatDuration } from 'routine-md/utils';

formatDuration(30); // "30m"
formatDuration(90); // "1h 30m"
formatDuration(120); // "2h"

// Long format
formatDuration(90, { format: 'long' }); // "1 hour 30 minutes"
formatDuration(30, { format: 'long' }); // "30 minutes"

// Hour precision
formatDuration(90, { precision: 'hours' }); // "1.5h"
```

### formatProgress(completed, total, options?)

Formats progress as percentage or fraction.

#### Parameters
- `completed` (number): Completed count
- `total` (number): Total count
- `options?` (object): Formatting options
  - `format` ('percentage' | 'fraction'): Format type (default: 'percentage')
  - `precision` (number): Decimal places for percentage

#### Examples
```javascript
import { formatProgress } from 'routine-md/utils';

formatProgress(3, 5); // "60%"
formatProgress(3, 5, { format: 'fraction' }); // "3/5"
formatProgress(1, 3, { precision: 1 }); // "33.3%"
```

### formatCompletionStreak(streak)

Formats completion streak count.

#### Examples
```javascript
import { formatCompletionStreak } from 'routine-md/utils';

formatCompletionStreak(1); // "1 day streak"
formatCompletionStreak(7); // "1 week streak"
formatCompletionStreak(30); // "1 month streak"
formatCompletionStreak(365); // "1 year streak"
```

### formatRelativeTime(date, options?)

Formats relative time (e.g., "2 hours ago", "in 3 days").

#### Examples
```javascript
import { formatRelativeTime } from 'routine-md/utils';

const now = new Date();
const twoHoursAgo = addDays(now, 0, -2); // 2 hours ago
const tomorrow = addDays(now, 1);

formatRelativeTime(twoHoursAgo); // "2 hours ago"
formatRelativeTime(tomorrow); // "in 1 day"
```

## Storage Utilities

### saveToLocalStorage(key, data)

Saves data to localStorage with JSON serialization.

#### Examples
```javascript
import { saveToLocalStorage } from 'routine-md/utils';

const routineData = { name: "Morning Routine", tasks: [...] };
saveToLocalStorage('routine_123', routineData);
```

### loadFromLocalStorage(key, defaultValue?)

Loads and deserializes data from localStorage.

#### Examples
```javascript
import { loadFromLocalStorage } from 'routine-md/utils';

const routineData = loadFromLocalStorage('routine_123', null);
const settings = loadFromLocalStorage('user_settings', { theme: 'light' });
```

### removeFromLocalStorage(key)

Removes item from localStorage.

#### Examples
```javascript
import { removeFromLocalStorage } from 'routine-md/utils';

removeFromLocalStorage('routine_123');
```

### clearRoutineStorage()

Clears all routine-related data from localStorage.

#### Examples
```javascript
import { clearRoutineStorage } from 'routine-md/utils';

clearRoutineStorage(); // Removes all routine data
```

### exportRoutineData(routines)

Exports routine data to JSON format for backup.

#### Examples
```javascript
import { exportRoutineData } from 'routine-md/utils';

const backup = exportRoutineData(userRoutines);
const blob = new Blob([backup], { type: 'application/json' });
// Create download link...
```

### importRoutineData(jsonData)

Imports routine data from JSON backup.

#### Examples
```javascript
import { importRoutineData } from 'routine-md/utils';

const fileContent = await file.text();
const importedRoutines = importRoutineData(fileContent);
```

## Analytics Utilities

### calculateCompletionRate(completions, totalDays)

Calculates completion rate percentage.

#### Examples
```javascript
import { calculateCompletionRate } from 'routine-md/utils';

const rate = calculateCompletionRate(25, 30); // 83.33
```

### calculateStreak(completionHistory)

Calculates current completion streak.

#### Parameters
- `completionHistory` (array): Array of completion objects with dates

#### Examples
```javascript
import { calculateStreak } from 'routine-md/utils';

const history = [
  { date: '2024-01-13', completed: true },
  { date: '2024-01-14', completed: true },
  { date: '2024-01-15', completed: true }
];

const streak = calculateStreak(history); // 3
```

### calculateBestStreak(completionHistory)

Calculates the best (longest) streak from history.

#### Examples
```javascript
import { calculateBestStreak } from 'routine-md/utils';

const bestStreak = calculateBestStreak(completionHistory); // 15
```

### getCompletionTrend(completionHistory, period)

Analyzes completion trend over a period.

#### Parameters
- `completionHistory` (array): Completion history data
- `period` ('week' | 'month' | 'quarter'): Analysis period

#### Returns
```typescript
{
  trend: 'increasing' | 'decreasing' | 'stable';
  percentage: number; // Change percentage
  data: Array<{ date: string; rate: number }>;
}
```

#### Examples
```javascript
import { getCompletionTrend } from 'routine-md/utils';

const trend = getCompletionTrend(completionHistory, 'month');
console.log(`Trend: ${trend.trend}, Change: ${trend.percentage}%`);
```

### generateProgressReport(routines, dateRange)

Generates comprehensive progress report.

#### Examples
```javascript
import { generateProgressReport } from 'routine-md/utils';

const report = generateProgressReport(userRoutines, {
  startDate: '2024-01-01',
  endDate: '2024-01-31'
});

console.log(`Total completion rate: ${report.overallCompletionRate}%`);
console.log(`Best performing routine: ${report.bestRoutine.name}`);
```

## Helper Functions

### debounce(func, delay)

Creates a debounced version of a function.

#### Examples
```javascript
import { debounce } from 'routine-md/utils';

const debouncedSearch = debounce((query) => {
  performSearch(query);
}, 300);

// Usage in React
const handleInputChange = debounce((value) => {
  setSearchQuery(value);
}, 500);
```

### throttle(func, limit)

Creates a throttled version of a function.

#### Examples
```javascript
import { throttle } from 'routine-md/utils';

const throttledScroll = throttle(() => {
  updateScrollPosition();
}, 100);

window.addEventListener('scroll', throttledScroll);
```

### generateId(prefix?)

Generates a unique ID with optional prefix.

#### Examples
```javascript
import { generateId } from 'routine-md/utils';

const routineId = generateId('routine'); // "routine_abc123def456"
const taskId = generateId('task'); // "task_xyz789uvw012"
const uniqueId = generateId(); // "abc123def456"
```

### cloneDeep(obj)

Creates a deep copy of an object.

#### Examples
```javascript
import { cloneDeep } from 'routine-md/utils';

const originalRoutine = { name: "Test", tasks: [{ name: "Task 1" }] };
const copiedRoutine = cloneDeep(originalRoutine);
```

### sortBy(array, key, direction?)

Sorts an array by a specific key.

#### Parameters
- `array` (array): Array to sort
- `key` (string | function): Sort key or function
- `direction` ('asc' | 'desc'): Sort direction (default: 'asc')

#### Examples
```javascript
import { sortBy } from 'routine-md/utils';

const tasks = [
  { name: "Exercise", order: 2 },
  { name: "Meditation", order: 1 },
  { name: "Reading", order: 3 }
];

const sortedTasks = sortBy(tasks, 'order'); // Sorted by order ascending
const sortedByName = sortBy(tasks, 'name', 'desc'); // Sorted by name descending

// Using function
const sortedByLength = sortBy(tasks, task => task.name.length);
```

### groupBy(array, key)

Groups array items by a specific key.

#### Examples
```javascript
import { groupBy } from 'routine-md/utils';

const routines = [
  { name: "Morning", category: "daily" },
  { name: "Evening", category: "daily" },
  { name: "Weekly Review", category: "weekly" }
];

const grouped = groupBy(routines, 'category');
// Result: { daily: [...], weekly: [...] }
```

### filterBy(array, filters)

Filters array by multiple criteria.

#### Examples
```javascript
import { filterBy } from 'routine-md/utils';

const routines = [
  { name: "Morning", active: true, tags: ["health"] },
  { name: "Evening", active: false, tags: ["relaxation"] },
  { name: "Workout", active: true, tags: ["health", "fitness"] }
];

const filtered = filterBy(routines, {
  active: true,
  tags: tag => tag.includes("health")
});
```

### pick(obj, keys)

Creates a new object with only specified keys.

#### Examples
```javascript
import { pick } from 'routine-md/utils';

const routine = {
  id: "123",
  name: "Morning Routine",
  description: "Start the day",
  tasks: [...],
  createdAt: "2024-01-01"
};

const summary = pick(routine, ['id', 'name', 'description']);
// Result: { id: "123", name: "Morning Routine", description: "Start the day" }
```

### omit(obj, keys)

Creates a new object excluding specified keys.

#### Examples
```javascript
import { omit } from 'routine-md/utils';

const routineWithoutMeta = omit(routine, ['createdAt', 'updatedAt']);
```

## Constants

### TIME_FORMATS

Predefined time format constants.

```javascript
import { TIME_FORMATS } from 'routine-md/utils';

console.log(TIME_FORMATS.ISO_DATE); // "YYYY-MM-DD"
console.log(TIME_FORMATS.US_DATE); // "MM/DD/YYYY"
console.log(TIME_FORMATS.TIME_24H); // "HH:mm"
console.log(TIME_FORMATS.TIME_12H); // "h:mm A"
```

### SCHEDULE_TYPES

Available schedule types.

```javascript
import { SCHEDULE_TYPES } from 'routine-md/utils';

console.log(SCHEDULE_TYPES.DAILY); // "daily"
console.log(SCHEDULE_TYPES.WEEKLY); // "weekly"
console.log(SCHEDULE_TYPES.MONTHLY); // "monthly"
console.log(SCHEDULE_TYPES.CUSTOM); // "custom"
```

### VALIDATION_MESSAGES

Standard validation error messages.

```javascript
import { VALIDATION_MESSAGES } from 'routine-md/utils';

console.log(VALIDATION_MESSAGES.REQUIRED); // "This field is required"
console.log(VALIDATION_MESSAGES.INVALID_EMAIL); // "Please enter a valid email address"
```

### DEFAULT_SETTINGS

Default application settings.

```javascript
import { DEFAULT_SETTINGS } from 'routine-md/utils';

console.log(DEFAULT_SETTINGS.THEME); // "light"
console.log(DEFAULT_SETTINGS.TIMEZONE); // "UTC"
console.log(DEFAULT_SETTINGS.DATE_FORMAT); // "YYYY-MM-DD"
```

## Type Definitions

### Routine Type

```typescript
interface Routine {
  id: string;
  name: string;
  description?: string;
  schedule: Schedule;
  tasks: Task[];
  tags: string[];
  active: boolean;
  createdAt: string;
  updatedAt: string;
}
```

### Task Type

```typescript
interface Task {
  id: string;
  name: string;
  description?: string;
  duration: number; // in minutes
  order: number;
  completed: boolean;
  completedAt?: string;
  notes?: string;
}
```

### Schedule Type

```typescript
interface Schedule {
  type: 'daily' | 'weekly' | 'monthly' | 'custom';
  time: string; // HH:MM format
  timezone: string;
  weekdays?: number[]; // 0-6, Sunday = 0
  monthDays?: number[]; // 1-31
  interval?: number; // Every N days/weeks/months
}
```

### ValidationResult Type

```typescript
interface ValidationResult {
  valid: boolean;
  errors: ValidationError[];
}

interface ValidationError {
  field: string;
  message: string;
  code: string;
}
```

### CompletionHistory Type

```typescript
interface CompletionRecord {
  date: string; // YYYY-MM-DD
  completed: boolean;
  completedAt?: string;
  qualityRating?: number; // 1-5
  notes?: string;
}
```

---

This utilities documentation provides comprehensive information about all helper functions, validators, formatters, and constants available in the Routine.md system. These utilities are designed to simplify common operations and provide consistent behavior across the application.