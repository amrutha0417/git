Feature: Validate LOB Hierarchy and Metric Details

  Scenario: Verify each LOB loads the corresponding metric details
    Given I navigate to the application
    When I click on each LOB
    Then I should see the corresponding metric details

  Scenario: Verify each LOB contains sub-LOBs
    Given I navigate to the application
    When I expand each LOB
    Then I should see the list of sub-LOBs
import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";

Given("I navigate to the application", () => {
  cy.visit("your-application-url"); // Replace with the actual URL
});

When("I click on each LOB", () => {
  cy.get("ul > li").each(($lob, index) => {
    cy.wrap($lob).click();
    cy.wait(1000); // Wait for metric details to load (replace with proper assertion)

    // Verify that metric details change dynamically
    cy.get(".metric-container") // Adjust the selector based on the actual metric container
      .should("be.visible")
      .and("contain.text", `Expected Metric for LOB ${index + 1}`); // Adjust based on dynamic data
  });
});

Then("I should see the corresponding metric details", () => {
  cy.get(".metric-container").should("be.visible");
});

When("I expand each LOB", () => {
  cy.get("ul > li").each(($lob) => {
    cy.wrap($lob).click();
    cy.wait(500); // Small wait for sub-LOBs to load
  });
});

Then("I should see the list of sub-LOBs", () => {
  cy.get("ul > li").each(($lob) => {
    cy.wrap($lob)
      .find("ul > li") // Sub-LOBs inside LOB
      .should("have.length.greaterThan", 0); // Ensure sub-LOBs exist
  });
});
