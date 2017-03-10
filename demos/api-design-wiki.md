## Objective

Develop a new responsive Popup experience that will replace the current ArcGIS JS API popup. The new Popup widget will respond to small resolution devices, provide a better user experience when used in small maps, use an updated visual design, be easier to theme, provide a better DOM structure, use scalable graphics, use smart mapping colors,  etc.

### Popups Must Include:

1. Title
2. Description (drop in any attribute)
3. Field Value (both 2 column tables and definition lists)
4. Media (picts, graphs, multiples, gallery of elements, note that these are not hosted)
5. Attachments (acts as a catch all and can include anything including picts, videos, documents, etc., note that these are hosted).
6. Ability to navigate from one attribute to another (met with Paul Barker on this).
7. Ability to edit a popup
8. Ability to search/filter/sort popups contents
9. Ability to configure popup

## Issues
arcgis-js-api/issues/1852
arcgis-js-api/issues/1244
 
## Stakeholders

Matt Driscoll, Kelly Hutchins, Dasa Paddock, Bjorn Svensson, Praveen Ponnusamy, Jeremy Bartley, Heath Meyette, Ganesh Subbiah

## Wireframes

[Wireframes on BOX](#)

![Click flow chart](#)

## Demos

- Demo Link

## Constructor
Popup(options, srcNode);

### Constructor options
See [Properties](#properties).

## Properties
|property|type|description|readonly|default|
|---|---|---|---|---|---|
|features|Array|Array of features||[]|
|promises|Array|Array of promises||[]|
|actions|Array|Array of objects||[{ id: "zoom-in", title: "", className:"", action: function() }]|
|featureCount|Integer|Number of features|true|0|
|visible|Boolean|Show popup for selected feature||false|
|selectedIndex|Integer|Index of feature that is selected. When features are set, the first index is selected automatically.||null|
|selectedFeature|Feature|Feature that is selected||null|
|zoomFactor|Integer|Number factor to zoom||4|
|paginationEnabled|Boolean|Show pagination for popup if available||true|
|alignment|String|Position of the popup||"top", "bottom", "left", "right"|
|animationOptions|Object|Animation easing options||{duration: 300, easing: easing.quadInOut}|
|autoRepositionEnabled|Boolean|Reposition the popup when it opens outside of the view||true|
|dockButtonVisible|Boolean|Allow dockable popup||true|
|loaded|Boolean|Is the widget loaded||false|
|title|String|Title string||null|
|content|String or HTML Element|content string or element node||null|
|location|Geometry|Geometry used to position the popup||null|
|pendingPromisesCount|Number|Number of promises remaining to be resolved|true|0|
|docked|Boolean|Docked state||false|
|dockOptions|Object|Docked settings||{autoDock: true,dockButtonEnabled: true,position: {portrait: "bottom",landscape: "right"},dockAtSize: {width: 480,height: 640}}|
|updateLocationEnabled|Boolean|Update the location when the selectedIndex changes||false|
|closestFirst|Boolean|Show closest features first||false|

## Methods

### startup

startup(): Start the widget.

### destroy

destroy(): Destroy the widget.

### next

### previous

## Events

#### action-click

```javascript
on("action-click", function(action){
  if(action.id === “my-action”{
    // do stuff
  }
});
```

## Examples

```javascript
popup.set({
  location: v.extent.clone().center,
  title: "Testing Title",
  content: "Testing Content",
  visible: true,
  features: null
});

popup.set({
  visible: true,
  features: [Graphic,Graphic,...]
});

popup.set({
  visible: true,
  promises: [promise,promise,...]
});

popup.set({
  location: v.extent.clone().center,
  title: "Testing Title Only",
  content: null,
  visible: true,
  actions: null,
  features: null
});

popup.set({
  visible: true,
  features: [Graphic,...],
  updateLocationEnabled: true
});
```

## Discussion Topics

### Questions

- Do we want a priority on the PopupTemplate?

- Need map to handle RTL

### Resolved Questions

- How do we handle promises and the loading state of features?
   
  > Popup handles promises

- Should we use "show" or "visible" property for displaying popup?

 > Visible

- Do we need "visibleWhenEmpty" property currently on widget?

 > No

- What should the "expanded" state be called? docked? expanded?

 > Docked

- Should we have the "docked" be another widget called "mapPanel"? Al L. requested a widget that would be a map panel.

 > No map panel for now

- For zoomFactor should we zoom the map's scale or zoom level to apply the factor to? Currently, it's level by a factor of 4.

 > Keep zoom level

- Should the user place the DIV for the popup or should we handle it and place it into a view? Where should it go?

 > Keep inside view. not in UI

- Should we have breakpoints for making the popup smaller depending on the map size or just one breakpoint to switch to the docked/expanded design?

 > Switch to docked at 480 and below. Use breakpoints for modifying width in other sizes. Use Bootstrap breakpoints.

- Do we need the zoom in icon on the popup? What if we had Plus and Minus buttons on the popup instead of the zoom magnifying glass?

 > Keep for now.

- Layer needs a better way to disable popup. Can only do it online currently.

  > Submitted issue: arcgis-js-api/issues/2460

- Popup higlighting graphic
  - where should the graphics be added since there is no map.graphics anymore?

   > Yann will work on this.

  - Is there a way to keep them on top always?

   > Should be

- PopupRenderer
  - What charting should the popup renderer use?

   > Dojo charts for now. Talk to Dan O'leary if necessary.

- When a popup has multiple features and the user pages to the next feature, do we move the popup to that new feature's geometry?
   - 3X does not move no matter what. User sets a location and all features point to that location.
   - My sample moved the popup to the center of the selected feature's geometry.

> Add autoUpdateLocation option to have both options. 

- How do we handle ordering the features?
  - What class sends the popup the deferreds. Shouldn't that class handle ordering?
  - 3x Popup had a function showClosestFirst(), do we need it?
  - Can we return the order of features depending on the feature type? Point -> Line -> Polygon.

> Need showClosestFirst back. Check old popup base. Orders closest feature within layer.
> PopupTemplate needs a priority and PopupManager will sort promises order by priority. This takes precedence over layer toc order.

- How can we handle the feedback class? We need the popup manager to tell this class if the request is async or sync and then tell it when it's done if async.

> View needs a way to have a loading spinner and feedback click like it has a popup. So the widget class will be associated to a view.

- PopupTemplate
  - Can we use content instead of description? Currently it's `new PopupTemplate({title: "", description: ""});`

> Should use popup.template. Need to implement according to issue.

## Pending

- Multiple popup elements. [WebGIS/webmap-spec#73](webmap-spec/issues/73)

-  Design Enhancements for Related Records experience in Popups

  (a) Provide the option to view related records in the popup window itself, rather than opening the attribute table.   
      **Issues**  
      [1](arcgis-portal-app/issues/3713)
      [2](arcgis-portal-app/issues/3159) 

  (b) Provide ability to navigate Related Records from popup  
      **issues**:   
     [1](arcgis-portal-app/issues/3158) 

## Notes

### 5.5.2015
- Look at popup renderer
  - Implementation area (how much widget vs renderer)
- Popup is on view
- Named "popupTemplate" not "infoTemplate".
- Named "popup" not "infoWindow".
- Tooltip named "tooltipTemplate".
- Graphic has tooltipTemplate and popupTemplate on it.
- popupTemplate has
  - elements
  - content
  - title
  - template
- Two classes: popup, popupTemplate
- Merge infoTemplate & popupTemplate