<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

## Canvas

You can customize the initial state of the module from the editor initialization, by passing the following [Configuration Object][1]

```js
const editor = grapesjs.init({
 canvas: {
   // options
 }
})
```

Once the editor is instantiated you can use its API and listen to its events. Before using these methods, you should get the module from the instance.

```js
// Listen to events
editor.on('canvas:drop', () => { ... });

// Use the API
const canvas = editor.Canvas;
canvas.setCoords(...);
```

## Available Events
* `canvas:dragenter` Something is dragged inside the canvas, `DataTransfer` instance passed as an argument.

* `canvas:dragover` Something is dragging on the canvas, `DataTransfer` instance passed as an argument.

* `canvas:dragend` When a drag operation is ended, `DataTransfer` instance passed as an argument.

* `canvas:dragdata` On any dataTransfer parse, `DataTransfer` instance and the `result` are passed as arguments.&#xA;By changing `result.content` you're able to customize what is dropped.

* `canvas:drop` Something is dropped in canvas, `DataTransfer` instance and the dropped model are passed as arguments.

* `canvas:spot` Spots updated.

```javascript
editor.on('canvas:spot', () => {
 console.log('Spots', editor.Canvas.getSpots());
});
```

* `canvas:spot:add` New canvas spot added.

```javascript
editor.on('canvas:spot:add', ({ spot }) => {
 console.log('Spot added', spot);
});
```

* `canvas:spot:update` Canvas spot updated.

```javascript
editor.on('canvas:spot:update', ({ spot }) => {
 console.log('Spot updated', spot);
});
```

* `canvas:spot:remove` Canvas spot removed.

```javascript
editor.on('canvas:spot:remove', ({ spot }) => {
 console.log('Spot removed', spot);
});
```

* `canvas:coords` Canvas coordinates updated.

```javascript
editor.on('canvas:coords', () => {
 console.log('Canvas coordinates updated:', editor.Canvas.getCoords());
});
```

* `canvas:zoom` Canvas zoom updated.

```javascript
editor.on('canvas:zoom', () => {
 console.log('Canvas zoom updated:', editor.Canvas.getZoom());
});
```

* `canvas:pointer` Canvas pointer updated.

```javascript
editor.on('canvas:pointer', () => {
 console.log('Canvas pointer updated:', editor.Canvas.getPointer());
});
```

* `canvas:frame:load` Frame loaded in canvas.&#xA;The event is triggered right after iframe's `onload`.

```javascript
editor.on('canvas:frame:load', ({ window }) => {
 console.log('Frame loaded', window);
});
```

* `canvas:frame:load:head` Frame head loaded in canvas.&#xA;The event is triggered right after iframe's finished to load the head elemenets (eg. scripts)

```javascript
editor.on('canvas:frame:load:head', ({ window }) => {
 console.log('Frame head loaded', window);
});
```

* `canvas:frame:load:body` Frame body loaded in canvas.&#xA;The event is triggered when the body is rendered with components.

```javascript
editor.on('canvas:frame:load:body', ({ window }) => {
 console.log('Frame completed the body render', window);
});
```

[Component]: component.html

[Frame]: frame.html

[CanvasSpot]: canvas_spot.html

## getConfig

Get configuration object

Returns **[Object][2]** 

## getElement

Get the canvas element

Returns **[HTMLElement][3]** 

## getFrameEl

Get the main frame element of the canvas

Returns **[HTMLIFrameElement][4]** 

## getWindow

Get the main frame window instance

Returns **[Window][5]** 

## getDocument

Get the main frame document element

Returns **HTMLDocument** 

## getBody

Get the main frame body element

Returns **[HTMLBodyElement][6]** 

## setCustomBadgeLabel

Set custom badge naming strategy

### Parameters

*   `f` **[Function][7]** 

### Examples

```javascript
canvas.setCustomBadgeLabel(function(component){
 return component.getName();
});
```

## getRect

Get canvas rectangular data

Returns **[Object][2]** 

## hasFocus

Check if the canvas is focused

Returns **[Boolean][8]** 

## scrollTo

Scroll canvas to the element if it's not visible. The scrolling is
executed via `scrollIntoView` API and options of this method are
passed to it. For instance, you can scroll smoothly by using
`{ behavior: 'smooth' }`.

### Parameters

*   `el` **([HTMLElement][3] | [Component])** 
*   `opts` **[Object][2]** Options, same as options for `scrollIntoView` (optional, default `{}`)

    *   `opts.force` **[Boolean][8]** Force the scroll, even if the element is already visible (optional, default `false`)

### Examples

