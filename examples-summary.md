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
  - **Inversion of Control:** The _consumer_, not the card component, controls the content and structure of the header and footer sections.
  - **Reusability:** The same `CardFlexible` component can be used for many different card layouts just by changing the provided snippets.

---

## 2. Data Fetching Wrapper Example

This example contrasts an attempt at reusable data fetching using props with a truly flexible approach using snippets.

**Location:**

- Components: `src/lib/components/patterns/*/FetchData*.svelte`
- Helper Component: `src/routes/examples/data-fetching/_UserListComponent.svelte` (for basic example)
- Demo Page: `src/routes/examples/data-fetching/+page.svelte`

### `FetchDataBasic.svelte` (Basic Composition)

- **Pattern:** Composition via Props and Internal Conditional Logic.
- **Implementation:** Centralizes the `fetch` call, loading, and error state management. Renders hardcoded UI for loading/error states. It takes a `displayType` prop (e.g., `'users'`) and internally imports and conditionally renders a specific component (`UserListComponent`) based on that prop's value.
- **Demonstrates:**
  - **Attempted Reusability:** An improvement over duplicating fetch logic entirely.
  - **Rigidity:**
    - Loading/error UI cannot be easily customized by the consumer.
    - Supporting a new display type (e.g., 'posts') requires modifying `FetchDataBasic` to import the new component and add another `{:else if}` block.
  - **Tight Coupling:** The component is tightly coupled to the specific components it imports (`UserListComponent`, etc.).
  - **Type Weakness:** Requires awkward type casting (`data as any`) or linter disabling when passing fetched data to the internally rendered component, as `FetchDataBasic` doesn't inherently know the data type matching `UserListComponent`.
  - **Limited Applicability:** Only works for the predefined `displayType` values it knows about.

### `FetchDataFlexible.svelte` (Flexible Composition)

- **Pattern:** Composition via Snippets (Render Props pattern).
- **Implementation:** Centralizes fetch logic but uses required named snippets (`loading`, `error`, `data`) for all UI rendering. The `error` snippet receives the `Error` object, and the `data` snippet receives the fetched data (with generics `<T>` for type safety).
- **Demonstrates:**
  - **Full UI Control:** The consumer defines the exact markup for the loading state, the error state (using the error details), and the success state (using the fetched data).
  - **Type Safety:** Uses Svelte 5 generics (`<T>`) and typed snippets (`Snippet<[error: Error]>`, `Snippet<[data: T]>`) to ensure the data passed to the snippet matches expectations.
  - **High Reusability:** The same component can be used to fetch and display *any* type of data with completely different UI templates just by changing the snippets and the generic type (`<FetchDataFlexible<User[]>>`, `<FetchDataFlexible<Post[]>>`).
  - **Inversion of Control:** The fetcher handles *fetching*, the consumer handles *rendering*.
  - **Note on Generics Usage:** The demo page (`+page.svelte`) uses `$derived` to reactively select the correctly typed component constructor (`FetchDataFlexible<Post[]>` or `FetchDataFlexible<User[]>`) based on application state, allowing direct rendering (`<TypedFetchData />`) without needing deprecated APIs like `<svelte:component>`.

---

## 3. Animation Wrapper Example

This example shows how flexible composition (using the default `children` snippet) can encapsulate behavior, like triggering animations on scroll.

**Location:**
- Component: `src/lib/components/patterns/flexible-composition/AnimateOnScroll.svelte`
- Demo Page: `src/routes/examples/animation/+page.svelte`

### `AnimateOnScroll.svelte` (Flexible Composition)

- **Pattern:** Composition via `children` Snippet.
- **Implementation:** Uses the Intersection Observer API to detect when its root element enters the viewport. It applies a CSS class (`.is-visible`) which triggers a fade-in-up transition defined in its `<style>` block. It renders whatever content is passed via the `children` snippet inside the observed element.
- **Demonstrates:**
    - **Behavior Encapsulation:** The complex logic of observing intersection and applying animation styles is hidden within the wrapper.
    - **Applying Behavior to Arbitrary Content:** The *same* wrapper can be used to animate simple HTML elements (`<p>`, `<img>`) or other Svelte components (`<CardFlexible>`) without any changes to the wrapper or the wrapped content.
    - **DRY Principle:** Avoids repeating Intersection Observer setup and CSS animation code across multiple components/elements.
    - **Contrast with Basic Approach:** The alternative (no wrapper) would involve manually adding the Observer logic and animation CSS to every element needing the effect, leading to significant code duplication and mixing behavioral logic with presentation.