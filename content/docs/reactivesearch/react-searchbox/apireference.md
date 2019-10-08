---
title: 'QuickStart'
meta_title: 'QuickStart to React SearchBox'
meta_description: 'React SearchBox is a lightweight react search library with some common utilities.'
keywords:
    - quickstart
    - react-searchbox
    - search library
    - elasticsearch
sidebar: 'docs'
nestedSidebar: 'react-searchbox-reactivesearch'
---

![Image to be displayed](https://i.imgur.com/toQyZW6.png)

`React SearchBox` creates a search box UI component that is connected to one or more database fields.

Example uses:

-   Searching for a rental listing by its `name` or `description` field.
-   Creating an e-commerce search box for finding products by their listing properties.

## Usage

### Basic Usage

```js
<SearchBox dataField={['group_venue', 'group_city']} />
```

### Usage With All Props

```js
<SearchBox
	app="good-books-ds"
	credentials="nY6NNTZZ6:27b76b9f-18ea-456c-bc5e-3a5263ebc63d"
	dataField={[
		{ field: 'original_title', weight: 1 },
		{ field: 'original_title.search', weight: 3 },
	]}
	title="Search"
	defaultValue="Songwriting"
	placeholder="Search for books"
	autosuggest={true}
	defaultSuggestions={[
		{ label: 'Songwriting', value: 'Songwriting' },
		{ label: 'Musicians', value: 'Musicians' },
	]}
	highlight={true}
	highlightField="group_city"
	queryFormat="or"
	fuzziness="AUTO"
	showClear
	showVoiceSearch
/>
```

## Props

-   **dataField** `string | Array<string | DataField>` [required]
    SearchBox accepts an Array in addition to String, which is useful for searching across multiple fields with or without field weights.<br/>
    Field weights allow weighted search for the index fields. This prop accepts an array of numbers. A higher number implies a higher relevance weight for the corresponding field in the search results.<br/>
    You can define the `dataField` as an array of objects of the `DataField` type to set the field weights.<br/>
    The `DataField` type has the following shape:
    ```ts
    type DataField = {
    	field: string;
    	weight: number;
    };
    ```
-   **nestedField** `String` [optional]
    use to set the `nested` mapping field that allows arrays of objects to be indexed in a way that they can be queried independently of each other. Applicable only when dataField is a part of `nested` type.
-   **title** `String or JSX` [optional]
    set the title of the component to be shown in the UI.
-   **defaultValue** `string` [optional]
    set the initial search query text on mount.
-   **value** `string` [optional]
    controls the current value of the component. It sets the search query text (on mount and on update). Use this prop in conjunction with `onChange` function.
-   **downShiftProps** `Object` [optional]
    allow passing props directly to `Downshift` component. You can read more about Downshift props [here](https://github.com/paypal/downshift#--downshift-------).
-   **placeholder** `String` [optional]
    set the placeholder text to be shown in the SearchBox input field. Defaults to "Search".
-   **showIcon** `Boolean` [optional]
    whether to display a search or custom icon in the input box. Defaults to `true`.
-   **iconPosition** `String` [optional]
    sets the position of the search icon. Can be `left` or `right`. Defaults to `right`.
-   **icon** `JSX` [optional]
    displays a custom search icon instead of the default 🔍
-   **showClear** `Boolean` [optional]
    show a clear text icon. Defaults to `false`.
-   **clearIcon** `JSX` [optional]
    allows setting a custom icon for clearing text instead of the default cross.
-   **autoSuggest** `Boolean` [optional]
    set whether the autoSuggest functionality should be enabled or disabled. Defaults to `true`.
-   **strictSelection** `Boolean` [optional]
    defaults to `false`. When set to `true` the component will only set its value and fire the query if the value was selected from the suggestion. Otherwise the value will be cleared on selection. This is only relevant with `autoSuggest`.
-   **defaultSuggestions** `Array` [optional]
    preset search suggestions to be shown on focus when the SearchBox does not have any search query text set. Accepts an array of objects each having a **label** and **value** property. The label can contain either String or an HTML element.
-   **debounce** `Number` [optional]
    sets the milliseconds to wait before executing the query. Defaults to `0`, i.e. no debounce.
-   **highlight** `Boolean` [optional]
    whether highlighting should be enabled in the returned results.
-   **highlightField** `String or Array` [optional]
    when highlighting is enabled, this prop allows specifying the fields which should be returned with the matching highlights. When not specified, it defaults to applying highlights on the field(s) specified in the **dataField** prop.
-   **queryFormat** `String` [optional]
    Sets the query format, can be **or** or **and**. Defaults to **or**.

    -   **or** returns all the results matching **any** of the search query text's parameters. For example, searching for "bat man" with **or** will return all the results matching either "bat" or "man".
    -   On the other hand with **and**, only results matching both "bat" and "man" will be returned. It returns the results matching **all** of the search query text's parameters.

-   **fuzziness** `String or Number` [optional]
    Sets a maximum edit distance on the search parameters, can be **0**, **1**, **2** or **"AUTO"**. Useful for showing the correct results for an incorrect search parameter by taking the fuzziness into account. For example, with a substitution of one character, **fox** can become **box**. Read more about it in the elastic search [docs](https://www.elastic.co/guide/en/elasticsearch/guide/current/fuzziness.html).
-   **showVoiceSearch** `Boolean` [optional]
    show an option in search bar to enable the voice to text search. Defaults to `false`.
-   **searchOperators** `Boolean` [optional]
    Defaults to `false`, if set to `true` than you can use special characters in the search query to enable the advanced search.<br/>
    Read more about it [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-simple-query-string-query.html).
-   **URLParams** `Boolean` [optional]
    enable creating a URL query string parameter based on the selected value of the list. This is useful for sharing URLs with the component state. Defaults to `false`.
-   **render** `Function` [optional]
    You can render custom suggestions by using `render` prop.
    <br/>
    It accepts an object with these properties:
    -   **`loading`**: `boolean`
        indicates that the query is still in progress
    -   **`error`**: `object`
        An object containing the error info
    -   **`data`**: `array`
        An array of parsed suggestions obtained from the applied query.
    -   **`rawData`**: `array`
        An array of original suggestions obtained from the applied query.
    -   **`value`**: `string`
        current search input value i.e the search query being used to obtain suggestions.
    -   **`downshiftProps`**: `object`
        provides all the control props from `downshift` which can be used to bind list items with click/mouse events.
        Read more about it [here](https://github.com/downshift-js/downshift#children-function).

```js
<SearchBox
	render={({ loading, error, data, value, downshiftProps: { isOpen, getItemProps } }) => {
		if (loading) {
			return <div>Fetching Suggestions.</div>;
		}
		if (error) {
			return <div>Something went wrong! Error details {JSON.stringify(error)}</div>;
		}
		return isOpen && Boolean(value.length) ? (
			<div>
				{data.slice(0, 5).map((suggestion, index) => (
					<div key={suggestion.value} {...getItemProps({ item: suggestion })}>
						{suggestion.value}
					</div>
				))}
				{Boolean(value.length) && (
					<div {...getItemProps({ item: { label: value, value: value } })}>
						Show all results for "{value}"
					</div>
				)}
			</div>
		) : null;
	}}
/>
```

Or you can also use render function as children

```js
<SearchBox>
        {
            ({
                loading,
                error,
                data,
                rawData,
                value,
                downshiftProps
            }) => (
                // return UI to be rendered
            )
        }
</SearchBox>
```

-   **renderError** `String or JSX or Function` [optional]
    can be used to render an error message in case of any error.
    ```js
    renderError={(error) => (
            <div>
                Something went wrong!<br/>Error details<br/>{error}
            </div>
        )
    }
    ```
-   **renderNoSuggestion** `String or JSX or Function` [optional]
    can be used to render a message when there are no suggestions found.
    ```js
    renderNoSuggestion={() => (
            <div>
                No suggestions found
            </div>
        )
    }
    ```
-   **getMicInstance** `Function` [optional]
    You can pass a callback function to get the instance of `SpeechRecognition` object, which can be used to override the default configurations.
-   **renderMic** `String or JSX or Function` [optional]
    can be used to render the custom mic option.<br/>
    It accepts an object with the following properties:

    -   **`handleClick`**: `function`
        needs to be called when the mic option is clicked.
    -   **`status`**: `string`
        is a constant which can have one of these values:<br/>
        `INACTIVE` - mic is in inactive state i.e not listening<br/>
        `STOPPED` - mic has been stopped by the user<br/>
        `ACTIVE` - mic is listening<br/>
        `DENIED` - permission is not allowed<br/>

    ```js
    	renderMic = {({ onClick, status }) => {
    				switch(status) {
    					case 'ACTIVE':
    						return <img src="/active_mic.png" onClick={onClick} />
    					case 'DENIED':
    					case 'STOPPED':
    						return <img src="/mute_mic.png" onClick={onClick} />
    					default:
    						return <img src="/inactive_mic.png" onClick={onClick} />
    				}
    	}}
    ```

-   **onChange** `function` [optional]
    is a callback function which accepts component's current **value** as a parameter. It is called when you are using the `value` prop and the component's value changes. This prop is used to implement the [controlled component](https://reactjs.org/docs/forms.html#controlled-components) behavior.

    ```js
    <DataSearch
    	value={this.state.value}
    	onChange={(value, triggerQuery, event) => {
    		this.setState(
    			{
    				value,
    			},
    			() => triggerQuery(),
    		);
    	}}
    />
    ```

> Note:
>
> If you're using the controlled behavior then it's your responsibility to call the `triggerQuery` method to update the query i.e execute the search query and update the query results in connected components by `react` prop. It is not mandatory to call the `triggerQuery` in `onChange` you can also call it in other input handlers like `onBlur` or `onKeyPress`.

-   **onSuggestions** `Function` [optional]
    You can pass a callback function to listen for the changes in suggestions.The function receives `suggestions` list.
-   **onError** `Function` [optional]
    gets triggered in case of an error and provides the `error` object, which can be used for debugging or giving feedback to the user if needed.

## Demo

<br />

<iframe src="https://codesandbox.io/embed/github/ShahAnuj2610/react-searchbox/tree/feat/storybook/example" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

## Styles

`DataSearch` component supports `innerClass` prop with the following keys:

-   `title`
-   `input`
-   `list`

Read more about it [here](/docs/reactivesearch/v3/theming/classnameinjection/).

## Extending

`DataSearch` component can be extended to

1. customize the look and feel with `className`, `style`,
2. connect with external interfaces using `beforeValueChange`, `onValueChange` and `onQueryChange`,
3. add the following [synthetic events](https://reactjs.org/events.html) to the underlying `input` element:

    - onBlur
    - onFocus
    - onKeyPress
    - onKeyDown
    - onKeyUp
    - autoFocus

    > Note:
    >
    > 1. All these events accepts the `triggerQuery` as a second parameter which can be used to trigger the `SearchBox` query with the current selected value (useful to customize the search query execution).
    > 2. There is a known [issue](https://github.com/appbaseio/reactivesearch/issues/1087) with `onKeyPress` when `autosuggest` is set to true, it is recommended to use `onKeyDown` for the consistency.

```js
<DataSearch
  ...
  className="custom-class"
  style={{"paddingBottom": "10px"}}
  beforeValueChange={
    function(value) {
      // called before the value is set
      // returns a promise
      return new Promise((resolve, reject) => {
        // update state or component props
        resolve()
        // or reject()
      })
    }
  }
  onValueChange={
    function(value) {
      console.log("current value: ", value)
      // set the state
      // use the value with other js code
    }
  }
  onQueryChange={
    function(prevQuery, nextQuery) {
      // use the query with other js code
      console.log('prevQuery', prevQuery);
      console.log('nextQuery', nextQuery);
    }
  }

/>
```

-   **className** `String`
    CSS class to be injected on the component container.
-   **style** `Object`
    CSS styles to be applied to the **SearchBox** component.
-   **defaultQuery** `Function`
    takes **value** and **props** as parameters and **returns** the data query to be applied to the source component, as defined in Elasticsearch Query DSL, which doesn't get leaked to other components.<br/>
    Read more about it [here](/docs/reactivesearch/v3/advanced/customquery#when-to-use-default-query).
-   **beforeValueChange** `Function`
    is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called every-time before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
-   **onValueChange** `Function`
    is a callback function which accepts component's current **value** as a parameter. It is called every-time the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example: You want to show a pop-up modal with the valid discount coupon code when a user searches for a product in a SearchBox.
-   **onQueryChange** `Function`
    is a callback function which accepts component's **prevQuery** and **nextQuery** as parameters. It is called everytime the component's query changes. This prop is handy in cases where you want to generate a side-effect whenever the component's query would change.
