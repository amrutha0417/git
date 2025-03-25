Feature: Validate Metrics Loading for All Hierarchy Levels  

  As a user  
  I want to navigate through different LOBs, Sub-LOBs, Area Products, and Teams  
  So that I can verify that metric details load correctly  

  Scenario: Verify metrics load for all hierarchy levels  
    Given I open the application  
    When I recursively navigate through the entire hierarchy  
    Then I should see metric details loaded at each level  







/// <reference types="cypress" />

Given("I open the application", () => {
  cy.visit("http://localhost:9900/#/agility-metrics"); // Adjust URL
});

When("I recursively navigate through the entire hierarchy", () => {
  cy.get(".lob-selector").each(($lob, lobIndex) => {
    cy.wrap($lob).click(); // Click LOB  
    cy.wait(500); // Wait for sub-LOBs to load  

    cy.get(".sub-lob-selector").each(($subLob, subLobIndex) => {
      cy.wrap($subLob).click(); // Click Sub-LOB  
      cy.wait(500); // Wait for Area Products to load  

      cy.get(".area-product-selector").each(($areaProduct, areaProductIndex) => {
        cy.wrap($areaProduct).click(); // Click Area Product  
        cy.wait(500); // Wait for Teams to load  

        cy.get(".team-selector").each(($team, teamIndex) => {
          cy.wrap($team).click(); // Click Team  
          cy.wait(500); // Wait for Metrics to load  

          // Verify metrics are loading
          cy.get(".metric-details")
            .should("be.visible")
            .and("not.be.empty");
        });
      });
    });
  });
});

Then("I should see metric details loaded at each level", () => {
  cy.get(".metric-details").should("be.visible").and("not.be.empty");
});



