# React + TypeScript Feature-First Style Guide

A **feature-first** approach to building **scalable, maintainable, and predictable** React applications with TypeScript. Designed for **clarity, consistency, and efficiency**, this guide minimizes cognitive load and enforces best practices.

---

## Core Principles

✅ **Feature-First Development** – Features are self-contained and organized around routes.\
✅ **Minimal Cognitive Load** – Engineers should immediately know where code belongs.\
✅ **Predictability & Consistency** – Every feature follows the same structure.\
✅ **No Unnecessary Abstractions** – Complexity is only introduced when necessary.\
✅ **Automation Over Convention** – Enforceable via ESLint/Prettier instead of manual rules.\

**Additional Resources:**

- [Google TypeScript Style Guide](https://google.github.io/styleguide/tsguide.html)
- [Airbnb React Style Guide](https://airbnb.io/javascript/react/)
- [My React TypeScript Style Guide](https://github.com/mlane/react-typescript-style-guide)

---

## Feature-First Folder Structure

### **What is a Feature?**

A **feature** is a **self-contained module** that represents:

- A **route** (`/guides/:guideId`)
- A **significant UI section**
- **Reusable business logic specific to a domain**

### **Feature Structure Example**

```
pages/guides/search  → Feature (route: `/guides/search`)
  GuideSearch.tsx  → Main component

pages/guides/:guideId  → Feature (route: `/guides/:guideId`)
  Guide.tsx  → Main component
  hooks/useGetGuideQuery.ts  → Query for getting details
  hooks/useGuideMetricShare.ts  → Handles copy-to-clipboard/sharing
  components/GuideHero.tsx  → Feature-scoped component

pages/profile/:accountHandle  → Feature (route: `/profile/:accountHandle`)
  Profile.tsx → Main component

pages/profile/:accountHandle/guides  → Feature (route: `/profile/:accountHandle/guides`)
  ProfileGuides.tsx → Main component
```

✅ **Flat structure:** No deep nesting inside features.\
✅ **Feature-scoped:** Hooks and components belong inside the feature they support.\
✅ **No `common/` folder** – Instead, use `src/hooks/` for sitewide hooks and `src/components/` for reusable components.

---

## Component Structure

**Ordering Inside a Component:**\
1️⃣ **Hooks** (`useState`, `useEffect`, etc.)  
2️⃣ **Local Variables** (constants, derived values)  
3️⃣ **useEffect Hooks** (side effects, lifecycle logic)  
4️⃣ **Event Handlers & Functions**  
5️⃣ **Return Statement (JSX)**

**Example Component:**

```tsx
export const Profile = () => {
  const { hasError, isLoading, profileData } = useGetProfileQuery()

  if (isLoading) return <ProfileLoading />

  if (hasError) return <ProfileEmpty />

  return (
    <section>
      <ProfileHero />
      <ProfileContent />
    </section>
  )
}
```

---

## Naming Conventions

| Item                              | Naming Convention                                          |
| --------------------------------- | ---------------------------------------------------------- |
| **Variables, Functions, Hooks**   | `camelCase` (e.g., `getUserProfile`)                       |
| **Components, Enums, Interfaces** | `PascalCase` (e.g., `UserProfileCard`)                     |
| **Folders**                       | `kebab-case` (e.g., `profile-settings/`)                   |
| **Constants**                     | Defined within feature files, not in a global folder.      |
| **GraphQL Queries/Mutations**     | `camelCase` inside operations, `PascalCase` for file names |

---

## GraphQL Queries & Mutations

| Type                      | Placement                         |
| ------------------------- | --------------------------------- |
| **Feature-based Queries** | Inside `pages/featureName/hooks/` |
| **Sitewide Queries**      | Inside `src/hooks/`               |

**Correct Naming Examples:**

```graphql
query GetGuide($id: ID!) { ... }             # Fetches full guide details
query GetGuideEvents($guideId: ID!) { ... }  # Fetches related guide events
query GetGuideImages($guideId: ID!) { ... }  # Fetches guide images
```

**Additional Resources:**

- [GraphQL Best Practices](https://graphql.org/learn/best-practices/)

---

## Types & Interfaces

| When to Use                                                    | Rule                                                    |
| -------------------------------------------------------------- | ------------------------------------------------------- |
| **Props for Components**                                       | Use `interface` (e.g., `interface ProfileProps {}`)     |
| **Everything Else (Utilities, Hooks, GraphQL, API Responses)** | Use `type` (e.g., `type UseGetProfileQueryResult = {}`) |
| **Extracting Subsets of Types**                                | Use `Pick<>` and `Omit<>`                               |

**Additional Resources:**

- [TypeScript Handbook: Types vs. Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

---

## Feature Flags

| Item        | Rule                                                  |
| ----------- | ----------------------------------------------------- |
| **Storage** | Centralized in `config/feature-flags/featureFlags.ts` |
| **Usage**   | Accessed via `useFlag` hook                           |
| **Cleanup** | Feature flags should be short-lived                   |

**Additional Resources:**

- [Feature Flags Best Practices](https://martinfowler.com/articles/feature-toggles.html)

---

## Comments & Documentation

✅ **Code should be self-explanatory; avoid unnecessary comments.**  
✅ **Use JSDoc `@todo` for tracking future work.**  
✅ **Only document "why", not "what" the code does.**

**Example:**

```ts
/** @todo Remove this workaround when the new API version is available */
const getUserPreferences = async (userId: string) => {
  return await fetch(`/api/preferences/${userId}`)
}
```

**Additional Resources:**

- [Clean Code Principles](https://www.oreilly.com/library/view/clean-code/9780136083238/)

---

## Final Thoughts

**This cheat sheet ensures clarity, predictability, and minimal cognitive load.**

✅ **Keep rules minimal & enforceable.**\
✅ **Follow automation-first principles.**\
✅ **Structure code in a way that scales naturally.**

This is your definitive **React + TypeScript Feature-First Style Guide**, designed to **reduce thinking, improve efficiency, and create scalable applications effortlessly.**
