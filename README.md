
Feature: Validate Metrics Loading for LOBs and Sub-LOBs

  Scenario: Verify metrics details are displayed for each LOB and Sub-LOB
    Given I open the application
    When I click on each LOB
    Then I should see 5 metric details displayed
    When I click on each Sub-LOB under the LOB
    Then I should see 5 metric details displayed for the Sub-LOB


import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";

Given("I open the application", () => {
  cy.visit("/"); // Replace with the actual URL of your application
});

When("I click on each LOB", () => {
  cy.get(".lob-item") // Replace with actual selector for LOB elements
    .each(($lob) => {
      cy.wrap($lob).click();
      cy.wait(1000); // Wait for the metrics to load
    });
});

Then("I should see 5 metric details displayed", () => {
  cy.get(".metric-container") // Replace with actual selector for metrics
    .should("have.length", 5);
});

When("I click on each Sub-LOB under the LOB", () => {
  cy.get(".lob-item") // Select each LOB
    .each(($lob) => {
      cy.wrap($lob).click();
      cy.wait(1000);

      cy.get(".sub-lob-item") // Select each Sub-LOB within the LOB
        .each(($subLob) => {
          cy.wrap($subLob).click();
          cy.wait(1000);
        });
    });
});

Then("I should see 5 metric details displayed for the Sub-LOB", () => {
  cy.get(".metric-container").should("have.length", 5);
});
