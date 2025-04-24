# Context Mapping

Context Mapping is a strategic DDD pattern used to define and understand the relationships between different [bounded contexts](/strategic-design/bounded-contexts) in a complex system.

## Why Context Mapping Matters

No bounded context exists in isolation. Understanding how they interact helps:

- Identify dependencies and potential points of failure
- Plan integration strategies between teams and systems
- Establish expectations and responsibilities 
- Navigate organizational politics and realities
- Make explicit decisions about coupling and autonomy

## Context Map Types

Context maps document the relationships between bounded contexts. These relationships can take several forms:

### Partnership (Cooperative)

Two teams have a close, symbiotic relationship with synchronized planning and development.

```
┌─────────────┐     ┌─────────────┐
│ Bounded     │◄►───┤ Bounded     │
│ Context A   │     │ Context B   │
└─────────────┘     └─────────────┘
```

- **Pros**: Tight coordination, aligned goals
- **Cons**: Higher coupling, potential bottlenecks
- **When to use**: For critical integrations with high interdependence

### Shared Kernel

Multiple bounded contexts share a subset of models which are kept in sync.

```
┌─────────────┐     ┌─────────────┐
│ Bounded     │     │ Bounded     │
│ Context A   │     │ Context B   │
│          ┌──┼─────┼──┐          │
│          │  │     │  │          │
│          │ Shared  │          │
│          │ Kernel  │          │
└──────────┴──┘     └┴─────────────┘
```

- **Pros**: Reduces duplication, ensures consistency
- **Cons**: Creates coupling, requires coordination
- **When to use**: When multiple teams need to share critical domain concepts

### Customer-Supplier

One context (the supplier) provides services to another context (the customer).

```
┌─────────────┐     ┌─────────────┐
│ Customer    │     │ Supplier    │
│ Context     │◄────┤ Context     │
└─────────────┘     └─────────────┘
```

- **Pros**: Clear responsibilities, defined interfaces
- **Cons**: Power dynamics, potential priority conflicts
- **When to use**: When one team depends on another's output

### Conformist

Similar to customer-supplier, but the downstream context has no influence over the upstream context and must conform to its model.

```
┌─────────────┐     ┌─────────────┐
│ Upstream    │     │ Downstream  │
│ Context     │────►│ Context     │
└─────────────┘     └─────────────┘
```

- **Pros**: Simplicity, reduced translation effort
- **Cons**: May force non-ideal models on downstream
- **When to use**: When integrating with systems you can't influence

### Anticorruption Layer (ACL)

The downstream context creates a layer to translate between its model and the upstream model, protecting its own model from corruption.

```
┌─────────────┐     ┌───────┐     ┌─────────────┐
│ Upstream    │     │  ACL  │     │ Downstream  │
│ Context     │────►│       │────►│ Context     │
└─────────────┘     └───────┘     └─────────────┘
```

- **Pros**: Strong isolation, model integrity
- **Cons**: Extra development effort, potential performance impact
- **When to use**: When protecting your model from external influence is critical, or when integrating with legacy systems

### Open Host Service / Published Language

Defines a protocol or language that provides access to the bounded context as a set of services, typically via an API.

```
┌─────────────┐     ┌───────┐     ┌─────────────┐
│ Bounded     │     │ API/  │     │ External    │
│ Context     │────►│ Pub.  │────►│ Consumers   │
└─────────────┘     └───────┘     └─────────────┘
```

- **Pros**: Clear API, supports multiple consumers
- **Cons**: Maintenance overhead, versioning challenges
- **When to use**: When your context needs to serve multiple other contexts

### Separate Ways

A decision to not integrate at all, avoiding dependencies between contexts.

```
┌─────────────┐     ┌─────────────┐
│ Bounded     │     │ Bounded     │
│ Context A   │     │ Context B   │
└─────────────┘     └─────────────┘
```

- **Pros**: Full autonomy, no coordination required
- **Cons**: Potential duplication, inconsistency
- **When to use**: When integration costs outweigh benefits

## Creating a Context Map

1. **Identify the bounded contexts** in your system
2. **Map the relationships** between them using the patterns above
3. **Document team ownership** for each context
4. **Identify integration mechanisms** (APIs, events, files, etc.)
5. **Note critical dependencies** and potential risks
6. **Review and refine** with all stakeholders

## Example Context Map

Here's a simplified context map for an e-commerce system:

```
                 ┌────────────────┐
                 │   Product      │
                 │   Catalog      │
                 │   (Team A)     │
                 └───────┬────────┘
                         │
                         │ Published Language
                         ▼
┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│   Inventory    │◄───►│   Order        │     │   Customer     │
│   Management   │     │   Management   │◄───►│   Management   │
│   (Team B)     │     │   (Team C)     │     │   (Team D)     │
└────────────────┘     └───────┬────────┘     └────────────────┘
                               │
                               │ Customer-Supplier
                               ▼
                       ┌────────────────┐
                       │   Payment      │
                       │   Processing   │
                       │   (Team E)     │
                       └────────────────┘
                               │
                               │ Anticorruption Layer
                               ▼
                       ┌────────────────┐
                       │   Legacy       │
                       │   Accounting   │
                       │   (Team F)     │
                       └────────────────┘
```

## Implementation Strategies

### Communication Methods

Different integration patterns may use different communication methods:

- **Synchronous APIs** - direct calls between services
- **Asynchronous messaging** - events and message queues
- **Batch processing** - files or bulk updates
- **Shared databases** - common data storage (use with caution)

### Translation Approaches

When models differ between contexts:

- **Direct mapping** - simple field-to-field translation
- **Transformation services** - complex translation logic
- **Adapters** - convert between different interfaces
- **Event translators** - convert domain events between formats

## Best Practices

- **Make the context map visual** and easily accessible
- **Keep it updated** as systems and relationships evolve
- **Use it for planning** new features and integrations
- **Review it periodically** with all stakeholders
- **Consider organizational factors**, not just technical ones
- **Identify problem areas** where relationship types may cause friction
- **Start simple** and add detail as needed

## Real-World Considerations

- **Legacy systems** often require anticorruption layers
- **Team dynamics** influence context relationships
- **Political boundaries** may determine what's possible
- **Existing investments** might dictate some relationships
- **Regulatory requirements** may force certain integration patterns

Context mapping acknowledges that integration between bounded contexts is as much about team relationships and organizational dynamics as it is about technical integration. 