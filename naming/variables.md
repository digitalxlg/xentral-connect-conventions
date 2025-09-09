# Variable Naming Conventions

This document outlines the conventions for naming variables in our Xentral-Connect projects. Adhering to these rules ensures consistency, readability, and maintainability across all our integration flows.

## Core Principles

1.  **Language**: All variable names must be in **English**. This ensures universal understanding within the development team.

2.  **Case Style**: Use **`snake_case`** for all variable names. This means all letters are lowercase and words are separated by an underscore (`_`).

3.  **Clarity over Brevity**: Variable names should be descriptive and unambiguous. Avoid overly short or cryptic names. It should be possible to understand the purpose of a variable from its name alone.

    ```
    // GOOD
    customer_shipping_address = "123 Main St"
    order_items_count = 5
    is_ready_for_dispatch = true

    // AVOID
    addr = "123 Main St" // Too short, unclear what kind of address
    num = 5 // Not descriptive
    rdy = true // Cryptic
    ```

## Standard Variable Names

To maintain consistency across all integrations, we use standardized names for common entities and data structures.

### Entity Naming

* **Single Entity**: When a variable holds a single object or entity, use the singular form of the entity's name.

    ```
    // Represents a single customer object
    customer = { "id": 123, "name": "Example Corp" }

    // Holds the details of one product
    product_details = { "sku": "ART-001", "price": 99.99 }
    ```

* **Collections of Entities**: When a variable holds a list, an array, or any collection of entities, always use the plural form.

    ```
    // A list of all orders
    orders = [ { "id": "A" }, { "id": "B" } ]

    // A collection of product variants
    product_variants = [ "red", "blue", "green" ]
    ```

### API and Data Transfer Objects (DTOs)

* **API Request Body**: The variable containing the data object to be sent via a CRUD command (e.g., POST, PUT, PATCH) to an API **must** be named `body`.

    ```
    // Data for creating a new customer
    body = {
        "firstname": "Max",
        "lastname": "Mustermann",
        "email": "max.mustermann@example.com"
    }
    // This 'body' variable is then mapped to the request body in the API call step.
    ```

* **API Response Data**: The variable holding the main data extracted from an API response should be named `response_<entity>`.

    ```
    // The content from an API call is stored here
    response_salesorder = { "status": "success", "order_id": 456 }
    
    // Accessing a specific field
    order_status = response_data.status
    ```

### Mapped Data Structures

* **Mapping Output**: A variable that holds the result of a mapping or transformation step **must** be prefixed with `mapped_<entity>`. This makes it clear that the data structure is no longer in its original source format.

    ```
    // An incoming order object from a shop system
    shop_order = { "orderId": "SHOP-1002", "customer": { "firstName": "Jane" } }
    
    // After mapping to our internal format, the variable is prefixed
    mapped_order = { "id": "SHOP-1002", "customer_firstname": "Jane" }
    ```

### Primary Keys and Identifiers

The distinction between a standalone variable holding a key and the key *within* an object is crucial.

* **Standalone Primary Key Variable**: A standalone variable that holds the value of a primary key for a lookup or reference **must** be named `primary`.

    ```
    // A standalone variable holding the ID for a lookup
    primary = 12345
    // This variable would then be used to fetch the full customer object, e.g., get_customer_by(primary)
    ```

* **Primary Key within a Data Structure**: The key that serves as the primary unique identifier *within* an object or data structure (representing a table row) **must** be named `id`.

    ```
    // An entity object structure with its primary key named 'id'
    order = {
        "id": "ORD-2023-001",
        "customer_id": 12345,
        "total_amount": 99.99
    }
    ```

* **Foreign Keys**: For foreign keys that reference the `id` of another entity, use the format `[entity_name]_id`.

    ```
    order_item = {
        "id": 987,
        "order_id": "ORD-2023-001", // Foreign key to the order entity's 'id'
        "product_id": "ART-XYZ",     // Foreign key to the product entity's 'id'
        "quantity": 2
    }
    ```

## Data Type Specific Conventions

For clarity, it is strongly recommended to use specific prefixes or suffixes for certain data types.

* **Booleans**: Prefix boolean variables with `is_`, `has_`, or `can_`.
* **Counts and Quantities**: Use the suffix `_count` or `_quantity`.
* **Timestamps and Dates**: Use the suffix `_at` (for date & time) or `_date` (for date only).

## Summary Table

| Category | Convention | Example |
| :--- | :--- | :--- |
| **General** | English, `snake_case` | `customer_first_name` |
| **API Request Body** | `body` | `body = { "key": "value" }` |
| **Mapped Data** | `mapped_<structure>` | `mapped_customer`, `mapped_invoice` |
| **Standalone Primary Key** | `primary` | `primary = 123` |
| **Primary Key in Object** | `id` | `customer = { "id": 123, ... }` |
| **Foreign Key** | `[entity_name]_id` | `customer_id`, `order_id` |
| **Collections** | Plural form | `orders`, `products` |
| **Booleans** | `is_`, `has_`, `can_` prefix | `is_enabled`, `has_attachment` |
| **Counts** | `_count` suffix | `items_count`, `errors_count` |
| **Timestamps** | `_at` suffix | `created_at`, `processed_at` |
| **Dates** | `_date` suffix | `shipping_date` |