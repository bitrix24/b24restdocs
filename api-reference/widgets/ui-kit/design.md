# Principles of the Bitrix24 UI Kit Design

The Bitrix24 UI Kit is built on the idea of **interface affinity**: any application using the UI Kit should look and feel like a continuation of the Bitrix24 platform, rather than an external service embedded via iframe.

Key principles underlying the library:

## 1. Consistency

Interface patterns should be recognizable and predictable for Bitrix24 users. This means:

- uniform margins, sizes, and grids,
- repeated interaction logic,
- unified colors and typography.

Users should feel "at home," regardless of which section of the platform they are in.

## 2. Adaptability and Scalability

Components function correctly on different screens—from laptops to embedded panels in mobile applications.

- Components use fluid sizes.
- Adaptability is set declaratively through Tailwind utility classes, without manual media queries.
- There is an option to define adaptive behavior directly in the template.

## 3. Inclusivity and Accessibility

All components are developed with basic accessibility requirements in mind:

- proper HTML semantics,
- support for keyboard navigation,
- clear focus and aria attributes,
- compatibility with dark mode.

## 4. Simplicity

The simpler the interface, the less cognitive load on the user, and the easier it is for the developer to assemble a ready screen:

- minimal unnecessary information,
- logical hierarchy,
- reasonable defaults and "out-of-the-box" behavior.

## 5. Component-Based

Everything in the UI Kit is built as a set of independent Vue components that can be used individually or assembled into templates.

- Clear isolation.
- Extensibility through props/slots.
- Combinability based on the Lego principle.

## 6. Brand Identity

The UI Kit adheres to the visual language of Bitrix24—colors, icons, button shapes, notification logic, and more align with the style of the main platform.

This is not just a library; it is an extension of the Bitrix24 interface in your applications.