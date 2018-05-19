# Html & Primitives

A-Frame provides a handful of elements such as `<a-box>` or `<a-sky>` called *primitives* that wrap the entity-component pattern to make it appealing for beginners



# Entity-Component-System(ECS)

ECS architecture is a common and desirable pattern in 3D and game development that follows the **composition over inheritance and hierarchy** principle.

The benefits of ECS include:

1. Greater flexibility when defining objects by mixing and matching reusable parts.
2. Eliminates the problems of long inheritance chains with complex interwoven functionality.
3. Promotes clean design via decoupling, encapsulation, modularization, reusability.
4. Most scalable way to build a VR application in terms of complexity.
5. Proven architecture for 3D and VR development.
6. Allows for extending new features (possibly sharing them as community components)

## ECS in A-Frame

- **Entities** are represented by the `<a-entity>` element and prototype.
- **Components** are represented by HTML attributes on `<a-entity>`‘s. Underneath, components are objects containing a schema, lifecycle handlers, and methods. Components are registered via the `AFRAME.registerComponent (name, definition)` API.
- **Systems** are represented by `<a-scene>`‘s HTML attributes. System are similar to components in definition. Systems are registered via the `AFRAME.registerSystem (name, definition)` API.



# Position

1. right hand system
2. length unit: meter
3. angle unit: degree