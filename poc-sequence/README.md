# Sequence Diagrams

Sample UML sequence diagrams showing interaction flows between system components.

## Diagrams

| Diagram | Description | Preview |
|---------|-------------|---------|
| **Payment Flow** | Mobile app to ITMX payment processing sequence | ![Preview](sample-sequence-payment-flow.png) |

## Files

| File | Type |
|------|------|
| [sample-sequence-payment-flow.drawio](./sample-sequence-payment-flow.drawio) | DrawIO Source |
| [sample-sequence-payment-flow.png](./sample-sequence-payment-flow.png) | PNG Export |

## Sequence Diagram Elements

- **Actors** - Users/external systems
- **Lifelines** - Vertical dashed lines representing object lifetime
- **Activations** - Rectangles showing when object is active
- **Messages** - Arrows between lifelines (sync/async)
- **Return Messages** - Dashed arrows for responses
- **Alt/Opt/Loop** - Combined fragments for conditionals

## Standards Applied

- UML actor shape for users
- Synchronous messages: solid arrow with filled head
- Asynchronous messages: solid arrow with open head
- Return messages: dashed arrow
- Shadow on all elements
