---
name: react-best-practices
description: React performance optimization guidelines for writing, reviewing, and refactoring frontend code. Use this skill when working on React components, rendering behavior, data fetching, bundle optimization, or general performance improvements.
license: MIT
metadata:
  author: imported
  version: "1.0.0"
---

# React Best Practices

Comprehensive performance optimization guide for React applications. Contains rule references across 8 categories, prioritized by impact to guide automated refactoring and code generation.

## When to Apply

Reference these guidelines when:
- Writing new React components or frontend views
- Implementing data fetching (client or server-side)
- Reviewing code for performance issues
- Refactoring existing React code
- Optimizing bundle size or load times

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | Eliminating Waterfalls | CRITICAL | `async-` |
| 2 | Bundle Size Optimization | CRITICAL | `bundle-` |
| 3 | Server-Side Performance | HIGH | `server-` |
| 4 | Client-Side Data Fetching | MEDIUM-HIGH | `client-` |
| 5 | Re-render Optimization | MEDIUM | `rerender-` |
| 6 | Rendering Performance | MEDIUM | `rendering-` |
| 7 | JavaScript Performance | LOW-MEDIUM | `js-` |
| 8 | Advanced Patterns | LOW | `advanced-` |

## Quick Reference

### 1. Eliminating Waterfalls (CRITICAL)

Rule references will be added incrementally under `rules/async-*.md`.

### 2. Bundle Size Optimization (CRITICAL)

Rule references will be added incrementally under `rules/bundle-*.md`.

### 3. Server-Side Performance (HIGH)

Rule references will be added incrementally under `rules/server-*.md`.

### 4. Client-Side Data Fetching (MEDIUM-HIGH)

Rule references will be added incrementally under `rules/client-*.md`.

### 5. Re-render Optimization (MEDIUM)

Rule references will be added incrementally under `rules/rerender-*.md`.

### 6. Rendering Performance (MEDIUM)

Rule references will be added incrementally under `rules/rendering-*.md`.

### 7. JavaScript Performance (LOW-MEDIUM)

Rule references will be added incrementally under `rules/js-*.md`.

### 8. Advanced Patterns (LOW)

Rule references will be added incrementally under `rules/advanced-*.md`.

## How to Use

Read individual rule files for detailed explanations and code examples as they are added:

```
rules/async-*.md
rules/bundle-*.md
```

Each rule file contains:
- Brief explanation of why it matters
- Incorrect code example with explanation
- Correct code example with explanation
- Additional context and references

## Rule Sources

This skill uses the individual rule files under `rules/` as the source of truth.
