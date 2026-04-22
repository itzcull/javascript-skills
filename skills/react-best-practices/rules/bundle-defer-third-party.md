---
title: Defer Non-Critical Third-Party Libraries
impact: MEDIUM
impactDescription: loads after hydration
tags: bundle, third-party, analytics, defer
---

## Defer Non-Critical Third-Party Libraries

Analytics, logging, and error tracking don't block user interaction. Load them after hydration.

**Incorrect (blocks initial bundle):**

```tsx
import { Analytics } from './analytics'

export function AppShell() {
  return (
    <>
      <main>{/* app content */}</main>
      <Analytics />
    </>
  )
}
```

**Correct (loads after hydration):**

```tsx
import { useEffect, useState } from 'react'

function DeferredAnalytics() {
  const [Analytics, setAnalytics] = useState<null | React.ComponentType>(null)

  useEffect(() => {
    let mounted = true

    void import('./analytics').then(mod => {
      if (mounted) {
        setAnalytics(() => mod.Analytics)
      }
    })

    return () => {
      mounted = false
    }
  }, [])

  return Analytics ? <Analytics /> : null
}
```
