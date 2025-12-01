# Facility Management Without Customer Assets: Agreements, Service Zones, and Service Tasks

This document explores an alternative approach to managing rented parking spaces in Dynamics 365 Field Service, focusing on leveraging `Agreements`, `Service Zones`, and `Service Tasks` for automatic invoicing (monthly and yearly), *without* utilizing the `Customer Asset` table.

## Introduction: Deviating from the Standard Model

The standard Dynamics 365 Field Service model often relies heavily on `Customer Assets` to represent specific equipment or locations being serviced, linking them directly to `Functional Locations` and then to `Agreement Booking Setups`. This provides granular asset tracking and service history.

However, for a scenario like rented parking spaces, where the "asset" is largely a fixed, non-repairable location and the primary need is recurring billing and occasional checks, you might consider simplifying the setup by *not* creating a `Customer Asset` for each parking spot.

**Trade-offs of this approach:**

*   **Less Granular Asset History:** You won't have a dedicated `Customer Asset` record to track individual service history directly for each parking spot.
*   **Reliance on Functional Locations and Naming Conventions:** More emphasis will be placed on the `Functional Location` hierarchy and clear naming conventions within `Agreements` to identify specific parking spaces.
*   **Potential Reporting Adjustments:** Some standard Field Service reports might assume `Customer Assets`, requiring custom reports for asset-specific data.

## Key Entities and Their Roles in this Approach

In this alternative model, the following entities take on slightly adjusted or more prominent roles:

*   **Account (<code>account</code>):** Still represents your tenants (customers). Each agreement will be tied to a tenant's account.
*   **Functional Location (<code>msdyn_functionallocation</code>):** This entity becomes crucial for uniquely identifying each individual parking space. Instead of linking a `Customer Asset` to it, the `Functional Location` itself will represent the specific rented spot.
    *   **Example Hierarchy:**
        *   `Main Building Parking Lot` (Functional Location)
        *   `└── Section A` (Functional Location)
            *   `└── Space A-101` (Functional Location - representing the individual spot)
            *   `└── Space A-102` (Functional Location - representing the individual spot)
*   **Agreement (<code>msdyn_agreement</code>):** The central record for each tenant's parking space rental contract. Each agreement will typically correspond to one tenant and one specific parking space (Functional Location). Its name or description should explicitly identify the parking space.
*   **Service Zone (<code>territory</code>):** Used for geographical grouping of work if you have technicians assigned to specific areas of your facility. It will be associated with the `Account` or `Agreement`.
*   **Incident Type (<code>msdyn_incidenttype</code>) & Service Task (<code>msdyn_incidenttypeservicetask</code>):** These are still used if you need to define recurring *work* (e.g., a monthly visual check of the parking space). If the agreement is purely for billing, the Incident Type might be very simple or even optional if you primarily use `Agreement Invoice Setups`.
*   **Agreement Booking Setup (<code>msdyn_agreementbookingsetup</code>):** Used to generate recurring `Work Orders`. If you have recurring physical checks of the parking space, you would use this, linking it to an `Incident Type`.
*   **Agreement Invoice Setup (<code>msdyn_agreementinvoicesetup</code>):** **This entity is critical for automatic billing.** You will use this to define the recurring charges (monthly rent, yearly fees) and their schedules.

## Step-by-Step Guide: Managing Parking Spaces Without Customer Assets

Here's how you would set up Dynamics 365 Field Service for this approach:

1.  **Define Your Detailed Functional Location Hierarchy:**
    *   Navigate to **Field Service > Assets > Functional Locations**.
    *   Create a hierarchy where the lowest level `Functional Location` *uniquely identifies each rentable parking space*.
        *   *Example:*
            *   Parent: `Building A Parking Lot`
            *   Child: `Level 1`
            *   Grandchild: `Space L1-001` (This `Functional Location` now represents the physical parking spot.)
    *   **Key:** Ensure each such `Functional Location` has a clear, identifiable name.

    <div class="illustration-box" style="border: 1px solid #ccc; padding: 15px; margin: 20px 0; background-color: #f9f9f9; border-radius: 5px;">
        <p><strong>Illustration: Detailed Functional Location Hierarchy</strong></p>
        <code>Main Building Parking Lot (Functional Location)</code><br>
        <code>└── Section A (Functional Location)</code><br>
        <code>    └── Space A-101 (Functional Location - Individual Parking Spot)</code><br>
        <code>    └── Space A-102 (Functional Location - Individual Parking Spot)</code>
    </div>

2.  **Create / Identify the Customer Account:**
    *   Ensure you have an <span class="entity">Account</span> record in Dynamics 365 for each tenant.

3.  **Create the Agreement (per Tenant, per Parking Space):**
    *   Navigate to **Field Service > Agreements > Agreements**.
    *   Create a new <span class="entity">Agreement</span> record for each tenant's rental of a specific parking space.
    *   Set the **"Service Account"** to the tenant's account.
    *   **Crucial:** Use the **"Name"** and/or **"Description"** fields to clearly state which parking space (Functional Location) this agreement pertains to (e.g., "Agreement: Tenant X - Parking Space A-101 Rental"). This acts as your direct identifier.
    *   Define the "Start Date" and "End Date" for the rental period.

4.  **Set Up Service Zones (if applicable):**
    *   Assign the appropriate <span class="entity">Service Zone</span> to the tenant's <span class="entity">Account</span>, or directly to the <span class="entity">Agreement</span> if the zone differs. This helps with technician dispatch if work orders are generated.

