# Bounded Contexts

A Bounded Context is a central pattern in Domain-Driven Design (DDD) that explicitly defines the boundaries within which a particular domain model applies.

## Understanding Bounded Contexts

In any complex system, attempting to create a single unified model for the entire domain often leads to:

- Inconsistent terminology and concepts
- Bloated models that are hard to maintain
- Difficulty in coordinating between teams
- Confusion about where specific logic should reside

Bounded contexts address these issues by dividing the larger system into smaller, more cohesive parts with:

- Clear boundaries
- Explicit interfaces for communication with other contexts
- Consistent internal language and models

## Identifying Bounded Contexts

Bounded contexts often align with:

- **Business capabilities** or subdomains
- **Organizational structures** (following Conway's Law)
- **Technical constraints** or requirements
- **Legacy system boundaries**

### Signs That You Need Separate Bounded Contexts

- A term has different meanings in different parts of the organization
- Different teams have responsibility for different parts of the system
- Different parts of the system have different rates of change
- Different parts of the system have different technical requirements

## Examples of Bounded Contexts

In an e-commerce application, you might have bounded contexts for:

- **Product Catalog**
  - Models products, categories, attributes
  - Focuses on search and browsing optimization
  
- **Order Management**
  - Models orders, line items, shipping
  - Focuses on transaction consistency
  
- **Customer Management**
  - Models customers, accounts, preferences
  - Focuses on customer relationships

- **Inventory**
  - Models stock levels, warehouses, availability
  - Focuses on inventory accuracy

## Implementing Bounded Contexts

Bounded contexts can be implemented in various ways:

- **Separate microservices** - each bounded context as its own service
- **Separate modules** within a monolith
- **Separate schemas** in a database
- **Separate codebases** or repositories

What matters most is that the boundaries are explicit and respected.

## Code Example

Here's a simplified example showing how two bounded contexts might model the same real-world concept differently:

```typescript
// Product Catalog Bounded Context
// Focus on product information and search
namespace ProductCatalog {
  class Product {
    id: string;
    name: string;
    description: string;
    price: Money;
    categories: Category[];
    attributes: ProductAttribute[];
    images: ProductImage[];
    // Methods for search, browsing, etc.
  }
}

// Order Management Bounded Context
// Focus on orders and pricing
namespace OrderManagement {
  class Product {
    id: string;
    name: string;
    price: Money;
    discount: Discount;
    taxRate: TaxRate;
    
    // Methods for pricing, availability, etc.
    calculateFinalPrice(): Money {
      // Apply discounts, taxes, etc.
    }
  }
}
```

Notice how each context models a "Product" differently according to its specific needs.

## Best Practices

- **Start with business subdomains** as candidate bounded contexts
- **Listen for language changes** as indicators of context boundaries
- **Document the contexts and their boundaries** explicitly
- **Define integration points** between contexts carefully
- **Avoid sharing databases** between contexts when possible
- **Assign clear ownership** of each bounded context to a team

## Challenges

- **Finding the right size** for bounded contexts
- **Managing dependencies** between contexts
- **Handling duplication** versus coupling trade-offs
- **Legacy system integration** with bounded contexts

## Related Concepts

- [Context Mapping](/strategic-design/context-mapping) - how bounded contexts relate to each other
- [Ubiquitous Language](/strategic-design/ubiquitous-language) - the shared language within a bounded context
- [Core Domain](/strategic-design/domain-storytelling) - identifying which bounded contexts deserve the most investment 