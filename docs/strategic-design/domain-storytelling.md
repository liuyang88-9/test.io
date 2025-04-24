# Domain Storytelling

Domain Storytelling is a collaborative approach to understanding and communicating about domain knowledge through visual stories created with domain experts.

## What is Domain Storytelling?

Domain Storytelling is a workshop format where domain experts tell stories about their work while a moderator documents these stories using a pictographic language. These visual stories help:

- Develop a shared understanding of the domain
- Identify the core domain and subdomains
- Establish a [ubiquitous language](/strategic-design/ubiquitous-language)
- Define boundaries for [bounded contexts](/strategic-design/bounded-contexts)
- Discover domain events and business processes

## The Domain Storytelling Process

1. **Prepare**: Identify stakeholders and scope
2. **Gather**: Bring together domain experts and technical team members
3. **Narrate**: Domain experts tell concrete stories about their work
4. **Visualize**: Moderator captures the story in real-time using pictographs
5. **Validate**: Participants review and refine the visual story
6. **Analyze**: Identify patterns, concepts, and boundaries
7. **Document**: Preserve the stories for future reference

## Visual Language

Domain Storytelling uses a simple pictographic language with four basic elements:

1. **Actors** (represented as stick figures): People, systems, or organizations that perform activities
2. **Work Objects** (represented as icons): Things actors work with, like documents, products, or information
3. **Activities** (represented as arrows): Actions that actors perform on work objects
4. **Annotations** (represented as numbers and notes): Sequence of activities and additional information

## Example Domain Story

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│    Customer                  Order System             Warehouse  │
│       ┌┐                        ┌┐                       ┌┐      │
│       └┘                        └┘                       └┘      │
│        │ 1. places              │                        │      │
│        │ ┌─────────┐            │                        │      │
│        └─►  Order  │            │                        │      │
│          └────┬────┘            │                        │      │
│               │                 │                        │      │
│               │ 2. receives     │                        │      │
│               └────────────────►│                        │      │
│                                 │                        │      │
│                                 │  3. creates            │      │
│                                 │  ┌──────────┐          │      │
│                                 └─►│Shipment  │          │      │
│                                    └─────┬────┘          │      │
│                                          │               │      │
│                                          │ 4. sends to   │      │
│                                          └──────────────►│      │
│                                                          │      │
└──────────────────────────────────────────────────────────────────┘
```

This simple domain story shows a customer placing an order, which is received by an order system that creates a shipment and sends it to a warehouse.

## When to Use Domain Storytelling

Domain Storytelling is particularly valuable when:

- Starting a new project or feature
- Onboarding new team members
- Resolving misunderstandings between teams
- Exploring edge cases and exceptions
- Identifying integration points between systems
- Discovering domain events for event storming

## Benefits of Domain Storytelling

- **Concrete examples** instead of abstract discussions
- **Visual representation** that's accessible to all stakeholders
- **Common understanding** across business and technical teams
- **Early discovery** of misconceptions and gaps in knowledge
- **Foundation for** identifying domain concepts and boundaries
- **Efficient knowledge transfer** from domain experts to development teams

## Domain Storytelling vs. Other Techniques

| Technique | Focus | When to Use |
|-----------|-------|-------------|
| Domain Storytelling | Concrete day-to-day workflows | Early domain exploration |
| Event Storming | Domain events and commands | Business process modeling |
| User Story Mapping | User journeys and features | Product planning |
| Process Modeling (BPMN) | Detailed process flows | Formal process documentation |
| Use Cases | Actor interactions with system | System requirements |

## From Stories to Domain Model

Domain stories provide valuable input for domain modeling:

1. **Actors** → Users, external systems, or context boundaries
2. **Work Objects** → Entities, value objects, or aggregates
3. **Activities** → Domain services, methods, or commands
4. **Sequence** → Process flows or domain events
5. **Annotations** → Business rules or requirements

## Example: From Story to Code

A domain story showing a customer adding items to a shopping cart might translate to:

```typescript
// Actors become classes or interfaces
interface Customer {
  id: string;
  name: string;
}

// Work objects become entities or value objects
class ShoppingCart {
  items: Array<CartItem> = [];
  
  // Activities become methods
  addItem(product: Product, quantity: number): void {
    const item = new CartItem(product, quantity);
    this.items.push(item);
  }
  
  calculateTotal(): Money {
    // Logic derived from the story
  }
}
```

## Best Practices

- **Focus on concrete examples**, not abstract processes
- **Limit scope** to 15-20 activities per story
- **Use the domain expert's language**
- **Let domain experts lead** the storytelling
- **Capture variations** and exceptions as separate stories
- **Document assumptions and questions**
- **Revisit stories** as domain understanding evolves

## Tools for Domain Storytelling

- **Whiteboard and sticky notes**: Simple, accessible to everyone
- **Domain Story Modeler**: Open-source tool for creating domain stories
- **Drawing tools**: Generic tools like Draw.io or Miro
- **Specialized DDD tools**: Tools that support domain modeling workflows

## Integrating with Other DDD Practices

Domain Storytelling works well with other DDD practices:

- Use stories to identify candidates for [bounded contexts](/strategic-design/bounded-contexts)
- Extract key terms for your [ubiquitous language](/strategic-design/ubiquitous-language)
- Identify relationships for [context mapping](/strategic-design/context-mapping)
- Discover domain events for event storming
- Identify core domains and supporting domains

## Real-World Example

### Insurance Claim Processing

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                                                                                │
│  Policyholder           Claims Portal           Adjuster          Payment Sys  │
│     ┌┐                     ┌┐                     ┌┐                 ┌┐        │
│     └┘                     └┘                     └┘                 └┘        │
│      │                      │                      │                  │        │
│      │ 1. submits           │                      │                  │        │
│      │ ┌────────┐           │                      │                  │        │
│      └─►  Claim │           │                      │                  │        │
│        └───┬────┘           │                      │                  │        │
│            │                │                      │                  │        │
│            │ 2. records     │                      │                  │        │
│            └───────────────►│                      │                  │        │
│                             │                      │                  │        │
│                             │ 3. assigns           │                  │        │
│                             │ ┌────────┐           │                  │        │
│                             └─►  Claim │           │                  │        │
│                               └───┬────┘           │                  │        │
│                                   │                │                  │        │
│                                   │ 4. reviews     │                  │        │
│                                   └───────────────►│                  │        │
│                                                    │                  │        │
│                                                    │ 5. approves      │        │
│                                                    │ ┌────────┐       │        │
│                                                    └─►  Claim │       │        │
│                                                      └───┬────┘       │        │
│                                                          │            │        │
│                                                          │ 6. sends   │        │
│                                                          └────────────►        │
│                                                                       │        │
│                                                                       │7.issues│
│                                                                       │┌──────┐│
│                                                                       └►Payment││
│                                                                         └──────┘│
└────────────────────────────────────────────────────────────────────────────────┘
```

From this story, we can identify:

- **Bounded Contexts**: Claims Management, Payment Processing
- **Entities**: Claim, Payment
- **Events**: Claim Submitted, Claim Assigned, Claim Approved, Payment Issued
- **Business Rules**: Claims must be reviewed and approved before payment

Domain Storytelling provides a foundation for deeper domain understanding and helps bridge the gap between business knowledge and software design. 