5.  **Optional: Agreement Booking Setup for Recurring Work (e.g., Inspections):**
    *   If you need to generate Work Orders for physical inspections or maintenance of the parking space:
        *   Create a new <span class="entity">Incident Type</span> (e.g., "Parking Space Visual Check") and associate relevant <span class="entity">Service Tasks</span> (e.g., "Check for litter," "Verify markings").
        *   Add a new <span class="entity">Agreement Booking Setup</span> to the Agreement.
        *   Set the "Incident Type" to your newly created "Parking Space Visual Check".
        *   Define the "Booking Recurrence" (e.g., "Monthly").
        *   **Important:** On the <span class="entity">Agreement Booking Setup</span>, you can set the **"Primary Incident Functional Location"** field to the specific `Functional Location` representing the parking space (e.g., "Space A-101"). This directly links the generated Work Order to the parking space's functional location.

    <div class="illustration-box" style="border: 1px solid #ccc; padding: 15px; margin: 20px 0; background-color: #f9f9f9; border-radius: 5px;">
        <p><strong>Illustration: Agreement Booking Setup linking to Functional Location</strong></p>
        <p><strong>Agreement:</strong> Tenant X Corp - Parking Space A-101 Rental</p>
        <p><strong>Agreement Booking Setup:</strong> Monthly Visual Check - Space A-101</p>
        <ul>
            <li>Recurrence: Monthly</li>
            <li>Incident Type: Parking Space Visual Check</li>
            <li><strong>Primary Incident Functional Location: "Space A-101"</strong></li>
        </ul>
    </div>

6.  **Mandatory: Agreement Invoice Setup for Automatic Invoicing:**
    *   This is the core for generating invoices.
    *   Add new <span class="entity">Agreement Invoice Setups</span> to the Agreement. You'll likely need separate ones for monthly and yearly charges.
    *   For **Monthly Rent:**
        *   Create an `Agreement Invoice Setup` (e.g., "Monthly Parking Rent").
        *   Add an `Agreement Invoice Product` with a product like "Parking Space Rental - Monthly" and the corresponding price.
        *   Define the "Invoice Recurrence" as "Monthly".
    *   For **Yearly Administrative Fee (or similar):**
        *   Create another `Agreement Invoice Setup` (e.g., "Yearly Admin Fee").
        *   Add an `Agreement Invoice Product` with a product like "Annual Admin Fee" and its price.
        *   Define the "Invoice Recurrence" as "Yearly".

    <div class="illustration-box" style="border: 1px solid #ccc; padding: 15px; margin: 20px 0; background-color: #f9f9f9; border-radius: 5px;">
        <p><strong>Illustration: Agreement Invoice Setups for Monthly and Yearly Billing</strong></p>
        <p><strong>Agreement:</strong> Tenant X Corp - Parking Space A-101 Rental</p>
        <p><strong>Agreement Invoice Setup 1:</strong> Monthly Parking Rent</p>
        <ul>
            <li>Invoice Recurrence: Monthly</li>
            <li>Agreement Invoice Products: "Parking Space Rental - Monthly" ($100)</li>
        </ul>
        <p><strong>Agreement Invoice Setup 2:</strong> Yearly Admin Fee</p>
        <ul>
            <li>Invoice Recurrence: Yearly</li>
            <li>Agreement Invoice Products: "Annual Admin Fee" ($50)</li>
        </ul>
    </div>

7.  **Activate the Agreement:**
    *   Once all booking (if any) and invoice setups are configured, change the <span class="entity">Agreement</span>'s "System Status" to "Active". This will trigger the automatic generation of Work Orders (if Booking Setups exist) and Invoices according to their defined recurrences.

## How it All Connects (Without Customer Assets)

<div class="illustration-box" style="border: 1px solid #ccc; padding: 15px; margin: 20px 0; background-color: #f9f9f9; border-radius: 5px;">
    <p><strong>Flow: Agreement to Functional Location (Direct/Implicit) for Billing/Work</strong></p>
    <p><span class="entity">Account (Tenant)</span> <span class="action">--has--></span> <span class="entity">Agreement</span></p>
    <p><span class="entity">Agreement</span> <span class="action">--is for (named/described as)--></span> <span class="entity">Functional Location (Space A-101)</span></p>
    <p>    ↓</p>
    <p><span class="entity">Agreement Invoice Setup</span> <span class="action">--generates invoices for--></span> <span class="entity">Agreement</span></p>
    <p>    ↓ (if physical work is involved)</p>
    <p><span class="entity">Agreement Booking Setup</span> <span class="action">--generates Work Orders for--></span> <span class="entity">Functional Location (Space A-101)</span></p>
    <p>    ↓</p>
    <p><span class="entity">Work Order</span> <span class="action">--has primary Incident Functional Location--></span> <span class="entity">Functional Location (Space A-101)</span></p>
</div>

## Conclusion

This approach allows you to manage rented parking spaces and automate invoicing without using `Customer Assets`. It relies heavily on a well-structured `Functional Location` hierarchy to represent individual parking spots and clear naming conventions within your `Agreements`. While it simplifies asset tracking, it shifts the responsibility for associating services and billing to the `Agreement` and its relationship with a specific `Functional Location`. Ensure your reporting needs are met with this model, possibly requiring custom reports to aggregate data by `Functional Location` rather than `Customer Asset`.
