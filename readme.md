# Smæshing App - Flowchart 

This file contains Mermaid flowcharts for:
- Customer app (ordering, points, game)
- Staff-only **Compliance & Procedures** module (manager/employee)

---

## 1) Main App Flow (Customer + Staff Gate)
```mermaid
flowchart TD
  A[Open App] --> B{Logged in?}
  B -- No --> C[Onboarding]
  C --> D[SMS Login]
  D --> E[OTP Verification]
  E --> F[User Profile Created]
  B -- Yes --> G[Home Dashboard]
  F --> G

  %% Customer areas
  G --> H[Menu / Monthly Burger]
  G --> I[Game & Points]
  G --> J[Order History]
  G --> K[Profile & Settings]
  G --> L[Support]

  K --> K1[Opening Hours]
  K --> K2[Push Notification Settings]
  K --> K3[Account & Privacy]

  %% Staff-only area
  G --> S{Staff role?}
  S -- No --> G
  S -- Yes --> C0[Compliance Section]
```

---

## 2) Ordering & Pickup Flow (Favrit POS)
```mermaid
flowchart TD
  A[Menu] --> B[Products from API]
  B --> C[Product Details]
  C --> D[Allergen & Filters]
  D --> E[Add to Cart]
  E --> F[Cart]
  F --> G[Pickup Time: ASAP / Scheduled]
  G --> H[Checkout]
  H --> I{Payment Method}
  I -- Vipps --> V[Vipps Payment]
  I -- Card --> K[Card Payment]
  V --> P[Payment Success]
  K --> P

  P --> Q[Create Order in Backend]
  Q --> R[Send Order to Favrit POS]
  R --> S{Accepted by POS?}
  S -- Yes --> T[Order Confirmed + ETA]
  S -- No --> U[Error: Retry / Refund + Support]

  T --> O[Order Status Screen]
  O --> O1[Preparing]
  O1 --> O2[Ready for Pickup]
  O2 --> O3[Completed]

  O3 --> M[Add Points per NOK spent]
```

---

## 3) Points, Game & QR Redeem Flow
```mermaid
flowchart TD
  A[Game & Points] --> B[Points Balance + Total Spend]
  B --> C{Game Unlocked?}
  C -- No --> D[Show requirement: Spend 1000 NOK to unlock]
  C -- Yes --> E{Played Today?}
  E -- Yes --> F[Locked until tomorrow]
  E -- No --> G[Play Daily Game]
  G --> H[Game Result]
  H --> I[Bonus Points / Loot]
  I --> B

  B --> J[Rewards Shop]
  J --> K[Select Reward Item]
  K --> L[Generate QR Redeem Token]
  L --> M[Scan QR at Counter]
  M --> N{Validate token}
  N -- Valid --> O[Mark token used + Deduct points]
  N -- Invalid --> P[Error: Invalid / Expired / Already used]

  %% Earn points from purchases
  Q[In-App Purchase] --> R[Earn points per NOK]
  S[In-Store Purchase] --> T[Identify user via QR / phone]
  T --> R
  R --> B
```

---

## 4) Push Notifications (Monthly Burger + Admin Custom)
```mermaid
flowchart TD
  A[Admin Panel] --> B{Push Type}
  B -- Monthly Burger --> C[Monthly Burger Announcement]
  B -- Custom Message --> D[Custom Push Message]
  C --> E[Send to all users with opt-in]
  D --> F[Send to segment with opt-in]
  E --> G[User opens app]
  F --> G
  G --> H[View Monthly Burger / Message]
```

---

## 5) Compliance & Procedures (Staff-only)

### 5A) Access & Entry (Employee/Manager)
```mermaid
flowchart TD
  A[Open Compliance Section] --> B{Role}
  B -- Employee --> C[Employee Home: My Tasks]
  B -- Manager --> D[Manager Home: Dashboard]
```

---

### 5B) Manager Flow (Create, Schedule, Assign, Monitor, Resolve)
```mermaid
flowchart TD
  A[Manager Dashboard] --> B[Templates]
  B --> B1[Create/Edit Checklist]
  B1 --> B2[Add items and steps]
  B2 --> B3[Set required fields]
  B3 --> C[Scheduling]
  
  C --> C1[Choose cadence]
  C1 --> C2[Set due time + assignees]
  C2 --> D[Publish tasks]

  A --> E[Monitor Status]
  E --> E1[Today: Due / Completed / Overdue]
  E --> E2[History: All runs]
  E --> E3[Deviation Inbox: Alerts]

  E3 --> F{Deviation severity}
  F -- Low --> G[Assign action + deadline]
  F -- Medium --> G
  F -- High --> G
  G --> H[Comment / Attach evidence]
  H --> I[Close deviation]
  I --> E3
```

---

### 5C) Employee Flow (Do tasks, Flag deviations, View history)
```mermaid
flowchart TD
  A[Employee: My Tasks] --> B[Open Task / Checklist Run]
  B --> C[Complete items step-by-step]
  C --> D{Something wrong?}
  D -- No --> E[Submit as Completed]
  D -- Yes --> F[Create Deviation]
  
  F --> F1[Choose severity + description]
  F1 --> F2[Add photo/comment]
  F2 --> G[Submit task with deviation]
  
  E --> H[Task marked completed]
  G --> H

  A --> I[View History]
  I --> J[Past completions + deviations]
```

---

### 5D) End-to-end Compliance Lifecycle (Task Run → Audit Trail)
```mermaid
flowchart TD
  A[Template Created] --> B[Scheduled Task Created]
  B --> C[Employee Completes Run]
  C --> D{Deviation raised?}
  D -- No --> E[Run Completed]
  D -- Yes --> F[Deviation Alert to Manager]
  F --> G[Manager Assigns Fix + Deadline]
  G --> H[Employee/Manager Resolves]
  H --> I[Manager Closes Deviation]
  I --> E
  E --> J[Stored in History + Audit Log]
```
7. **Diagram 5D**: Added missing resolution step before closing deviations
8. **General**: Removed overly complex node labels, fixed syntax consistency, ensured all paths complete properly
