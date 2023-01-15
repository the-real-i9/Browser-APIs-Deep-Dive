# Web Share API
This is used for sharing data over the web.\
This API triggers the underlying OS's sharing mechanisms.

## `navigator.canShare(data)`
This is used to test where the this data is sharable, which may not be, due to some issues whethet with the data or with the OS. Returns true otherwise false.

## `navigator.share(data)`
It must be triggered by user UI action. e.g. *Button-Click*. Resolves to a Promise

```js
// data format, it can be one or more of these
{
    title: "Title Data",
    text: "Text Data",
    url: "validUrl",
    files[]: [File,...] // "File" objects. Media files
}
// All properties are optional but at least one known data property must be specified.
```
