# [Feature Name]

> Brief one-liner: what this feature does and who it's for.
> e.g. "Allows merchants to view and filter their transaction history."

## Table of Contents

- [Keywords](#keywords)
- [Assumptions](#assumptions)
- [Introduction](#introduction)
- [User Interaction and Design](#user-interaction-and-design)
- [Technical](#technical)

## Keywords

Define any abbreviations, domain terms, or project-specific jargon used in this document.

| **Word** | **Description** |
|----------|-----------------|
| **[Term]** | [What it means in this context] |

## Assumptions

List things that are assumed to be true **before** work begins. These are not requirements — they are pre-conditions or constraints that the team is relying on.

- The user is already authenticated.
- The relevant backend endpoint exists and returns the expected shape.
- [Add any other assumptions specific to this feature]

## Introduction

A short paragraph (2–5 sentences) explaining **what** this feature is and **why** it exists. Avoid implementation detail here — focus on the goal and the user problem it solves.

## User Interaction and Design

This section is written for QA testers and anyone who needs to understand how the feature works without needing to know the technical details. It covers every screen the user sees, what they can do on it, and what happens next — supported by screenshots and examples.

### [Screen Name]

> What is this screen? One sentence describing its purpose.
> e.g. "This is the Transaction History screen, where the user can see a list of all their past transactions."

The screen is laid out in two columns — the **Description** column walks through what the user sees and does, and the **Screenshot** column shows the matching screen.

| **Description** | **Screenshot** |
|-----------------|----------------|
| Walk through what the user sees and does on this screen, step by step:<br><br>1. The user arrives here from [previous screen or action — e.g. tapping "View History" on the Dashboard].<br>2. They see [describe the main content — e.g. a list of transactions, most recent first].<br>3. They can [describe available actions — e.g. scroll the list, tap a row for details, or use the filter button at the top].<br>4. Tapping [action] takes the user to [next screen] — see [Next Screen Name](#next-screen-name) below. | ![Screen Name Screenshot](./assets/screen-name.png) |

> If this journey continues into another feature, link to that document:
> Continued in [Feature Name](../feature-name/index.md)

#### Error & Edge Case States

Document every non-happy-path state the user might encounter on this screen. One row per state; include a screenshot for each where possible. Remove any row that does not apply.

| **State** | **Description** | **Screenshot** |
|-----------|-----------------|----------------|
| **Loading** | Describe what the user sees while the screen is waiting for data (e.g. "A full-screen shimmer placeholder is shown until the data loads."). | ![Loading state screenshot](./assets/screen-name-loading.png) |
| **Empty** | Describe what the user sees when there is no data to display (e.g. "A message reads 'No transactions yet' with a prompt to make their first payment."). | ![Empty state screenshot](./assets/screen-name-empty.png) |
| **Error** | Describe what the user sees when something goes wrong (e.g. "An inline error banner reads 'Something went wrong. Please try again.' with a Retry button."). List any distinct error cases if they show different messages or behaviour. | ![Error state screenshot](./assets/screen-name-error.png) |

## Technical

The following section expands on the technical implementation.

> The code blocks below are illustrative **pseudocode** — every project uses its own language.
> Replace them with the real code, in the real language, including whatever in-code
> documentation convention that language uses (e.g. KDoc for Kotlin, Javadoc for Java,
> docstrings for Python, JSDoc for JavaScript/TypeScript, XML doc comments for C#).

### Library Implementation

List any **third-party libraries** introduced or heavily used for this feature. Add one sub-section per library.

#### [Library Name]

[Library URL](https://example.com)

Brief description of what the library is and what it does. This can be copied from the library's own documentation or written in your own words.

###  Common

List each shared utility, constant, or data model used by this feature and link directly to its section in the common documentation. Do not duplicate the code here — the source of truth lives in the common docs.

- [ExampleService](../../common/index.md#exampleservice)
- [ExampleConstants](../../common/index.md#exampleconstants)
- [ExampleDto / Example / ExampleRepository](../../common/index.md#examplerepository)

### Screens

Overview of every screen that belongs to this feature. Use one sub-section per screen (see template below). Duplicate the sub-section for each screen.

### Shared Widgets / Partials

UI components that are **reused across multiple screens** within this feature (e.g. a shared loading state, a shared error banner, a common list item).

- `TransactionListItem` — row widget used on both the history and search screens


#### 1. [Screen Name]

> e.g. `TransactionHistoryScreen`

##### Screen Description

What does this screen show and what is its purpose? Include the entry point (where the user comes from) and possible exit points (where they can go next).

##### API Interaction

One sub-section per endpoint this screen calls. The heading links to the API documentation (external or internal). The method links to the data model used for the request/response in the Data / Networking section below.

###### [GET /v1/example](https://api-docs.example.com/v1/example)

**Method:** [`GET`](#data--networking)

**Trigger:** This endpoint is called when the screen loads — the user does not need to take any action.

**Description:** Briefly describe what this call fetches or submits and what the user sees as a result (e.g. "Fetches the list of transactions for the current user. The list is displayed as soon as the response is received; a loading indicator is shown in the meantime.").

---

###### [POST /v1/example](https://api-docs.example.com/v1/example)

**Method:** [`POST`](#data--networking)

**Trigger:** This endpoint is called when the user taps the [Button Name] button.

**Description:** Briefly describe what this call does and what the user sees as a result (e.g. "Submits the user's input. On success the user is navigated to the confirmation screen; on failure an error message is shown inline.").

##### Utility / Service

Screen-specific helpers or services used only by this screen. Give each interface its own heading so it can be linked to from other docs.

###### ExampleScreenService

```
interface ExampleScreenService

    // Validates the user's input before submission.
    // input: the raw string entered by the user
    // returns: true if the input meets all requirements, false otherwise
    validateInput(input) -> Boolean
```

##### Screen Arguments

Document every argument this screen receives when it is opened, and anything it returns when it closes. Paste the argument type or parameter keys as they appear in code.

###### ExampleScreenArgs

```
ExampleScreenArgs:
    // The ID of the record to display.
    id: String
    // The filter type pre-selected when the screen opens. Null means no filter applied.
    filterType: String?
```

| **Argument** | **Type** | **Required** | **Description** |
|--------------|----------|:------------:|-----------------|
| `id` | `String` | Yes | ID of the record to display |
| `filterType` | `String?` | No | Pre-selected filter; `null` means no filter applied |

**Returns:** [Describe anything passed back to the calling screen, or "Nothing — this screen does not return a result."]

---

##### Data / Networking

Screen-specific models or repository calls not already covered in Common. Give each class or interface its own heading so it can be linked to from other docs.

###### ExampleScreenState

```
ExampleScreenState:
    isLoading: Boolean
    items: List<Example>
    // Non-null when an error should be shown to the user.
    errorMessage: String?
```

##### Constants

Screen-specific keys or values. Give each constants group its own heading so it can be linked to from other docs.

###### ExampleScreenConstants

```
ExampleScreenConstants:

    // Key for the record ID passed into this screen.
    ARG_ID = "arg_id"
```
