<!-- .slide: data-background="../reveal.js/img/title.png" class="center" -->

# Deep Dive on How ArcGIS API for JavaScript Widgets Were Built

### Matt Driscoll – [@driskull](https://twitter.com/driskull)
### JC Franco – [@arfncode](https://twitter.com/arfncode)

![Deep Dive](images/deepdive.gif)

---

# Agenda

- Prerequisites
- How we got here
- Our development lifecycle
- Widget development tips
- Tools we use
- Resources
- Q & A

---

# Prereqs: Accessor

- [Accessor SDK](https://developers.arcgis.com/javascript/latest/guide/implementing-accessor/index.html) `esri/core/Accessor`

---

# Prereqs: TypeScript

- Leverage ES6 (syntactic sugar)
- Interfaces
- Typing
- `const` and `let` vs `var`
- `() => {}` vs `bind` or this-binding utility
- [TypeScript Setup](https://developers.arcgis.com/javascript/latest/guide/typescript-setup/index.html)

---

# How we got here

- 3.x
  - Dojo Dijit
  - Dijit Themes
  - Logic tied to UI
- 4.x
  - Abstracted & framework independent
  - ViewModels

---

# Why?

- Framework independent
- Easily customizable themes
- Responsive design
- Redesigned Widget API
  - Consistent with core API

---

# Our development lifecycle

How do we go about developing widgets?

![lifecycle](images/lifecycle.gif)

---

# Development lifecycle

- API design
- Kickoff UI/UX design
- Develop ViewModel
- Develop View
- Write tests
- Pull request
- API merge!!!

---

# Development lifecycle: API Design

- Widget developer writes objective for widget
  - [Sample](https://github.com/jcfranco/dev-summit-2017-widgets-deep-dive/blob/master/demos/api-design-wiki.md)
- Widget dev defines API in markdown
  - View & ViewModel
    - Properties
    - Methods
    - Events
  - Sample code snippets
  - Demos
  - Q & A
- API reviewed and tweaked
- JS doc written and approved

---

# Development lifecycle: ViewModel

- Friendly, consistent naming
- Public methods
  - Return types
  - Arguments
- Public properties

---

# Development lifecycle: View

- Research DOM structure needed for widget
  - Layout containers needed
  - Using proper semantic tags for nodes
- CSS lookup object used in `render()`
- Accessible, Aria roles present if necessary
- Properties, events, methods aliased as necessary

---

# Development lifecycle: Styles

- Classes needed
- BEM naming of classes
- 4x Widgets can use flexbox for layout

---

# Development lifecycle: UI/UX Design

- Meeting with our creative lab
- Discuss needs, API
- Collaborate on design and tweak markup as necessary
- Receive mockup/wireframes/assets/Sass
- Implement design

---

# Development lifecycle: Tests

- Make sure we have tests that hit all the API
- Unit, integration, functional, screenshot tests
- Methods are tested with all options and return types
- Assert properties behave as expected when modified
- Screenshot tests
- Test early

---

# Development lifecycle: Pull Request

- All the code changes done in a git branch
- PR is opened with all changes and tests included
- PR is reviewed and tested
- API build is successful
- Merge!

![merge](images/merge.gif)

---

# Development tips

![challenges](images/challenges.gif)

---

# Development tips: API Design

How things can be done differently in 4 compared to 3

- Leverage
  - Collection
  - Accessor
- View properties instead of events
  - Read-only properties
- Promises for async operations
- Support modules
  - Offloading logic where appropriate. More modular.

---

# Development tips: `render()`

- Hiding nodes

```
render() {
  const childNode = this.childVisible? <div>child</div> : null;

  return (
    <div>{childNode}</div>
  );
}
```

---

# Development tips: `render()`

- Reusing classes

```
const CSS = {
  root: "example",
  part: "example__part",
  disabled: "example--disabled"
};
```

---

# Development tips: `render()`

- Toggling classes

```
render() {
  const dynamicClasses = {
    [CSS.disabled]: this.isDisabled
  };

  return (
    <div classes={dynamicClasses}>...</div>
  );
}
```

---

# Development tips: `render()`

- `class` cannot change between renders

```
render() {
  const rootClass = someCondition ? CSS.foo : CSS.bar;

  // throws error - cannot change class
  return (
    <div class={rootClass}>...</div>
  );
}
```

---

# Development tips: `render()`

- Use `join` to apply multiple classes

```
render() {
  return (
    <div class={join(CSS.root, CSS.button, CSS.shadow)}>...</div>
  );
}
```

---

# Development tips: `render()`

- Toggle styles (similar to `classes`)

```
render() {
  const dynamicStyles = {
    "x": getX(),
    "y": getY()
  };

  return (
    <div styles={dynamicStyles}>...</div>
  );
}
```

---

# Development tips: `render()`

- Attributes are not removed between render calls

```
render() {
  const tabIndex = this.focusable ? 0 : null;

  // `tabIndex` attribute will get rendered when value is falsy
  return (
    <div tabIndex={tabIndex}>...</div>
  );
}
```

---

# Development tips: `render()`

- String templates!

```
render() {
  return (
    <div class={CSS.root}>`Hello, ${this.person.name}!`</div>
  );
}
```

---

# Development tips: `render()`

- Distinguishable children

```
render() {

  // children are NOT dynamically added/removed, `key` is NOT needed
  return (
    <div>
      <div>foo</div>
      <div>bar</div>
    </div>
  );
}
```

---

# Development tips: `render()`

- Distinguishable children

```
render() {
  const foo = this.showFoo? <div key="foo">foo</div> : null;
  const bar = this.showBar? <div key="bar">bar</div> : null;

  // children are dynamically added/removed, `key` IS needed
  return (
    <div>
      {foo}
      {bar}
    </div>
  );
}
```

**Note**: `key` can be a string, number or object

---

# Development tips: `render()`

- Storing data on attributes

```
render() {
  return (
    <div onclick={this._handleClick}
         data-lucky-numbers={luckyNumbers()}>Lucky numbers!</div>
  );
}

private _handleClick(event: MouseEvent) {
  const node = event.currentTarget as Element;
  const luckyNums = node.getAttribute("data-lucky-numbers");
  console.log(`Today's lucky numbers are: ${luckyNums}`);
}
```

---

# Development tips: `render()`

- Binding

```
render() {
  return (
    <div class={CSS.base}>
      <div onclick={this._logThis}>this === node</div>

      <div bind={this}
           onclick={this._logThis}>this === widget</div>
    </div>
  );
}
```

---

# Development tips: `render()`

- DOM events

```
render() {
  return (
    <div class={CSS.base}>
      <img onclick={this._handleClickEvent} />
    </div>
  );
}

private _handleClickEvent(event: MouseEvent) {
  // do something with event
}
```

---

# Development tips: Real Nodes

- Focusing nodes
  - Markup in `render()` is virtual
  - Need to store reference to actual node with `afterCreate` or `afterUpdate`

---

# Development tips: `render()`

- Real nodes

```
private _realNode: Element = null;

render() {
  return (
    <div afterCreate={this._storeThisNode}>...</div>;
  )
}

private _storeThisNode(node: Element): void {
  this._realNode = node;
}
```

---

# Development tips: `render()`

- accessibleHandler

```
render() {
  return (
    <div onclick={this._doSomething}
         onkeydown={this._doSomething} />
  );
}

@accessibleHandler()
private _doSomething(): void {
  // ...
}
```

---

# Development tips: `render()`

- Close childless node tags for conciseness

```
render() {
  return (
    <div>
      <div class={CSS.childless} />
    </div>
  );
}
```

---

# Development tips: `render()`

- Keep `render` manageable by extracting pieces as it grows

```
render() {
  return <div>{this._renderContent()}</div>;
}

private _renderContent(): any {
  return (
    <div>
      <h1>{this.title}</h1>
      {this._renderItems()}
    </div>
  );
}
```

---

#  Development tips: ViewModels

- Rethinking APIs
  - More collections
  - More Accessors
  - View properties instead of events
  - Read-only properties
- Support modules
  - Offloading logic where appropriate. More modular
- Autocasting

---

# Development tips: Styling

- CSS to Sass
  - Variables
  - Theming
  - Mixins
- [SDK Guide: Styles](https://developers.arcgis.com/javascript/latest/guide/styling/index.html)
- [Sass](http://sass-lang.com/)
- [BEM](http://getbem.com/)
- [Icon font](https://developers.arcgis.com/javascript/latest/guide/esri-icon-font/index.html) / SVG

---

# Widget Theming: Sass

- CSS preprocessor
- Variables
- `@mixin` (group statements)
- `@include` - (use mixins)
- `@import` - (split up files)
- `@extend` - (inheritance)
- More power!

---

# Sass Install

- [Installing Sass](http://sass-lang.com/install)

```
sudo gem install sass
```

---

# Widget BEM

- [BEM](http://getbem.com/): Block Element Modifier
- Methodology to create reusable components
- Uses delimiters to separate block, element, modifiers
- Provides semantics (albeit verbose)
- Keeps specificity low
- Scopes styles to blocks

```css
/* block */
.example-widget {}

/* block__element */
.example-widget__input {}
.example-widget__submit {}

/* block--modifier */
.example-widget--loading {}

/* block__element--modifier */
.example-widget__submit--disabled {}
```

---

# Development tips: Styling within View

CSS lookup object

```
const CSS = {
  base: "my-widget",
  title: "my-widget__title"
};
```

Lookup referenced in JSX

```
<div class={CSS.base}/>
  <h1 class={CSS.title}>Hello world</h1>
</div>
```

---

# Our Tools

- IDEs
- Tasks
- Other

![Tasks](images/tools.gif)

---

# Tools: IDEs

- Multiple flavors
  - Visual Studio Code
  - WebStorm
  - [and more...](https://www.typescriptlang.org/)
- Plugins

---

# Tools: Tasks

- Node/npm
- Installing Grunt
- Compile TS/Sass

![Tasks](images/tasks.gif)

---

# Other tools

Besides an IDE and browser dev tools...

- SourceTree
- Terminal
- GitHub Enterprise
- Slack :D
- A handful of browsers
- Coffee

---

## Additional Resources

- [4x widget snippets](https://github.com/jcfranco/4x-widget-snippets/)
- [JavaScript Sessions at DevSummit](https://devsummit.schedule.esri.com/#search/sessions/q:javascript)
- [Documentation - 4.3](https://developers.arcgis.com/javascript/)

![idea](images/idea.gif)

---

# Find this on GitHub

[GitHub Code](http://esriurl.com/deepdive)

[![Source Code](images/code.png)](http://esriurl.com/deepdive)

---

# Please Take Our Survey!

1. Download the Esri Events app and go to DevSummit
2. Select the session you attended
3. Scroll down to the "Feedback" section
4. Complete Answers, add a Comment, and Select "Submit"

<img src="images/rate.gif" width="400">

---

# Questions?

<img src="images/questions.gif" width="400">

---

![Thank you!](images/thanks.gif)

---

<!-- .slide: data-background="../reveal.js/img/end.png" -->
