# Tactical Design in DDD

While [Strategic Design](/strategic-design/) focuses on the big picture of your system, Tactical Design deals with the implementation details within a single [bounded context](/strategic-design/bounded-contexts). It provides patterns and building blocks for creating an expressive, rich domain model.

<div class="ddd-architecture-container">
  <img src="/your-repo-name/ddd-architecture.svg" alt="DDD Architecture Overview - Tactical Design Highlighted" class="ddd-architecture-diagram" />
</div>

## What is Tactical Design?

Tactical Design offers specific modeling tools to express your domain model in code. It provides patterns for:

- Modeling domain concepts
- Encapsulating business rules
- Ensuring domain logic integrity
- Creating clear, maintainable code

These patterns help translate the domain knowledge captured during strategic design into a working software model.

## Key Building Blocks of Tactical Design

### Entities

[Entities](/tactical-design/entities) are objects defined by their identity, not their attributes. They have a lifecycle and are tracked through state changes over time. Examples include a Customer, Order, or Account.

### Value Objects

[Value Objects](/tactical-design/value-objects) are immutable objects defined by their attributes, with no conceptual identity. Examples include Money, Address, or Date Range.

### Aggregates

[Aggregates](/tactical-design/aggregates) are clusters of entities and value objects with clear boundaries, treated as a single unit for data changes. They help manage complexity and enforce consistency.

### Domain Events

[Domain Events](/tactical-design/domain-events) represent something significant that happened in the domain. They capture state changes and enable loose coupling between parts of the system.

### Repositories

[Repositories](/tactical-design/repositories) provide a way to obtain and persist aggregates, abstracting the underlying data storage mechanism.

### Domain Services

[Domain Services](/tactical-design/domain-services) encapsulate domain logic that doesn't naturally fit within entities or value objects, typically involving multiple domain objects.

### Factories

[Factories](/tactical-design/factories) encapsulate the complex creation logic of entities and aggregates, ensuring they're created in a valid state.

## Tactical Design in Action

Here's a simplified example showing some tactical design patterns working together:

```typescript
// Entity
class Order {
  private readonly id: OrderId;
  private items: OrderItem[] = [];
  private status: OrderStatus;
  
  constructor(id: OrderId, customerId: CustomerId) {
    this.id = id;
    this.status = OrderStatus.Created;
    
    // Domain Event published when order is created
    DomainEvents.publish(new OrderCreatedEvent(id, customerId));
  }
  
  // Behavior that enforces business rules
  addItem(product: Product, quantity: number): void {
    if (this.status !== OrderStatus.Created) {
      throw new OrderException("Cannot add items to a confirmed order");
    }
    
    const item = new OrderItem(product.id, product.price, quantity);
    this.items.push(item);
  }
  
  confirm(): void {
    if (this.items.length === 0) {
      throw new OrderException("Cannot confirm an empty order");
    }
    
    this.status = OrderStatus.Confirmed;
    
    // Another domain event published
    DomainEvents.publish(new OrderConfirmedEvent(this.id));
  }
  
  total(): Money {
    return this.items.reduce(
      (sum, item) => sum.add(item.subTotal()), 
      Money.zero()
    );
  }
}

// Repository interface
interface OrderRepository {
  findById(id: OrderId): Order;
  save(order: Order): void;
}

// Factory
class OrderFactory {
  static createOrder(customerId: CustomerId): Order {
    const orderId = OrderId.generate();
    return new Order(orderId, customerId);
  }
}

// Application Service using the domain model
class OrderApplicationService {
  constructor(
    private orderRepository: OrderRepository,
    private productRepository: ProductRepository
  ) {}
  
  placeOrder(command: PlaceOrderCommand): OrderId {
    // Use factory to create a valid order
    const order = OrderFactory.createOrder(command.customerId);
    
    // Add items from command
    for (const item of command.items) {
      const product = this.productRepository.findById(item.productId);
      order.addItem(product, item.quantity);
    }
    
    // Confirm the order, triggering business rule validation
    order.confirm();
    
    // Persist the new order
    this.orderRepository.save(order);
    
    // Return the ID of the new order
    return order.id;
  }
}
```

## Benefits of Tactical Design

- **Domain Alignment**: Code structure closely mirrors domain concepts
- **Encapsulation**: Business rules are protected inside the appropriate objects
- **Expressiveness**: Code clearly communicates domain intent
- **Maintainability**: Changes to business rules are localized
- **Testability**: Domain logic can be tested in isolation
- **Flexibility**: Separation of concerns allows parts to evolve independently

## Tactical vs. Strategic Design

|                  | Strategic Design | Tactical Design |
|------------------|-----------------|-----------------|
| **Scope**        | System-wide     | Within a single bounded context |
| **Focus**        | Problem space   | Solution space  |
| **Participants** | Domain experts and technical team | Primarily technical team |
| **Output**       | Bounded contexts, context maps    | Code models and structures |
| **Timeframe**    | Early in project | Throughout development |

## Getting Started with Tactical Design

1. Begin with [Strategic Design](/strategic-design/) to understand your problem domain
2. Identify the key entities and value objects in your bounded context
3. Determine aggregate boundaries based on business invariants
4. Implement repositories for data access
5. Use domain services for operations that don't belong to entities
6. Identify and publish domain events for significant state changes
7. Refactor continuously as your domain understanding evolves

The following sections will explore each of these tactical patterns in depth, with examples and best practices for implementing them effectively. 