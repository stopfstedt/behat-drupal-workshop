---
layout: default
next_page: building_websites.html
prev_page: behat.html
---

# Behat and Drupal

* [Behat Drupal Extension](https://drupal.org/project/drupalextension)
* Drupal-specific additions:
    - region mapping/region scope
    - text mapping
    - subcontext discovery
    - step definitions for common scenarios

## The three drivers

* Blackbox
    * assumes anonymous user
    * works with remote sites

* Drush
    * works with remote sites
    * allows user creation

* Drupal API
    * provides super user privileges
    * allows creation of users, nodes, vocabularies/taxonomies
    * tag scenarios with `@api`
    * works _only_ with local sites

[Comparison matrix](http://dspeak.com/drupalextension/drivers.html)

## Extension via subcontexts

* Define step-definitions in `*.behat.inc`
