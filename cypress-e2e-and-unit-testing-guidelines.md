# Cypress E2E and Unit Testing Guidelines

This documentation provides guidelines for standardizing Cypress End-to-End (E2E) testing with Vue.js. The provided configuration and commands help ensure consistency and efficiency in test writing practices.

***

## Writing Tests

Create test files under the `cypress/e2e` directory. Use the custom commands to interact with elements and perform actions.

### Guidelines for Test File Structure

{% stepper %}
{% step %}
### File Naming

Use PascalCase for file names with the extension `.cy.spec.ts`, placed under the `cypress/e2e` directory. The file name should match the scope or page being tested.

Example: When testing the Home page, the file name should be `HomePage.cy.spec.ts`.
{% endstep %}

{% step %}
### Top-Level Describe

Each spec file should only contain one top-level `describe`, which represents the page or component being tested. Additional `describe` blocks can be used to group similar actions on the same page.
{% endstep %}

{% step %}
### Component Accessibility

Write code that is easy to test by adding custom data attributes:

* Use `data-wv-name` for the root element of a component.
* Use `data-wv-section` for each section within a component.
{% endstep %}
{% endstepper %}

### Example Test: `Example.cy.spec.ts`

{% code title="Example.cy.spec.ts" %}
```typescript
Copydescribe('Example Test', () => {
  beforeEach(() => {
    cy.login();
    cy.visit('/');
  });

  it('Should load the main section', () => {
    cy.getSection('main').should('exist');
  });

  it('Should find an element by name', () => {
    cy.getByName('submit-button').click();
    cy.get('body').should('contain', 'Submission successful');
  });

  it('Should find an element by custom data attribute', () => {
    cy.getByData('role', 'admin').should('exist');
  });
});
```
{% endcode %}

### Unit Tests

For unit tests, place files under the `cypress/unit` directory. Use the same file name as the code file being tested, following PascalCase for consistency.

Example: For `formatName.util.ts`, the test file should be named `FormatNameUtil.cy.spec.ts`.

Updated onDecember8, 2025, 8:34 AMUTC

***

https://fewangsit.hashnode.space/docs/vue-typescript-coding-style-guide

https://fewangsit.hashnode.space/docs/panduan-development-untuk-proyek-supply
