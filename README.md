# search-ui-azure-connector

This connector is used to connect [Search UI](https://github.com/elastic/search-ui) to [Azure Cognitive Search](https://azure.microsoft.com/en-us/services/search/). Checkout the live demo of [Search UI with Azure Cognitive Search](https://azurecognitivesearchui.z19.web.core.windows.net), or have a look at the [React sample](#sample).

## Getting started

Install **React Search UI** and the **Azure Cognitive Search connector**.

```bash
npm install --save @elastic/react-search-ui search-ui-azure-connector
```

```javascript
import { AzureCognitiveSearchConnector } from "search-ui-azure-connector";

// We'll connect to the Azure Cognitive Search public sandbox and send a query
// to its "nycjobs" index built from a public dataset of available jobs in New York
// https://www.npmjs.com/package/@azure/search-documents#send-your-first-search-query
const connector = new AzureCognitiveSearchConnector({
  endpoint: "https://azs-playground.search.windows.net/",
  queryKey: "252044BE3886FE4A8E3BAA4F595114BB",
  indexName: "nycjobs",
});

<SearchProvider
  config={{
    apiConnector: connector,
  }}
>
  <div className="App">{/* Place Components here! */}</div>
</SearchProvider>;
```

> If you would like to connect to your own Azure Cognitive Search instance, make sure to use the [query key](https://docs.microsoft.com/en-us/azure/search/search-security-api-keys) and not the admin key.

## Configuration

All configuration for Search UI is provided in a single configuration object, as documented [here](https://github.com/elastic/search-ui/blob/master/ADVANCED.md#advanced-configuration).

This connector supports the configuration possibilities of Search UI for filters, facets, autocomplete and suggestions. Specific Azure Cognitive Search configuration possibilities are documented below.

### Set custom suggester

By default, the connector uses `sg` as the suggester name for autocomplete and suggestions. If you want to specify a custom [suggester](https://docs.microsoft.com/en-us/azure/search/index-add-suggesters) name, add the `suggester` property to the `results` and/or `suggestions` config.

```json
{
  "autocompleteQuery": {
    "results": {
      "suggester": "sg"
    },
    "suggestions": {
      "suggester": "sg"
    }
  }
}
```

### Set Autocomplete Mode

The Autocomplete API supports [three different modes](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete#autocomplete-modes). The default mode is `oneTerm`. You can add the `autocompleteMode` property to the `results` config, to change the Autocomplete Mode.

```javascript
{
  "autocompleteQuery": {
    "results": {
      "autocompleteMode": "twoTerms" // oneTerm, twoTerms or oneTermWithContext
    }
  }
}
```

If you specify fields in `result_fields`, make sure they exist in your suggester.

## Limitations

This connector does not support all the functionality offered by [Search UI](https://github.com/elastic/search-ui). The following features are not (yet) supported.

- Facet search
- autocompleteQuery does not take results per page into account
- Search UI doesn't offer a 'detail page' currently, just the search overview

## Sample

A sample using the more advanced features of Search UI can be found [here](./sample), built via [Create React App](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app). Checkout the live demo of [Search UI with Azure Cognitive Search](https://azurecognitivesearchui.z19.web.core.windows.net) or have a look at the [code sandbox](https://codesandbox.io/s/search-ui-nycjobs-d79oq).
