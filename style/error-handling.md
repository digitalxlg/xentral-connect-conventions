# Error Handling & Logging Conventions

This document outlines the standardized approach to error handling, status reporting, and logging within our Xentral-Connect workflows. Adhering to these conventions is critical for maintaining transparent, debuggable, and robust integrations.



## Case Identifier

The **Case Identifier** is a unique reference to the specific data record being processed. Its primary purpose is to allow for quick lookups and traceability from a workflow run back to the source data in the connected systems.

**Convention:**
The identifier should be the most human-readable unique key of the resource.

-   **Primary Choice:** A document number (e.g., `documentNumber`, `number`).
-   **Secondary Choice:** The primary key of the entity (e.g., `id`).

**Examples:**
-   For an order: `ORD-2023-5821`
-   For a product: `PROD-001274`
-   For a customer without a number: `cst_8a3f4b2c1d`



## Content Identifier (Final Workflow Status)

The **Content Identifier** provides an at-a-glance summary of the final outcome of a workflow run. It consists of a status tag followed by a concise explanation in English.

### Status Tags & Colors

There are three possible final status tags, which directly correlate to the visual status (color) of the workflow run.

1.  **`[Success]` - Green Status**
    This indicates that the workflow completed all its intended actions successfully.
    -   `[Success]: Created new customer in Xentral`
    -   `[Success]: Updated stock for 5 products`

2.  **`[Abort]` - Green Status**
    This indicates a **controlled exit**. The workflow stopped intentionally because a predefined condition was not met. This is not an error, but a valid business logic path. The status is intentionally green because no manual intervention is required.
    -   `[Abort]: Order already exists in Xentral`
    -   `[Abort]: Could not find matching country code`
    -   `[Abort]: Product is marked as inactive, skipping update`

3.  **`[Error]` - Red Status**
    This indicates an **uncontrolled failure**. The workflow stopped unexpectedly due to a technical issue, an API failure, a logic bug, or invalid data that was not caught by a controlled check. A red status always requires investigation.
    -   `[Error]: Xentral API returned status 500`
    -   `[Error]: Failed to parse JSON response`
    -   `[Error]: Division by zero in price calculation`




## In-Workflow Logging

Log messages created *within* the flow of a workflow are used for debugging and tracing the execution path.

**Convention:**
Log messages should be prefixed with `[INFO]:`. They should be used to output the state of important variables or confirm the completion of a specific step.

**Examples:**
-   `[INFO]: Starting product sync for SKU: ABC-123`
-   `[INFO]: Quantity before calculation: 5`
-   `[INFO]: Quantity after calculation: 3`
-   `[INFO]: Found matching customer with ID: 4711`



## Example Workflow Run

Here is how these conventions would look in the context of a single workflow execution.

-   **Workflow Name:** `Order | Shopify -> Xentral | Single`
-   **Case Identifier:** `SH-1099`

**Logs:**
-   `[INFO]: Customer found with Xentral ID: 2055.`
-   `[INFO]: Order contains 3 line items.`
-   `[INFO]: Order already exists. Aborting process.`

**Final Outcome:**
-   **Status Color:** Green
-   **Content Identifier:** `[Abort]: Order already exists in Xentral`

## Summary Table

| Category | Convention | Purpose | Example |
| :--- | :--- | :--- | :--- |
| **Case Identifier** | `documentNumber` or `id` | Traceability of the processed record. | `ORD-2023-5821` |
| **Successful End** | `[Success]: <description>` | Confirms successful completion. (Green) | `[Success]: Updated Product` |
| **Controlled Exit** | `[Abort]: <description>` | Confirms a planned, safe exit. (Green) | `[Abort]: Product inactive`|
| **Uncontrolled Error** | `[Error]: <description>` | Signals an unexpected failure. (Red) | `[Error]: API unavailable`|
| **Internal Log** | `[INFO]: <description>` | Provides debug info during the run. | `[INFO]: Stock is 25` |