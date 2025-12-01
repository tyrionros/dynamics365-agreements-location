# Dynamics 365 Field Service Documentation for Facility Management

This repository serves as a comprehensive documentation hub for implementing and understanding Dynamics 365 Field Service in the context of facility management, specifically tailored for scenarios involving rented parking spaces. It provides detailed explanations, step-by-step guides, and alternative approaches to configuring D365 Field Service to meet these needs.

## Key Topics Covered

The documentation is organized into several key areas, each with both an HTML and a Markdown file for easy consumption and in-depth understanding:

1.  **Connecting Agreements to Functional Locations (Standard Approach):**
    *   **Files:** `d365_agreements_functional_locations.html`, `d365_agreements_functional_locations.md`
    *   **Description:** Explains the standard method of linking recurring agreements to physical locations within Field Service, primarily through the use of Customer Assets, for managing parking space rentals.

2.  **Service Zones and Service Tasks with Agreements:**
    *   **Files:** `d365_service_zones_tasks_agreements.html`, `d365_service_zones_tasks_agreements.md`
    *   **Description:** Details how Service Zones and Service Tasks integrate with Field Service Agreements, addressing potential operational considerations and best practices for their use in facility management.

3.  **Logical Names of Dynamics 365 Field Service Tables:**
    *   **Files:** `d365_logical_names.html`, `d365_logical_names.md`
    *   **Description:** Provides a clear definition of the logical names for the core Dynamics 365 Field Service entities (tables) relevant to the documentation, which is essential for developers and integrators.

4.  **Facility Management Without Customer Assets (Alternative Approach):**
    *   **Files:** `d365_agreements_no_customer_assets.html`, `d365_agreements_no_customer_assets.md`
    *   **Description:** Explores an alternative configuration that allows for managing rented parking spaces and automating invoicing (monthly and yearly) using Agreements, Service Zones, and Service Tasks, *without* relying on the Customer Asset table. This section highlights the trade-offs and revised entity roles.

## How to Use This Documentation

Each topic is provided in two formats:

*   `.html` files: For easy viewing in a web browser with basic styling for readability.
*   `.md` files: For in-depth reading, contribution, or conversion to other formats.

To view the HTML files, simply open them in your preferred web browser. The Markdown files can be viewed in any Markdown viewer or editor.

## Project Structure

```
.
├── GEMINI.md
├── README.md
├── d365_agreements_functional_locations.html
├── d365_agreements_functional_locations.md
├── d365_service_zones_tasks_agreements.html
├── d365_service_zones_tasks_agreements.md
├── d365_logical_names.html
├── d365_logical_names.md
├── d365_agreements_no_customer_assets.html
└── d365_agreements_no_customer_assets.md
```

## Contributing and Feedback

If you have suggestions, corrections, or additional insights regarding Dynamics 365 Field Service for facility management, please feel free to contribute by opening issues or submitting pull requests. Your feedback is highly appreciated!
