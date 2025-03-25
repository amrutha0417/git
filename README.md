
Feature: Validate Metrics Loading for LOBs and Sub-LOBs

  Scenario: Verify metrics details are displayed for each LOB and Sub-LOB
    Given I open the application
    When I click on each LOB in the navigation menu
    Then I should see 5 metric details displayed on the main page
    When I click on each Sub-LOB under a LOB
    Then I should see 5 metric details displayed for the selected Sub-LOB





import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";

Given("I open the application", () => {
  cy.visit("/"); // Replace with the actual application URL
});

When("I click on each LOB in the navigation menu", () => {
  cy.get("nav ul > li.lob-item") // Target LOBs inside nav > ul
    .each(($lob) => {
      cy.wrap($lob).click();
      cy.wait(1000); // Wait for the metrics to load

      // Verify metrics after clicking LOB
      cy.get(".metric-container") // Update with actual selector
        .should("have.length", 5);
    });
});

Then("I should see 5 metric details displayed on the main page", () => {
  cy.get(".metric-container") // Update with actual selector
    .should("have.length", 5);
});

When("I click on each Sub-LOB under a LOB", () => {
  cy.get("nav ul > li.lob-item") // Iterate over each LOB
    .each(($lob) => {
      cy.wrap($lob).click();
      cy.wait(1000); // Wait for the sub-LOBs to load

      // Click each sub-LOB inside the current LOB
      cy.wrap($lob)
        .find("ul > li.sub-lob-item") // Target sub-LOBs inside current LOB
        .each(($subLob) => {
          cy.wrap($subLob).click();
          cy.wait(1000); // Wait for metrics to load
          
          // Verify metrics after clicking sub-LOB
          cy.get(".metric-container")
            .should("have.length", 5);
        });
    });
});

Then("I should see 5 metric details displayed for the selected Sub-LOB", () => {
  cy.get(".metric-container").should("have.length", 5);
});

