# [Feature Name]

> Brief one-liner: what this is and what problem it solves.
> e.g. "Shared formatting utilities used across the transaction and payment features."

## Table of Contents

- [Keywords](#keywords)
- [Assumptions](#assumptions)
- [Introduction](#introduction)
- [Technical](#technical)

## Keywords

Define any abbreviations, domain terms, or project-specific jargon used in this document.

| **Word** | **Description** |
|----------|-----------------|
| **CR** | "Content Ready" — the backend API layer of Phoenix |
| **[Term]** | [What it means in this context] |

## Assumptions

List things that are assumed to be true **before** work begins. These are not requirements — they are pre-conditions or constraints that the team is relying on.

- [Add assumptions specific to this component]

## Introduction

A short paragraph (2–5 sentences) explaining **what** this component is and **why** it exists. Focus on the problem it solves and who consumes it — avoid implementation detail.

## Technical

> **Keep this in sync with the code.** The snippets below are copied from the actual interfaces and models — not the implementations. If a method signature, property, or constant changes in code, update it here too.
>
> The code blocks below are illustrative **pseudocode** — every project uses its own language.
> Replace them with the real code, in the real language, including whatever in-code
> documentation convention that language uses (e.g. KDoc for Kotlin, Javadoc for Java,
> docstrings for Python, JSDoc for JavaScript/TypeScript, XML doc comments for C#).

### Utility / Service

Paste / write the **interface** (not the implementation) for each service. Give each interface its own heading so it can be linked to from feature screen docs and other features.

#### ExampleService

```
interface ExampleService

    // Returns a formatted display string for the given amount and currency code.
    // amount: the raw amount in the smallest currency unit (e.g. cents)
    // currencyCode: ISO 4217 currency code (e.g. "USD")
    // returns: a localised, human-readable amount string (e.g. "$ 12.50")
    formatAmount(amount, currencyCode) -> String

    // Checks whether the current user session is still valid.
    // returns: true if the session is active, false if it has expired
    isSessionActive() -> Boolean
```

### Constants

Paste / write the constants exactly as they appear in code, grouped however your language groups constants (a module, a static/companion class, an enum, a namespace, etc.). Give each group its own heading so it can be linked to from feature screen docs and other features.

#### ExampleConstants

```
ExampleConstants:

    // Maximum number of items to request per page from the API.
    PAGE_SIZE = 20

    // Key used to pass the selected filter type between screens.
    ARG_FILTER_TYPE = "arg_filter_type"
```

### Data / Networking

Paste / write the data models (DTOs and domain models) and the repository **interface**. Give each type or interface its own heading so it can be linked to from feature screen docs and other features.

#### ExampleDto

```
ExampleDto:
    id: String
    amount: Long
    currencyCode: String
    // ISO 8601 timestamp of when the record was created.
    createdAt: String
```

#### Example

```
// Domain model used throughout the app after mapping from ExampleDto.
Example:
    id: String
    amount: Long
    currencyCode: String
    createdAt: Long
```

#### ExampleRepository

```
interface ExampleRepository

    // Returns a paginated stream of results for the current user.
    // page: zero-based page index
    // returns: the list of Example items for the requested page
    getExamples(page) -> Stream<List<Example>>
```