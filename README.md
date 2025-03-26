
Feature: LOB Navigation and Metrics Verification

  Scenario: Navigate to LOB and verify metrics
    Given The user is on the application homepage
    When The user navigates to the "Consumer & Community Banking" LOB
    Then The LOB page should load successfully
    And The LOB metrics should be displayed

  Scenario: Navigate to a Product and verify metrics
    Given The user is on the LOB page
    When The user selects the "Field Performance Reporting & Insights" Product
    Then The Product page should load successfully
    And The Product metrics should be displayed

  Scenario: Navigate to an Area Product and verify metrics
    Given The user is on the Product page
    When The user selects the "BB FPR&I" Area Product
    Then The Area Product page should load successfully
    And The Area Product metrics should be displayed

  Scenario: Verify error handling for failed metrics
    Given The user is on any page
    When Metrics fail to load
    Then An appropriate error message should be displayed


import { Given, When, Then } from 'cypress-cucumber-preprocessor/steps';

Given('The user is on the application homepage', () => {
  cy.visit('http://localhost:9900/agility-metrics/product-groups/lobs'); // Update with actual URL
  cy.wait(2000);
});

When('The user navigates to the {string} LOB', (lobName) => {
  cy.contains(lobName).click();
  cy.wait(2000);
});

Then('The LOB page should load successfully', () => {
  cy.url().should('include', '/overview');
  cy.get('.metric-container').should('be.visible'); // Ensure metrics section is visible
});

Then('The LOB metrics should be displayed', () => {
  cy.get('.metric-card').each(($el) => {
    cy.wrap($el).should('be.visible');
  });
});

When('The user selects the {string} Product', (productName) => {
  cy.contains(productName).click();
  cy.wait(2000);
});

Then('The Product page should load successfully', () => {
  cy.url().should('include', '/product-overview');
  cy.get('.metric-container').should('be.visible');
});

Then('The Product metrics should be displayed', () => {
  cy.get('.metric-card').each(($el) => {
    cy.wrap($el).should('be.visible');
  });
});

When('The user selects the {string} Area Product', (areaProductName) => {
  cy.contains(areaProductName).click();
  cy.wait(2000);
});

Then('The Area Product page should load successfully', () => {
  cy.url().should('include', '/team-overview');
  cy.get('.metric-container').should('be.visible');
});

Then('The Area Product metrics should be displayed', () => {
  cy.get('.metric-card').each(($el) => {
    cy.wrap($el).should('be.visible');
  });
});

When('Metrics fail to load', () => {
  cy.intercept('GET', '**/metrics', { statusCode: 500 }).as('metricsFail');
  cy.wait('@metricsFail');
});

Then('An appropriate error message should be displayed', () => {
  cy.get('.error-message').should('contain', 'Failed to load metrics');
});

Feature: LOB Navigation and Metrics Verification

  Scenario: Navigate through LOB structure and verify metrics
    Given the user is on the landing page
    When the user navigates to the "LOB" page
    Then the "LOB" page should load successfully
    And metrics relevant to "LOB" should be displayed

    When the user selects and navigates to a "Product" page
    Then the "Product" page should load successfully
    And metrics relevant to "Product" should be displayed

    When the user selects and navigates to an "Area Product" or "Team" page
    Then the "Area Product/Team" page should load successfully
    And metrics relevant to "Area Product/Team" should be displayed

  Scenario: Verify metrics visibility and correctness
    Given the user is on any metrics page
    Then all metrics should be visible and not hidden
    And each metric should have a label and value

  Scenario: Handle metric load failures
    Given the user is on any metrics page
    When metrics fail to load
    Then an appropriate error message should be displayed


import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";

Given("the user is on the landing page", () => {
  cy.visit("/");
});

When("the user navigates to the {string} page", (page) => {
  cy.contains(page).click();
});

Then("the {string} page should load successfully", (page) => {
  cy.url().should("include", page.toLowerCase());
  cy.get("h1").should("contain.text", page);
});

Then("metrics relevant to {string} should be displayed", (page) => {
  cy.get(".metrics").should("be.visible");
  cy.get(".metric-item").each(($metric) => {
    cy.wrap($metric).should("contain.text", ":");
  });
});

Given("the user is on any metrics page", () => {
  cy.get(".metrics").should("exist");
});

Then("all metrics should be visible and not hidden", () => {
  cy.get(".metric-item").should("be.visible");
});

Then("each metric should have a label and value", () => {
  cy.get(".metric-item").each(($metric) => {
    cy.wrap($metric).find(".label").should("not.be.empty");
    cy.wrap($metric).find(".value").should("not.be.empty");
  });
});

When("metrics fail to load", () => {
  cy.intercept("GET", "/api/metrics", { statusCode: 500 }).as("metricsFail");
  cy.reload();
});

Then("an appropriate error message should be displayed", () => {
  cy.get(".error-message").should("be.visible").and("contain.text", "failed to load");
});
