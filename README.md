# 🧰 Silverstripe Search SDK

Silverstripe Search is designed to integrate easily with the Silverstripe CMS. This Software Development Kit (SDK) is a one-stop-shop for getting your application integrated.

<!-- TOC -->
* [🧰 Silverstripe Search SDK](#-silverstripe-search-sdk)
  * [Tagged dependencies](#tagged-dependencies)
  * [Engine vs Index](#engine-vs-index)
  * [Setting up](#setting-up)
  * [Installation](#installation)
    * [Install the complete SDK run:](#install-the-complete-sdk-run)
    * [Updates](#updates)
    * [🛼 Roll-your-own installation](#-roll-your-own-installation)
  * [Specify environment variables](#specify-environment-variables)
    * [Understanding your engine prefix and suffix:](#understanding-your-engine-prefix-and-suffix)
  * [Indexing your CMS content](#indexing-your-cms-content)
  * [Get search to your users](#get-search-to-your-users)
<!-- TOC -->

## Tagged dependencies

As part of the Silverstripe Search managed service, we will put the required dependencies (and the explicit versions) through our internal regression testing before adding them to this SDK.

This means that new tags for the SDK **will trail** tags being made on the upstream dependencies to ensure they are stable and work well with the service.

**If you want updates to dependencies faster**, then you should define your own requirements for those dependencies with your own version constraints. By doing so, you accept the risks of your defined versioned constraints.

## Engine vs Index

> [!IMPORTANT]
> **TL;DR:**\
> For all intents and purposes, "engine" and "index" are synonomous. If we refer to something as "engine", but the Discoverer module is asking for an "index", then you simply need to give it the data you have for your engine.

The Discoverer and Forager modules are built to be service-agnostic; meaning, you can use them with any search provider, as long as there is an adaptor for that service.

When Discoverer refers to an "index", it is talking about the data store used for housing your content. These data stores are known by different names across different search providers. Algolia and Elasticsearch call them "indexes", Typesense calls them "collections", App Search calls them "engines". Discoverer had to call them **something** in its code, and it chose to call then "indexes"; Silverstripe Search, however, calls them "engines".

Actions apply in the same way to all of the above. In Silverstripe Search, the action of "indexing" is the action of adding data to your engine, where it is said to be "indexed". Updating that data is commonly referred to as "re-indexing".

## Setting up

Before you can get stared with this SDK you will need:

- A Silverstripe CMS application
- Bifröst Environment variables (provided when you sign up for Silverstripe Search)

## Installation

There are 3 core parts to the SDK:

1. **Indexing:** Getting your CMS content indexed into the search engine
   - Provided by the [Forager](https://github.com/silverstripeltd/silverstripe-forager) module 
2. **Querying:** Searching the indexed content in your search engine
   - Provided by the [Discoverer](https://github.com/silverstripeltd/silverstripe-discoverer) module 
3. **Search UI:** A default implementation of a search results page with basic control over search options, designed to get you up and running quickly if you have basic search requirements
   - Provided by the [Discoverer > Search UI](https://github.com/silverstripeltd/silverstripe-discoverer-search-ui) module 

**1. Indexing** and **2. Querying** are likely to be requirements for most projects, but **3. Search UI** potentially **isn't**. Some reasons why you might not want the Search UI include:

* You are using Subsites
* You are using Fluent
* You perform your search queries through JavaScript/XHR
* You have other complex search requirements that aren't covered by the Search UI module

### Install the complete SDK run:

```shell
composer require silverstripe/silverstripe-search-sdk
```

### Updates

To update the SDK, be sure to include `-W` / `--with-all-dependencies`

```
composer update silverstripe/silverstripe-search-sdk -W
```

### 🛼 Roll-your-own installation

If you want to dial in your installation you can manually add the packages above to your composer.json.

- If you only want **1. Indexing**, then you could require just the Forager dependencies
- If you only want **2. Querying**, then you could require just the Discoverer dependencies

It will be up to you what version constraints you apply, but noting again, only the versions required in this repository have gone through our regression testing process.

## Specify environment variables

To integrate with Silverstripe Search, define environment variables containing your endpoint, engine prefix, management API key, and query API key.

```
BIFROST_ENDPOINT=<< provided on sign up >>
BIFROST_ENGINE_PREFIX=<< provided on sign up >>
BIFROST_MANAGEMENT_API_KEY=<< provided on sign up >>
BIFROST_QUERY_API_KEY=<< provided on sign up >>
```

> [!TIP]
> For Silverstripe Cloud clients:
> - `BIFROST_ENDPOINT` and `BIFROST_ENGINE_PREFIX` should be added as Variables
> - `BIFROST_MANAGEMENT_API_KEY` and `BIFROST_QUERY_API_KEY` should be added as Secrets.

### Understanding your engine prefix and suffix:

* All Silverstripe Search engine names follow a 4 slug format like this: `search-<subscription>-<environment>-<suffix>`
* Your `<engine-prefix>` is everything except `-<suffix>`; so, it's just `search-<subscription>-<environment>`

For example:

| Engine                    | Engine prefix        | Engine suffix |
|---------------------------|----------------------|---------------|
| search-acmecorp-prod-main | search-acmecorp-prod | main          |
| search-acmecorp-prod-inc  | search-acmecorp-prod | inc           |
| search-acmecorp-uat-main  | search-acmecorp-uat  | main          |
| search-acmecorp-uat-inc   | search-acmecorp-uat  | inc           |

**Why?**

Because you probably have more than one environment type that you're running search on (e.g. Production and UAT), and (generally speaking) you should have different engines for each of those environments. So, you can't just hardcode the entire engine name into your project, because that code doesn't change between environments.

Whenever you make a query, Forager and Discoverer will ask you for the "index" name; you will actually want to provide only the `<suffix>`. We will then take `BIFROST_ENGINE_PREFIX` and your `<suffix>`, put them together, and that's what will be queried. This allows you to set `BIFROST_ENGINE_PREFIX` differently for each environment, while having your `<suffix>` hardcoded in your project.

## Indexing your CMS content

Please see the [Forager > Bifröst](https://github.com/silverstripeltd/silverstripe-forager-bifrost?tab=readme-ov-file#configuration) documentation for details on how to get set up with indexing your content.

## Get search to your users

You may or may not want to use the [Discoverer > Search UI](https://github.com/silverstripeltd/silverstripe-discoverer-search-ui) module, **if you don't want to use it**, then the Discoverer module has quite a bit of documentation regarding how you can get your search form and results in front of your users; including an example `SearchResults` Page and Controller:

* [Discoverer > Bifröst](https://github.com/silverstripeltd/silverstripe-discoverer-bifrost?tab=readme-ov-file#usage) documentation for details on how to start querying the indexed content from your engines.
* [Simple usage](https://github.com/silverstripeltd/silverstripe-discoverer/blob/main/docs/simple-usage.md): An example `SearchResults` Page, Controller, and template, with a basic query example with pagination and  result fields
* [Detailed querying](https://github.com/silverstripeltd/silverstripe-discoverer/blob/main/docs/detailed-querying.md): How to query with filters, search fields, result fields, facets, etc
* [Detailed result handling](https://github.com/silverstripeltd/silverstripe-discoverer/blob/main/docs/detailed-result-handling.md): More information about the `Results` object that Discoverer returns to you whenever you perform a search query
