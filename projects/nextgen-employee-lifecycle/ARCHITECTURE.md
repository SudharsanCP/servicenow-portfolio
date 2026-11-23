# Architecture — NextGen Employee Onboarding

```mermaid
flowchart TD
    A[Employee submits New Employee Onboarding] --> B[Service Catalog item<br/>11 variables]
    B --> C[Catalog UI Policies<br/>Conditional field visibility]
    B --> D[Catalog Client Script<br/>Start date must be at least 5 days away]
    D --> E[Flow Designer trigger<br/>Catalog Item Requested]
    E --> F[Get Catalog Variables]
    F --> G[Ask For Approval<br/>Manager]
    G --> H{Approval result}
    H -->|Approved| I[Update RITM Approval = Approved]
    I --> J[Create IT catalog task]
    I --> K[Create HR catalog task]
    I --> L[Create Facilities catalog task]
    H -->|Rejected / Cancelled output| M[Update RITM Approval = Rejected]
    M --> N[No department tasks created]
```

## Notes

- The flow is triggered automatically when the catalog item is requested.
- The flow uses a manager approval step.
- The approved branch creates the IT, HR, and Facilities tasks sequentially.
- The rejected branch updates the Requested Item approval status to Rejected and does not create those tasks.
- This diagram reflects the tested onboarding scope only. Offboarding and external integrations are not implemented.
