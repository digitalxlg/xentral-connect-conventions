Markdown

# Resource Naming Conventions

This document defines the naming conventions for all resources created within the Xentral-Connect platform. A consistent naming scheme is crucial for quickly identifying, understanding, and debugging our integration flows. The pipe symbol (`|`) is used as a primary separator to structure the names logically.

## Workflows

Workflows are the core logic of our integrations. Their name should clearly communicate their purpose and context. We differentiate between four types of workflows.

### Logic Workflows
These workflows contain the main business logic for data transformation and synchronization.

**Pattern:** `<Entity> | <Source> -> <Target> | <Single or All>`

* `<Entity>`: The primary data entity being processed (e.g., `Product`, `Order`, `Customer`).
* `<Source> -> <Target>`: The systems between which the data flows.
* `<Single or All>`: Specifies if the workflow processes a single record or a batch of records.

**Examples**
* Product | Connect -> Xentral | Single
* Salesorder | Shopify -> Connect | All
* Stock | Connect -> Shopify | All


### Trigger Workflows
These workflows do not contain logic themselves but initiate a logic workflow. They are triggered by events, webhooks, or schedules.

**Pattern:** `<Type> | <Context> | <Detail>`

* `<Type>`: The trigger type (`Event`, `Hook`, `Cronjob`).
* `<Context>`: For events, `Export` or `Import`. For cronjobs, the execution interval (e.g., `1h`, `15min`, `daily`).
* `<Detail>`: For events, the specific event name (e.g., `product.created`). For cronjobs, a short description of the job.

**Examples**

* Event | Export | product.created
* Hook | Import | Order Paid
* Cronjob | 1h | Sync Stocklevels
* Cronjob | daily | Generate Report


### Helper Workflows
These are reusable sub-workflows that perform a specific, isolated task and return a value.

**Pattern:** `<Short Description> | return { <output> }`

* `<Short Description>`: A concise, action-oriented description of what the helper does.
* `<output>`: A brief representation of the returned data structure.

**Examples**

* Customer Validation | return { is_valid, reason }
* Calculate Shipping Cost | return { price, currency }
* Format Address | return { formatted_address }


### Configuration Workflow
There is one central workflow to hold global configuration values.

**Name:** `Config | Global Settings & Variables`

---

## Data Structures
These define the schema for entities within our project.

**Pattern:** `digitalXL | <entity>`

**Examples**

* digitalXL | product
* digitalXL | customer
* digitalXL | order


---

## Datasets
Datasets are used to store collections of data, often as temporary storage or as the result of a query.

**Pattern:** `digitalXL | <source> | <shortDescription>`

**Examples**

* digitalXL | Xentral | Open Orders
* digitalXL | Shopify | Unfulfilled Line Items
* digitalXL | temp | Customers To Update


---

## Mappings
Mappings define the transformation rules from one data structure to another.

**Pattern:** `digitalXL | <source_data_structure> -> <target_data_structure>`

**Examples**

* digitalXL | shopify_order -> digitalXL_order
* digitalXL | digitalXL_order -> xentral_auftrag


---

## Text Templates
These are used for generating dynamic content, like API responses or notifications.

**Pattern:** `digitalXL | <usecase>`

**Examples**

* digitalXL | Galaxus Status Response
* digitalXL | Order Confirmation Email Body
* digitalXL | Error Notification Slack


## Summary Table

| Resource Type | Pattern / Convention | Example |
| :--- | :--- | :--- |
| **Logic Workflow** | `<Entity> \| <Source> -> <Target> \| <Single/All>` | `Product \| Connect -> Xentral \| Single` |
| **Trigger Workflow** | `<Type> \| <Context> \| <Detail>` | `Cronjob \| 15min \| Sync Prices` |
| **Helper Workflow** | `<Description> \| return { <output> }` | `Customer Validation \| return { is_valid }` |
| **Data Structure** | `digitalXL \| <entity>` | `digitalXL \| product` |
| **Dataset** | `digitalXL \| <source> \| <description>` | `digitalXL \| Xentral \| Open Orders` |
| **Mapping** | `digitalXL \| <source> -> <target>` | `digitalXL \| shopify_prod -> digitalXL_prod` |
| **Text Template** | `digitalXL \| <usecase>` | `digitalXL \| Galaxus Response` |