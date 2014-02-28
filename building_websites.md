---
layout: default
next_page: module_development.html
prev_page: behat_and_drupal.html
---

# Building Websites

Implement your website _according to spec_.

## Case Study: ITS Website Migration

* [UCSF's ITS Homepage](http://it.ucsf.edu)
* powered by Views, Context, Nodes, CCK, Blocks, Taxonomy
* Migration from Drupal 6 to 7 _as-is_
* _But, where is the original site spec?!_

## Approach

1. Cover the D6 site with feature tests
2. Rebuild the site in D7
3. Migrate content over
4. Run _the same_ feature tests against D7 site
5. _Success!_

## What To Test

* homepage _[done]_
* common page elements (navigation, footer, banner) _[done]_
* public views (How To, Services Lists) _[todo]_
* exemplary node pages - _[todo]_

## Hands-on Demo

1. Follow the [install instructions](https://github.com/ucsf-drupal/behat_it_website/blob/master/README.md)
    - Skip the steps for setting up PhantomJS
    - Composer installation instructions for [Windows](https://getcomposer.org/doc/00-intro.md#installation-windows), [Mac](https://getcomposer.org/doc/00-intro.md#globally-on-osx-via-homebrew-) and [Linux](https://getcomposer.org/doc/00-intro.md#globally)
2. Grab the code from the [GitHub repo](https://github.com/ucsf-drupal/behat_it_website)
3. [Run the tests in the web-browser](https://github.com/ucsf-drupal/behat_it_website/blob/master/README.md#run-test-in-a-web-browser)

Questions? Problems?

_Let's take it apart..._

## Configuration

* Behat Configuration `behat.yml`
* defaults

    ```yaml
    default:
      extensions:
        Behat\MinkExtension\Extension:
          base_url: http://it.ucsf.edu
          default_session: goutte
          javascript_session: selenium2
          browser_name: "firefox"
          goutte: ~
          selenium2:
            capabilities: { "browser": "firefox", "version": "14" }
    ```

* Regions mapping in the Drupal extension

    ```yaml
    Drupal\DrupalExtension\Extension:
      blackbox: ~
      region_map:
        UCSF Nav: '#ucsf-nav'
        UCSF Mobile Nav: '#ucsf-nav-mobile'
        Main Nav: '#header .primary-links'
        Main Top Home Page: '#maintop'
        Main Left Home Page: '#main-left'
        Main Middle Home Page: '#main-mid'
        Main Right Home Page: '#main-right'
        Secondary Page Left Sidebar: '#sidebar-left'
        Secondary Page Left Sidebar Bottom: '#sidebar-left .block-side-bar_left_bottom'
        Secondary Page Middle Column: '#content-middle'
        Footer: '#footer'
    ```

* driver-specific profiles

    ```yaml
    # Use this profile to run all tests in Firefox
    browser:
      extensions:
        Behat\MinkExtension\Extension:
          default_session: selenium2
    ```

## Implementation

### Features

* see `features/homepage.feature` and co.

* Feature Declaration

    ```gherkin
    Feature: Homepage
      In order to quickly gain an overview on the site's offerings
      As a site visitor
      I want to see primary services, tools and recent news listed on the homepage
    ```

* Background (runs before each scenario)

    ```gherkin
    Background:
      Given I am on the homepage
    ```

* Scenario

    ```gherkin
    Scenario: Main Top
      Then I should see 3 image-links in the "Main Top Home Page" region
    ```

##Step Definitions

* implemented in `features/bootstrap/FeatureContext.php`
* Class extending `Drupal\DrupalExtension\Context\DrupalContext`
* step definitions are class methods
* mapping to steps via annotations and Regular Expressions in method doc blocks
* find page elements via CSS or XPath selectors

```php
<?php
    /**
     * @Then /^I should see (\d+) image-links in the "([^"]*)" region$/
     */
    public function iShouldSeeImageLinksInTheRegion($num, $region) {
        $regionObj = $this->getRegion($region);
        if (! $regionObj) {
            throw new \Exception(sprintf('Region "%s" on the page %s does not exist.', $region, $this->getSession()->getCurrentUrl()));
        }
        $regionId = $regionObj->getAttribute('id');
        $this->assertNumElements((int) $num, "#{$regionId} a > img");
    }
```

## Learn More

* [Behat Guide: Writing Features in the Gherkin Language](http://docs.behat.org/guides/1.gherkin.html)
* [Behat Guide: Step Definitions](http://docs.behat.org/guides/2.definitions.html)
