
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

