---
title: "Expo SQLite"
description: "Using Expo SQLite for local persistence in React Native applications."
date: 2026-10-10
updated: 2026-10-10
category: "react-native"
tags:
  - react-native
  - expo
  - sqlite
published: true
projects:
  - notes
---

# Expo SQLite

Expo SQLite provides a local SQLite database for React Native applications. It is useful for offline-first features, cached API data, and small collections of structured device data.

## Install

```bash
npx expo install expo-sqlite
```

## Open a database

```ts
import * as SQLite from 'expo-sqlite';

const db = await SQLite.openDatabaseAsync('app.db');
```

## Create a table and insert data

```ts
await db.execAsync(`
  PRAGMA journal_mode = WAL;
  CREATE TABLE IF NOT EXISTS notes (
    id INTEGER PRIMARY KEY NOT NULL,
    title TEXT NOT NULL,
    body TEXT NOT NULL,
    created_at TEXT NOT NULL
  );
`);

await db.runAsync(
  'INSERT INTO notes (title, body, created_at) VALUES (?, ?, ?)',
  'First note',
  'Stored locally on the device.',
  new Date().toISOString()
);
```

## Query rows

```ts
const notes = await db.getAllAsync<{
  id: number;
  title: string;
  body: string;
  created_at: string;
}>('SELECT * FROM notes ORDER BY created_at DESC');
```

Use parameterized queries for user-provided values, run migrations when the schema changes, and keep database access behind a small repository layer so screens do not own persistence details.
