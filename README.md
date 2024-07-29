# 🧰 Silverstripe Search SDK

Silverstripe Search is designed to integrate easily with the Silverstripe CMS. This Software Development Kit (SDK) is a one-stop-shop for getting your application integrated.

## Setting up

Before you can get stared with this SDK you will need:

- A Silverstripe CMS application
- Bifröst Environment variables (provided when you sign up for Silverstripe Search)

## Installation

To install the complete SDK run

```
composer require silverstripe/silverstripe-search-sdk
```

## Using the SDK

### Environment variables

To get started you will need to add the Bifröst environment variables (provided when you sign up for Silverstripe Search). 

For local development this is normally done in your `.env` file and should look like the following:

```
BIFROST_ENDPOINT=<< provided on sign up >>
BIFROST_ENGINE_PREFIX=<< provided on sign up >>
BIFROST_MANAGEMENT_API_KEY=<< provided on sign up >>
BIFROST_QUERY_API_KEY=<< provided on sign up >>
```

For Silverstripe Cloud, you should add `BIFROST_ENDPOINT` and `BIFROST_ENGINE_PREFIX` as Variables, and `BIFROST_MANAGEMENT_API_KEY` and `BIFROST_QUERY_API_KEY` should be added as Secrets.

### Configuration

You will also need to set up an initial index config file. We suggest using `app/_config/search.yml` so its easy to find later. A simple index that just indexes Page titles would look like:

```YAML
SilverStripe\Forager\Service\IndexConfiguration:
  indexes:
    main:
      includeClasses:
        SilverStripe\CMS\Model\SiteTree:
          fields:
            title:
              property: Title
``` 

Here the name of the index variant is `main`. This is added to your `BIFROST_ENGINE_PREFIX` to give the full name of your index. E.g. if the `BIFROST_ENGINE_PREFIX=search-acme-prod` then this index's full name is `search-acme-prod-main`.

There are a range of options for you to customise your indexing to get the most out of your content. Refer to the [Forager configuration docs](https://github.com/silverstripeltd/silverstripe-forager/blob/1.0.0/docs/en/configuration.md) for more information. 

> [!WARNING]
> Once you add a field to an index you cannot change its name or type without deleting the engine so choose field names carefully

## 🛼 Roll-your-own installation

If you want to dial in your installation you can manually add the packages in the following groups:

### Querying

The [silverstripe/silverstripe-discoverer](https://github.com/silverstripeltd/silverstripe-discoverer) module contains provider-agnostic interfaces for performing search queries, including filtering and faceting. The [silverstripe/silverstripe-discoverer-bifrost](https://github.com/silverstripeltd/silverstripe-discoverer-bifrost) contains Silverstripe Search specific implementation to connect you with our Bifröst API. 

Refer to the [silverstripe/silverstripe-discoverer docs](https://github.com/silverstripeltd/silverstripe-discoverer) for more information on its use.

### Search Content management

The [silverstripe/silverstripe-forager](https://github.com/silverstripeltd/silverstripe-forager) module contains provider-agnostic interfaces and admin UI for configuring the service and index management. The [silverstripe/silverstripe-forager-bifrost](https://github.com/silverstripeltd/silverstripe-forager-bifrost) contains Silverstripe Search specific implementation to connect you with our Bifröst API. 

Refer to the [silverstripe/silverstripe-forager docs](https://github.com/silverstripeltd/silverstripe-forager) for more information on its use.

### Search UI

The [silverstripe/silverstripe-discoverer-search-ui](https://github.com/silverstripeltd/silverstripe-discoverer-search-ui) module contains the base parts to show results on a Search page including a Page Type, Controller, templates, and basic styling.

### Search page and results

The [silverstripe/silverstripe-discoverer-search-ui](https://github.com/silverstripe/silverstripe-discoverer-search-ui) package contains a base Search Page Type, Controller and Templates that work with the [discoverer](https://github.com/silverstripeltd/silverstripe-discoverer) and [forager](https://github.com/silverstripeltd/silverstripe-forager) modules. You can use the CMS to add the new Page Type to get up and running quickly.

To customise the look and feel of your search page then check out the [silverstripe/silverstripe-discoverer-search-ui](https://github.com/silverstripe/silverstripe-discoverer-search-ui) docs. If you would like to add filters or other advance query options then head over to the [discoverer](https://github.com/silverstripeltd/silverstripe-discoverer) module. 
