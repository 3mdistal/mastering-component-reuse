# Live Code Examples Summary

This document summarizes the Svelte 5 code examples created to accompany the article on component composition patterns.

## 1. Card Component Example

This example demonstrates the difference between basic (props-based) and flexible (snippet-based) composition for a common UI element like a Card.

**Location:**
- Components: `src/lib/components/patterns/*/Card*.svelte`
- Demo Page: `src/routes/examples/card/+page.svelte`

### `CardBasic.svelte` (Basic Composition)

- **Pattern:** Composition via Props.
- **Implementation:** Defines specific props for optional content sections (`headerText`, `footerTitle`, `footerBody`, `footerActionText`). The internal structure, especially for the footer, is rigidly defined within the component.
- **Demonstrates:**
    - **Rigidity:** The consumer cannot easily change the footer's structure (e.g., add a second button, an icon) without modifying `CardBasic` itself.
    - **Prop Explosion:** Customizing variations requires adding more and more props to the component, making its API complex.
    - **Limited Reusability:** Only suitable for use cases that exactly match its predefined structure.

### `CardFlexible.svelte` (Flexible Composition)

- **Pattern:** Composition via Snippets.
- **Implementation:** Defines optional named snippets (`header`, `footer`) and uses the default `children` snippet. The component provides the outer shell (styling, layout) but delegates the rendering of the header, footer, and body entirely to the consumer.
- **Demonstrates:**
    - **Flexibility:** The consumer provides the exact markup needed for the header and footer directly within the `<CardFlexible>` tags using `{#snippet ...}`.
    - **Inversion of Control:** The *consumer*, not the card component, controls the content and structure of the header and footer sections.
    - **Reusability:** The same `CardFlexible` component can be used for many different card layouts just by changing the provided snippets.

---

## 2. Data Fetching Wrapper Example

This example contrasts an attempt at reusable data fetching using props with a truly flexible approach using snippets.

**Location:**
- Components: `src/lib/components/patterns/*/FetchData*.svelte`
- Helper Component: `src/routes/examples/data-fetching/_UserListComponent.svelte` (for basic example)
- Demo Page: `src/routes/examples/data-fetching/+page.svelte`

### `FetchDataBasic.svelte` (Basic Composition)

- **Pattern:** Composition via Props (centralized logic, but rigid output).
- **Implementation:** Centralizes the `fetch` call, loading, and error state management. However, it renders hardcoded UI for loading/error states and requires a specific `displayComponent` prop. This prop must accept the fetched data via a specific contract (a `data` prop).
- **Demonstrates:**
    - **Attempted Reusability:** An improvement over duplicating fetch logic entirely.
    - **Rigidity:** The loading and error UI cannot be easily customized by the consumer. The consumer *must* provide a component conforming to the expected `data` prop signature.
    - **Type Challenges:** Shows how passing component constructors requires careful type handling or assertions (`displayComponent={UserListComponent as Component<{ data?: unknown }>}`).
    - **Limited Applicability:** Difficult to reuse if the desired loading/error UI differs or if the display component needs data passed differently.

### `FetchDataFlexible.svelte` (Flexible Composition)

- **Pattern:** Composition via Snippets (Render Props pattern).
- **Implementation:** Centralizes fetch logic but uses required named snippets (`loading`, `error`, `data`) for all UI rendering. The `error` snippet receives the `Error` object, and the `data` snippet receives the fetched data (with generics `<T>` for type safety).
- **Demonstrates:**
    - **Full UI Control:** The consumer defines the exact markup for the loading state, the error state (using the error details), and the success state (using the fetched data).
    - **Type Safety:** Uses Svelte 5 generics (`<T>`) and typed snippets (`Snippet<[error: Error]>`, `Snippet<[data: T]>`) to ensure the data passed to the snippet matches expectations.
    - **High Reusability:** The same component can be used to fetch and display *any* type of data with completely different UI templates just by changing the snippets and the generic type (`<FetchDataFlexible<User[]>>`, `<FetchDataFlexible<Post[]>>`).
    - **Inversion of Control:** The fetcher handles *fetching*, the consumer handles *rendering*. 