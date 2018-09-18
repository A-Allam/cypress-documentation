---
title: command:retry
---

The `command:retry` event fires whenever a command begins its retrying routines. This is called on the trailing edge after Cypress has internally waited for the retry interval. When commands fail, the final error is the one that actually bubbles up to fail the test.

This is useful to understand **why** a command is retrying, and generally includes the actual error causing the retry to happen. Essentially, it can be used to debug why Cypress is failing.

# Environment

Event | {% url "Browser" catalog-of-events#Browser-Events %} | {% url "Background Process" background-process %}
--- | --- | ---
{% url `command:retry` command-retry-event %} | {% fa fa-check-circle green %} | {% fa fa-check-circle green %}

# Arguments

**{% fa fa-angle-right %} retry** ***(Object)***

Details of the command retry.

# Usage

## In the browser

In a spec file or support file you can tap into the `command:retry` event.

```javascript
Cypress.on('command:retry', (details) => {
  // details looks something like this:
  // {
  //   assertions: []
  //   error: AssertionError {
  //     message: 'expected \'#login\' to exist in the DOM',
  //     stack: 'AssertionError: expected \'#login\' to exist in the DOM',
  //     displayMessage: 'Expected to find element: \'#login\', but never found it.',
  //     actual: '#login',
  //     expected: '#login'
  //   }
  //   _name: "get"
  //   _runnable: Test {
  //     async: 0,
  //     body: 'function () {\n  cy.visit(\'/test.html\');\n  cy.get(\'#login\');',
  //     title: 'test login',
  //     type: 'test',
  //     ... more properties ...
  //   }
  // }
})
```

## In the background process

Using your {% url "`backgroundFile`" background-process %} you can tap into the `command:retry` event. The retry details look a little different in the background process.

```javascript
module.exports = (on, config) => {
  on('command:retry', (details) => {
    // details looks something like this:
    // {
    //   name: 'get',
    //   error: {
    //     name: 'AssertionError',
    //     message: 'expected \'#login\' to exist in the DOM',
    //     stack: 'AssertionError: expected \'#login\' to exist in the DOM',
    //     displayMessage: 'Expected to find element: \'#login\', but never found it.',
    //     actual: '#login',
    //     expected: '#login'
    //   },
    //   runnable: {
    //     async: 0,
    //     body: 'function () {\n  cy.visit(\'/test.html\');\n  cy.get(\'#login\');',
    //     title: 'test login',
    //     type: 'test',
    //     ... more properties ...
    //   }
    // }
  })
}
```

# See also

- {% url `command:end:event` command-end-event %}
- {% url `command:enqueued:event` command-enqueued-event %}
- {% url `command:start:event` command-start-event %}
