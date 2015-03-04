# Cascading drop-down menu

jQuery plugin, which allows you to populate a set of form drop-down menus based on the previous selection.

## Demo

(Cascading drop-down menu jQuery plugin)[http://jquery-cascading-dropdown.ssdtutorials.com/]

## Basic usage

To use the plugin without overwriting any default settings, you'll need to create a structure of the form with a number of drop-down (select) menus.
In the following example I'm using

```
<form>

    <select
        name="category"
        class="cascadingDropDown"

        data-group="product-1"
        data-target="make"
        data-url="data/make.json"
        data-replacement="container1"

        >
        <option value="">Select category</option>
        <option value="1">Shoes</option>
        <option value="2">T-shirts</option>
        <option value="3">Jeans</option>
        <option value="4">Hats</option>
        <option value="5">Belts</option>
        </select>

    <select
        name="make"
        class="cascadingDropDown"

        data-group="product-1"
        data-id="make"
        data-target="colour"
        data-url="data/colour.json"
        data-replacement="container1"
        data-default-label="Select make"

        disabled
        >
        </select>

    <select
        name="colour"
        class="cascadingDropDown"

        data-group="product-1"
        data-id="colour"
        data-target="size"
        data-url="data/size.json"
        data-replacement="container1"
        data-default-label="Select colour"

        disabled
        >
        </select>

    <select
        name="size"
        class="cascadingDropDown"

        data-group="product-1"
        data-id="size"
        data-default-label="Select size"
        data-replacement="container1"
        data-url="data/final.json"

        disabled
        >
        </select>

</form>
```

### Select attributes

- Each select tag should have a trigger class assigned to it - in the above example I've used `cascadingDropDown`.
- The first select should have all items / options ready for selection.
- All other selects should have the `disabled` attribute.
- All selects should have their `name` attribute.
- All selects should have the following `data-*` attributes:
    - `data-group` : indicates association of the select elements and helps distinguish between different groups where multiple blocks of cascading drop-downs are used.
- All selects except the last one should have the following `data-*` attribute:
    - `data-target` : indicates the select element that should be affected by the value selected from its originator
    - `data-url` : url that needs to be called when the `change` event is triggered on the given select ( should also be applied to the last one if selection includes the replacement of the container - see below )
- All selects except the first one are also required to have the following `data-*` attributes:
    - `data-id` : the id of the element corresponds to the `data-trigger` of the previous element
    - `data-default-label` : the default label for the first `option` item
- Optional `data-*` attributes:
    - `data-replacement` : corresponding container with `data-replacement-container` - used for replacing additional content with each selection.


### Instantiating the plugin

To use the plugin you'll need the latest version of jQuery plus the plugin itself.

To instantiate the plugin without overwriting any default settings simply call it on the element using its class.


```
<script src="js/jquery-2.1.3.min.js"></script>
<script src="js/jquery.cascading-drop-down.js"></script>
<script>
    $('.cascadingDropDown').ssdCascadingDropDown();
</script>
```

## Parameters

You can overwrite the following settings of the plugin:

```
attrDataGroup                   : 'group', // data-group attribute
attrDataId                      : 'id', // data-id attribute
attrDataUrl                     : 'url', // data-url attribute
attrDataTarget                  : 'target', // data-target attribute
attrDataDefaultLabel            : 'default-label', // data-default-label attribute
attrDataReplacement             : 'replacement', // data-replacement attribute

attrDataReplacementContainer    : 'replacement-container', // data-replacement-container attribute
attrDataReplacementDefault      : 'default-content', // data-default-content attribute

classReplacementContainer       : 'cascadingContainer', // class associated with the receiving container

indexSuccess                    : 'success', // json response key to indicate whether the call was successful (true) or not (false)
indexError                      : 'error', // json response key to store the error message
indexMenu                       : 'menu', // json response key to store the new menu items
indexReplacement                : 'replacement', // json response key to store the replacement for the content container

verify                          : true, // whether to run verification with instantiation

errorCallback                   : function(message, data) { console.warn(message); } // method call when json response was not successful { success : false }. It takes the error message plus the all data returned back with the call
```

## Verification

By default plugin will log the missing `data-*` attributes on all selects when it's first instantiated - so if you check console you might see something like:

```
category is missing attribute data-id
category is missing attribute data-default-label
size is missing attribute data-target
category is missing attribute data-id
category is missing attribute data-default-label
size is missing attribute data-target
```

In the above example the log is referring to the optional parameters on the first and last item.

If you'd like to disable verification simply pass in the argument `verify` set to `false`:

```
$('.cascadingDropDown').ssdCascadingDropDown({
    verify : false
});
```

## Error callback

When the ajax call has returned a valid data with the `success` set to false - the `errorCallback` is executed. You can overwrite it with your own error handling by passing the function as `errorCallback` argument. Function takes 2 arguments - first being the actual error message returned in json format - second the whole json response.

```
$('.cascadingDropDown').ssdCascadingDropDown({
    errorCallback : function(message, data) {

        alert(message);

    }
});
```

## JSON response structure

The response sent back to the script should use the following format:

```
{
  "success": true,
  "replacement": "Records filtered by the make and any previous selection",
  "menu": [
    {
      "name": "White",
      "value": "1"
    },
    {
      "name": "Black",
      "value": "2"
    },
    {
      "name": "Yellow",
      "value": "3"
    },
    {
      "name": "Blue",
      "value": "4"
    },
    {
      "name": "Green",
      "value": "5"
    },
    {
      "name": "Red",
      "value": "6"
    }
  ]
}
```

The keys can be overwritten when instantiating the plugin:

```
$('.cascadingDropDown').ssdCascadingDropDown({
    indexSuccess        : 'isSuccess',
    indexError          : 'errorMessage',
    indexMenu           : 'items',
    indexReplacement    : 'content'
});
```

With the above overwritten, the response would now be:

```
{
  "isSuccess": true,
  "errorMessage" : "", // you can omit it if success is set to true
  "content": "Records filtered by the make and any previous selection",
  "items": [
    {
      "name": "White",
      "value": "1"
    },
    {
      "name": "Black",
      "value": "2"
    },
    {
      "name": "Yellow",
      "value": "3"
    },
    {
      "name": "Blue",
      "value": "4"
    },
    {
      "name": "Green",
      "value": "5"
    },
    {
      "name": "Red",
      "value": "6"
    }
  ]
}
```

## Content replacement

With each select, apart from fetching new set of `option`s for the next drop-down, you can also update the content of some container.

The value for the container is passed with the json response with the default index of `replacement`, which can obviously be overwritten (see above).

To do this, you need to add the `data-replacement` attribute to each `select` tag that you want to update the container with the value corresponding to container's `data-replacement-container` attribute.

```
<select
    name="category"
    class="cascadingDropDown"

    data-group="product-1"
    data-target="make"
    data-url="data/make.json"
    data-replacement="container1"

    >
    <option value="">Select category</option>
    <option value="1">Shoes</option>
    <option value="2">T-shirts</option>
    <option value="3">Jeans</option>
    <option value="4">Hats</option>
    <option value="5">Belts</option>
    </select>
```

Container should also have a `data-default-content` attribute, which is to store some default message when the page first loads and when the first menu in the group selects the first (empty) option.
It should also have a `class` attribute set to `cascadingContainer`.

```
<div
    class="cascadingContainer"
    data-replacement-container="container1"
    data-default-content="To see results please make your selection using menus above."
    ></div>
```

Again, you can overwrite these attributes like so:

```
$('.cascadingDropDown').ssdCascadingDropDown({
    attrDataReplacementContainer    : 'rep-container',
    attrDataReplacementDefault      : 'def-content'
    classReplacementContainer       : 'container'
});
```

With the above, the structure of our container would now be:

```
<div
    class="container"
    data-rep-container="container1"
    data-def-content="To see results please make your selection using menus above."
    ></div>
```

The default content is placed within the container on plugin instantiation so no need for doing it manually.