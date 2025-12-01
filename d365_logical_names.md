# Logical Names of Dynamics 365 Field Service Tables in Documentation

This document defines the logical names of the key Dynamics 365 Field Service tables (entities) discussed in the previously generated HTML and Markdown documentation related to agreements, functional locations, service zones, and service tasks. Understanding these logical names is crucial for development, integration, and advanced reporting within Dynamics 365.

## Core Entities and Their Logical Names

Here are the entities involved and their corresponding logical names, which are typically used when referencing these tables programmatically (e.g., in FetchXML, Web API calls, or custom code):

*   **Account**
    *   **Logical Name:** `account`
    *   **Description:** Represents a customer, company, or organization. In our scenario, this would be your tenants who are renting parking spaces. Agreements are always associated with an Account.

*   **Agreement**
    *   **Logical Name:** `msdyn_agreement`
    *   **Description:** Defines recurring work or services for a customer over a specified period. This entity automates the creation of Work Orders and Invoices.

*   **Agreement Booking Setup**
    *   **Logical Name:** `msdyn_agreementbookingsetup`
    *   **Description:** A child record of an Agreement, it specifies the details of the recurring work to be performed. This includes the frequency, incident type, and importantly, the Customer Asset it pertains to.

*   **Customer Asset**
    *   **Logical Name:** `msdyn_customerasset`
    *   **Description:** Represents a piece of equipment, property, or item that a customer owns or rents, for which services are provided. In your scenario, each individual parking space is a Customer Asset.

*   **Functional Location**
    *   **Logical Name:** `msdyn_functionallocation`
    *   **Description:** Represents a spatial position where work can be performed or equipment can be installed. It helps organize physical spaces hierarchically (e.g., parking lot > section > level).

*   **Work Order**
    *   **Logical Name:** `msdyn_workorder`
    *   **Description:** Represents a task or set of tasks to be performed by a field technician. Work Orders are generated from Agreement Booking Setups.

*   **Incident Type**
    *   **Logical Name:** `msdyn_incidenttype`
    *   **Description:** A template that defines a common problem or type of work. It specifies required Service Tasks, products, and services for a particular job.

*   **Service Task**
    *   **Logical Name:** `msdyn_servicetask` (Note: Service Tasks are typically associated *with* an Incident Type; the `msdyn_servicetask` entity often refers to the *definition* of a task, which is then linked to an Incident Type via `msdyn_incidenttypeservicetask` or directly as `msdyn_workorderservicetask` on a Work Order.)
    *   **More precisely, when defining tasks within an Incident Type, you'd be looking at the `Incident Type Service Task` entity:**
        *   **Logical Name:** `msdyn_incidenttypeservicetask`
        *   **Description:** Links a specific Service Task definition (`msdyn_servicetask`) to an Incident Type (`msdyn_incidenttype`), defining the steps for that type of work.

*   **Service Zone**
    *   **Logical Name:** `territory` (This is the underlying entity used for Service Zones in Field Service, although it might be surfaced as "Service Zone" in the UI).
    *   **Description:** Represents a geographical area or logical grouping used for organizing resources and dispatching work.

*   **Bookable Resource**
    *   **Logical Name:** `bookableresource`
    *   **Description:** Represents a technician, equipment, or facility that can be scheduled for work. (Implicitly involved in scheduling work orders that use service zones).

*   **Booking Status**
    *   **Logical Name:** `bookingstatus`
    *   **Description:** Defines the status of a Bookable Resource Booking (e.g., Scheduled, In Progress, Completed). (Relevant to Work Order scheduling).

---

This list covers the primary entities that are central to managing agreements, assets, locations, and services within Dynamics 365 Field Service, as they relate to your facility management scenario. Understanding these logical names is crucial for anyone working with the underlying data model of Dynamics 365.