```javascript
const selected = editor.getSelected();
// Scroll smoothly (this behavior can be polyfilled)
canvas.scrollTo(selected, { behavior: 'smooth' });
// Force the scroll, even if the element is alredy visible
canvas.scrollTo(selected, { force: true });
```

## setZoom

Set canvas zoom value

### Parameters

*   `value` **[Number][9]** The zoom value, from 0 to 100

### Examples

```javascript
canvas.setZoom(50); // set zoom to 50%
```

Returns **this** 

## getZoom

Get canvas zoom value

### Examples

```javascript
canvas.setZoom(50); // set zoom to 50%
const zoom = canvas.getZoom(); // 50
```

Returns **[Number][9]** 

## setCoords

Set canvas position coordinates

### Parameters

*   `x` **[Number][9]** Horizontal position
*   `y` **[Number][9]** Vertical position
*   `opts` **ToWorldOption**  (optional, default `{}`)

### Examples

```javascript
canvas.setCoords(100, 100);
```

Returns **this** 

## getCoords

Get canvas position coordinates

### Examples

```javascript
canvas.setCoords(100, 100);
const coords = canvas.getCoords();
// { x: 100, y: 100 }
```

Returns **[Object][2]** Object containing coordinates

## getLastDragResult

Get the last created Component from a drag & drop to the canvas.

Returns **([Component] | [undefined][10])** 

## addSpot

Add or update canvas spot.

### Parameters

*   `props` **[Object][2]** Canvas spot properties.
*   `opts` **AddOptions**  (optional, default `{}`)

### Examples

```javascript
// Add new canvas spot
const spot = canvas.addSpot({
 type: 'select', // 'select' is one of the built-in spots
 component: editor.getSelected(),
});

// Add custom canvas spot
const spot = canvas.addSpot({
 type: 'my-custom-spot',
 component: editor.getSelected(),
});
// Update the same spot by reusing its ID
canvas.addSpot({
 id: spot.id,
 component: anotherComponent,
});
```

Returns **[CanvasSpot]** 

## getSpots

Get canvas spots.

### Parameters

*   `spotProps` **[Object][2]?** Canvas spot properties for filtering the result. With no properties, all available spots will be returned. (optional, default `{}`)

### Examples

```javascript
canvas.addSpot({ type: 'select', component: cmp1 });
canvas.addSpot({ type: 'select', component: cmp2 });
canvas.addSpot({ type: 'target', component: cmp3 });

// Get all spots
const allSpots = canvas.getSpots();
allSpots.length; // 3

// Get all 'select' spots
const allSelectSpots = canvas.getSpots({ type: 'select' });
allSelectSpots.length; // 2
```

Returns **[Array][11]<[CanvasSpot]>** 

## removeSpots

Remove canvas spots.

### Parameters

*   `spotProps` **([Object][2] | [Array][11]<[CanvasSpot]>)?** Canvas spot properties for filtering spots to remove or an array of spots to remove. With no properties, all available spots will be removed. (optional, default `{}`)

### Examples

```javascript
canvas.addSpot({ type: 'select', component: cmp1 });
canvas.addSpot({ type: 'select', component: cmp2 });
canvas.addSpot({ type: 'target', component: cmp3 });

// Remove all 'select' spots
canvas.removeSpots({ type: 'select' });

// Remove spots by an array of canvas spots
const filteredSpots = canvas.getSpots().filter(spot => myCustomCondition);
canvas.removeSpots(filteredSpots);

// Remove all spots
canvas.removeSpots();
```

Returns **[Array][11]<[CanvasSpot]>** 

## hasCustomSpot

Check if the built-in canvas spot has a declared custom rendering.

### Parameters

*   `type` **[String][12]** Built-in canvas spot type

### Examples

```javascript
grapesjs.init({
 // ...
 canvas: {
   // avoid rendering the built-in 'target' canvas spot
   customSpots: { target: true }
 }
});
// ...
canvas.hasCustomSpot('select'); // false
canvas.hasCustomSpot('target'); // true
```

Returns **[Boolean][8]** 

## getWorldRectToScreen

Transform a box rect from the world coordinate system to the screen one.

### Parameters

*   `boxRect` **[Object][2]** 

Returns **[Object][2]** 

[1]: https://github.com/GrapesJS/grapesjs/blob/master/src/canvas/config/config.ts

[2]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[3]: https://developer.mozilla.org/docs/Web/HTML/Element

[4]: https://developer.mozilla.org/docs/Web/API/HTMLIFrameElement

[5]: https://developer.mozilla.org/docs/Web/API/Window

[6]: https://developer.mozilla.org/docs/Web/HTML/Element/body

[7]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function

[8]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean

[9]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[10]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined

[11]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array

[12]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String
