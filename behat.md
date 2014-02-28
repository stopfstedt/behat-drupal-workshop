---
layout: default
next_page: behat_and_drupal.html
prev_page: introduction.html
---

# Behat

## What It Is

* BDD framework for in PHP
* Open Source (MIT License)
* Inspired by [Cucumber](http://cukes.info/)

## How To Use It

1. Write your user stories ( _Features_ and _Scenarios_ ) in a human-readable format ( _Gerkin language_ ).

    ```gherkin
    Feature: Some terse yet descriptive text of what is desired
    In order to realize a named business value
    As an explicit system actor
    I want to gain some beneficial outcome which furthers the goal

    Scenario: Some determinable business situation
    Given some precondition
    And some other precondition
    When some action by the actor
    And some other action
    And yet another action
    Then some testable outcome is achieved
    And something else we can check happens too
    ```

2. Back up your stories with code (_Step Definitions_).

    ```php
    <?php
      /**
       * @Then /^some testable outcome is achieved$/
       */
      public function assertSomeTestableOutcomeIsAchieved() {
        // make your assertion here,
        // if it fails throw an exception
      }
    ```

3. Run it.

## The Stack

* [Behat](http://behat.org/)
* [Mink](http://mink.behat.org/)
    * sister project to Behat
    * provides web driver integration for Behat
    * pluggable (Goutte, Selenium, ZombieJS, others)
* [Selenium WebDriver](http://docs.seleniumhq.org/)
    * Open Source web driver
    * emulates user behaviour in web browsers
    * Java
    * takes input from Mink
    * works with [any number of browsers](http://docs.seleniumhq.org/about/platforms.jsp#browsers) (FF, Chrome, IE, Safari)
*  [PhantomJS](http://phantomjs.org/)
    * open source [headless web browser](http://blog.arhg.net/2009/10/what-is-headless-browser.html)
    * requires Node.js
    * can be integrated with Selenium

