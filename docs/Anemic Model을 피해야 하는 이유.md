---
aliases: 
tags: 
description:
title: Anemic Model을 피해야 하는 이유
created: 2024-12-20T14:30:28
updated: 2024-12-20T14:30:29
---

### 1. **Violation of Encapsulation**

   - **Issue**: Business logic spread across services (instead of being encapsulated within entities) breaks the principle of encapsulation.
   - **Consequence**: Entities become mere data containers, and the logic needed to ensure consistent states becomes scattered. This fragmentation makes understanding and maintaining the codebase difficult.

---

### 2. **Inconsistent States**

   - **Issue**: Without centralized control over state transitions, it's easy to manipulate entities in ways that leave them in invalid or inconsistent states.
   - **Consequence**: For example, if `Product.stockCount` is modified directly from multiple services, there's no guarantee that all operations check for valid business rules, such as preventing negative stock.

---

### 3. **Increased Risk of Bugs**

   - **Issue**: When logic is decentralized, programmers need to remember to apply the same rules and constraints across different services.
   - **Consequence**: This increases the likelihood of mistakes, such as forgetting to check a business rule or applying it inconsistently.

---

### 4. **Poor Code Reuse and DRY Violation**

   - **Issue**: Business logic scattered across the application leads to duplication because each service re-implements the same logic.
   - **Consequence**: When business rules change, developers must update multiple places, increasing the risk of missing some, leading to discrepancies and technical debt.

---

### 5. **Lack of Expressive Models**

   - **Issue**: Anemic models are not expressive; they don’t communicate business logic or domain behavior clearly.
   - **Consequence**: The intent of the code becomes harder to understand. Developers must constantly dive into service implementations to figure out how domain concepts are supposed to behave, increasing cognitive load.

---

### 6. **Weak Domain Boundaries**

   - **Issue**: Without entities enforcing rules, the domain becomes less autonomous and more dependent on external layers like the application or service layer.
   - **Consequence**: This weakens the domain model, making it harder to adapt to changes in business requirements. The domain becomes a passive representation of data, not a true reflection of business processes.

---

### 7. **Harder to Test**

   - **Issue**: With logic in services rather than encapsulated in entities, testing becomes harder because:
     - Tests must mock or stub out multiple dependencies.
     - State validation requires re-testing in every service that handles the entity.
   - **Consequence**: Testing becomes more complex and brittle, discouraging thorough test coverage.

---

### 8. **Difficulty in Maintaining Business Rules**

   - **Issue**: In an anemic model, it's challenging to answer questions like "What are all the rules governing the lifecycle of a Product?"
   - **Consequence**: Business rules are scattered and fragmented, making them harder to locate, understand, and update.

---

### 9. **Tight Coupling to Service Layer**

   - **Issue**: Services handling business logic directly create tight coupling between the domain and service layers.
   - **Consequence**: Refactoring or reusing domain logic in different contexts (e.g., another service or application) becomes difficult.

---

### 10. **Challenges with Scaling Teams**

   - **Issue**: In a large team, an anemic model makes it difficult for developers to understand the domain because the business logic isn’t localized in one place.
   - **Consequence**: Onboarding new team members becomes harder, as they must grasp the scattered rules instead of relying on the domain model to serve as the single source of truth.

---

### 11. **Impacts on Domain-Driven Design (DDD)**

   - **Issue**: Anemic models violate the principles of DDD by treating entities as passive data containers rather than behavior-rich components that enforce business rules.
   - **Consequence**: This undermines the value of DDD, making it harder to model complex business domains effectively.

---

### 12. **Scalability and Performance Issues**

   - **Issue**: Business logic in services can lead to bloated services and a lack of cohesion. Additionally, it may require fetching more data than necessary or making excessive database calls.
   - **Consequence**: This affects application performance and makes scaling harder as services become overloaded with responsibilities.

---

### 13. **Resistance to Change**

   - **Issue**: When logic is fragmented, introducing changes becomes riskier because the impact is harder to trace.
   - **Consequence**: Developers might resist necessary changes due to fear of introducing bugs, leading to stagnation and technical debt.

---

### 14. **Violation of Object-Oriented Design Principles**

   - **Issue**: Anemic models go against core object-oriented principles like:
     - **Encapsulation**: Data and behavior are not grouped.
     - **Cohesion**: Entities lack meaningful responsibilities.
     - **Polymorphism**: Behavior isn’t implemented within entities, reducing opportunities for abstraction.
   - **Consequence**: The design degenerates into a procedural style that doesn’t leverage the strengths of object-oriented programming.

---

### 15. **Inability to Leverage Rich Framework Features**

   - **Issue**: Many modern frameworks and tools are designed to work well with behavior-rich domain models.
   - **Consequence**: Anemic models limit your ability to take advantage of features like lifecycle hooks, automated validation, and ORM capabilities like entity listeners.

---

### Summary

Anemic models hinder code clarity, scalability, maintainability, and adherence to sound design principles. They result in:
- More bugs.
- Harder-to-test and maintain code.
- Fragile systems resistant to change.

By moving toward richer domain models, you create a system that enforces consistency, promotes reusability, and scales better over time, all while making the code more expressive and aligned with business goals.
