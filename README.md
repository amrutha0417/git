
Feature: Navigation through hierarchical structure

  Scenario: User navigates through LOBs, Sub LOBs, Area Product Lines, and Teams
    Given I open the application
    When I click on the LOB "Finance"
    Then I should see Sub LOBs loaded
    And I should see metrics details loaded

    When I click on the Sub LOB "Corporate Banking"
    Then I should see Area Product Lines loaded
    And I should see metrics details loaded

    When I click on the Area Product Line "Risk Management"
    Then I should see Teams loaded
    And I should see metrics details loaded

    When I click on the Team "Fraud Detection"
    Then I should see metrics details loaded



/// <reference types="cypress" />

import { Given, When, Then } from "cypress-cucumber-preprocessor/steps";

Given("I open the application", () => {
  cy.visit("/"); // Replace with the correct application URL if needed
});

When("I click on the LOB {string}", (lobName: string) => {
  cy.get("ul.lob-list li").contains(lobName).click();
});

Then("I should see Sub LOBs loaded", () => {
  cy.get("ul.sub-lob-list").should("be.visible");
});

Then("I should see metrics details loaded", () => {
  cy.get(".metrics-container").should("be.visible"); // Adjust this selector based on your application
});

When("I click on the Sub LOB {string}", (subLobName: string) => {
  cy.get("ul.sub-lob-list li").contains(subLobName).click();
});

Then("I should see Area Product Lines loaded", () => {
  cy.get("ul.area-product-list").should("be.visible");
});

When("I click on the Area Product Line {string}", (areaProductName: string) => {
  cy.get("ul.area-product-list li").contains(areaProductName).click();
});

Then("I should see Teams loaded", () => {
  cy.get("ul.team-list").should("be.visible");
});

When("I click on the Team {string}", (teamName: string) => {
  cy.get("ul.team-list li").contains(teamName).click();
});


