Feature: Search in left navigation

  Background:
    Given I am on the dashboard page

  Scenario Outline: Search for different entities and verify relevant level is displayed
    When I search for "<searchItem>" in the left navigation
    Then I should see the "<entity>" level displayed

    Examples:
      | entity             | searchItem         |
      | Product            | Auto               |
      | Product            | Business Banking   |
      | Product            | Finance            |
      | Product            | Risk               |
      | Product Owner      | John Doe           |
      | Area Product       | Retail Experience  |
      | Area Product Owner | Jane Smith         |
      | Team               | Fraud Team         |
      | Agile Lead         | Michael Johnson    |





Feature: Search in left navigation

  Background:
    Given I am on the dashboard page

  Scenario Outline: Search for an entity and verify the relevant level is displayed
    When I search for "<entity>" in the left navigation
    Then I should see the "<expectedSection>" level displayed

    Examples:
      | entity              | expectedSection          |
      | Auto                | Auto                     |
      | Business Banking    | Business Banking         |
      | Finance             | Finance                  |
      | Risk                | Risk                     |
      | John Doe            | Product Owner            |
      | Payments            | Product                  |
      | Retail Experience   | Area Product             |
      | Jane Smith          | Area Product Owner       |
      | Fraud Team          | Team                     |
      | Michael Johnson     | Agile Lead               |








Feature: Navigation and Metrics Verification in LOB Application

  As a user,
  I want to navigate through all LOBs, Products, and Teams
  So that I can verify that all metrics are displayed correctly

  Background:
    Given the user is on the application homepage

  Scenario: Navigate through all dynamically loaded LOBs, Products, and Teams
    When the user navigates through all LOBs and their sub-levels
    Then all relevant metrics should be displayed correctly at every level
    And no error messages should be displayed at any level



/// <reference types="cypress" />

import { Given, When, Then } from 'cypress-cucumber-preprocessor/steps';

// Recursive function to navigate all levels dynamically
function navigateHierarchy(level, parentSelector) {
  cy.get(parentSelector) // Get all items (LOBs, Products, etc.)
    .find('li')
    .each(($el) => {
      cy.wrap($el).click(); // Click the parent item to load child elements
      cy.wait(2000); // Wait for child elements to load

      // Verify that metrics are loaded
      cy.get('[data-testid="metrics-container"]').should('be.visible');
      cy.get('[data-testid="metric-label"]').each(($el) => {
        cy.wrap($el).should('be.visible').and('not.be.empty');
      });
      cy.get('[data-testid="metric-value"]').each(($el) => {
        cy.wrap($el).should('be.visible').and('not.be.empty');
      });

      // AFTER CLICKING: Re-fetch the child list (since it's dynamically loaded)
      cy.wrap($el).find('ul').then(($childList) => {
        if ($childList.length > 0) {
          navigateHierarchy(level + 1, $childList); // Recursively navigate deeper
        }
      });

      // Navigate back after processing the child elements
      cy.go('back');
      cy.wait(1000);
    });
}

// Step 1: Open the application
Given('the user is on the application homepage', () => {
  cy.visit('http://localhost:9900'); // Update with actual URL
  cy.wait(2000);
});

// Step 2: Start navigation from LOBs dynamically
When('the user navigates through all LOBs and their sub-levels', () => {
  navigateHierarchy(1, 'ul'); // Start recursive navigation from LOBs
});

// Step 3: Validate that metrics are correctly displayed at every level
Then('all relevant metrics should be displayed correctly at every level', () => {
  cy.get('[data-testid="metrics-container"]').should('be.visible');
  cy.get('[data-testid="metric-label"]').each(($el) => {
    cy.wrap($el).should('be.visible').and('not.be.empty');
  });
  cy.get('[data-testid="metric-value"]').each(($el) => {
    cy.wrap($el).should('be.visible').and('not.be.empty');
  });
});

// Step 4: Ensure no errors appear at any level
Then('no error messages should be displayed at any level', () => {
  cy.get('[data-testid="error-message"]').should('not.exist');
});



