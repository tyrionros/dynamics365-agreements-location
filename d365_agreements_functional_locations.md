# Documentation: Connecting Dynamics 365 Field Service Agreements to Functional Locations

This Markdown document provides an in-depth explanation of the accompanying HTML file, `d365_agreements_functional_locations.html`. The HTML document is designed to illustrate and guide users through the process of linking Dynamics 365 Field Service agreements to functional locations, particularly in a facility management context like managing rented parking spaces.

## Purpose of the HTML Documentation

The primary goal of `d365_agreements_functional_locations.html` is to demystify the process of associating recurring service agreements with specific physical locations in Dynamics 365 Field Service. It addresses a common scenario in facility management where services (e.g., parking space rentals) need to be tied to defined spaces.

## Key Concepts Explained

The HTML document breaks down the process by first defining crucial Dynamics 365 Field Service entities:

*   **Functional Locations:** These represent hierarchical physical spaces. The documentation clarifies how an entire parking lot can be a top-level functional location, with sections or levels as sub-locations. It uses an illustration to show this hierarchy, for example:
    *   `Main Building Parking Lot (Functional Location)`
    *   `└── Section A (Functional Location)`
        *   `└── Space A-101 (Customer Asset)`
*   **Agreements:** Explained as the mechanism for defining recurring work for a customer over a set period. In the context of parking, this would be the rental agreement with a tenant.
*   **Customer Assets:** These represent the actual items or spaces being serviced. The document emphasizes that each individual parking space should be a Customer Asset, as it acts as the bridge between the Agreement and the Functional Location.

## Step-by-Step Connection Guide

The HTML provides a clear, ordered guide to establish the connection:

1.  **Create/Identify the Customer Account:** Ensures the tenant (customer) is set up in D365.
2.  **Define Functional Location Hierarchy:** Guides on setting up the physical layout of the parking facility using Functional Locations.
3.  **Create/Link the Customer Asset (Parking Space):** This critical step details how to create each parking space as a Customer Asset and, most importantly, **link it directly to its corresponding Functional Location**. An illustration demonstrates this linkage.
4.  **Create the Agreement:** Steps for setting up the recurring service agreement with the tenant.
5.  **Add Agreement Booking Setup to the Agreement:** Explains how to define the recurring work (e.g., monthly inspection or rental charge) within the agreement. The key here is to **associate the Agreement Booking Setup with the specific Customer Asset (parking space)**. This creates the indirect link from the Agreement to the Functional Location.
6.  **Activate the Agreement:** The final step to make the agreement live and start generating work orders/invoices.

## Illustrations and Their Meaning

The HTML documentation incorporates `illustration-box` elements to visually represent the relationships between entities and the hierarchical structure:

*   **Functional Location Hierarchy:** Shows how broader areas (parking lots) contain smaller, defined sections.
*   **Customer Asset linked to Functional Location:** Clearly depicts that a parking space (Customer Asset) resides within a specific section (Functional Location).
*   **Agreement Booking Setup linking to Customer Asset:** Illustrates how the recurring service definition points to the individual parking space.
*   **Flow: Agreement to Functional Location:** A summary diagram showcasing the indirect path: `Agreement -> Agreement Booking Setup -> Customer Asset -> Functional Location`. This explicitly notes that the Customer Asset is the crucial "bridge."

## Additional Context and Considerations

The documentation emphasizes that while agreements don't *directly* link to functional locations, the Customer Asset serves as the intermediary. This is a vital point for understanding the data model in Dynamics 365 Field Service. Work Orders generated from the agreement will inherit the Customer Asset, thereby carrying the associated Functional Location information.

The HTML also suggests that while Agreement Service Assets can be linked to Functional Locations, the recommended best practice for organizational clarity is to first associate assets with their functional locations.

This comprehensive guide, presented in HTML with clear illustrations and reinforced by this Markdown explanation, aims to provide a thorough understanding for facility managers using Dynamics 365 Field Service.
