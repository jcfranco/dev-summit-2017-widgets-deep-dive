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
- Widget development challenges
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
- remove binding for `() => {}`
- [TypeScript Setup](https://developers.arcgis.com/javascript/latest/guide/typescript-setup/index.html)

---

# How we got here

- 3.x
  - Dojo Dijit
  - Dijit Themes
- 4.x
  - Abstracted & framework independent
  - ViewModels

---

# Why?

- Framework independent
- Customizable themes
- Responsive
- Redesigned API
- consistent with core API

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
  - [Sample](pdf/popup-api.pdf)
- Widget dev defines API in markdown
  - View & ViewModel
    - Properties
    - Methods
    - Events
  - sample code snippets
  - demos
  - Q & A
- API reviewed and tweaked
- JS doc written and approved

---

# Development lifecycle: ViewModel

- friendly, consistent naming
- public methods
  - return types
  - arguments
- public properties

---

# Development lifecycle: View

- Research dom structure needed for widget
- Layout containers needed
- Using proper semantic tags for nodes
- CSS object used in JSX
- Accessible, Aria roles present if necessary
- Properties, events, methods aliased as necessary

---

# Development lifecycle: Styles

- Classes needed
- BEM naming of classes
- 4x Widgets can use flexbox for layout
- Sass mixins needed?

---

# Development lifecycle: UI/UX Design

- Meeting with our creative lab
- Discuss needs, API
- Collaborate on design and tweak JSX as necessary
- Receive mockup/wireframes/assets/Sass
- implement design

---

# Development lifecycle: Tests

- Make sure we have tests that hit all the API
- Unit, integration, functional tests
- Methods are tested with all options and return types
- Properties behave as expected when modified
- etc.
- Screenshot tests

---

# Development lifecycle: Pull Request

- All the code changes done in a git branch
- PR is opened with all changes and tests included
- PR is reviewed and tested
- API build is successful
- Merge!

---

# Development tips

![challenges](images/challenges.gif)

---

# Development tips: API Design

How things can be done differently in 4x compared to 3

- New tools
  - Collection
  - Accessor

---

# Development tips: JSX

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

# Development tips: JSX

- Reusing classes

```
const CSS = {
  root: "example",
  part: "example__part",
  disabled: "example--disabled"
};
```

---

# Development tips: JSX

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

# Development tips: JSX

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

# Development tips: JSX

- Use `join` to apply multiple classes

```
render() {
  return (
    <div class={join(CSS.root, CSS.button, CSS.shadow)}>...</div>
  );
}
```

---

# Development tips: JSX

- String templates!

```
render() {
  return (
    <div class={CSS.root}>`Hello, ${this.person.name}!`</div>
  );
}
```

---

# Development tips: JSX

- Distinguishable children

```
render() {

  // children are NOT dynamically added/removed, `key` is NOT necessary
  return (
    <div>
      <span>first</span>
      <span>second</span>
      <span>third</span>
    </div>
  );
}
```

---

# Development tips: JSX

- Distinguishable children

```
render() {
  const first = this.showFirst? <span key="first">first</span> : null;
  const second = this.showSecond? <span key="second">second</span> : null;
  const third = this.showThird? <span key="third">third</span> : null;

  // children are dynamically added/removed, `key` IS necessary
  return (
    <div>
      {first}
      {second}
      {third}
    </div>
  );
}
```

**Note**: `key` can be a string, number or object

---

# Development tips: JSX

- JSX storing data on attributes

```
render() {
  return (
    <div onclick={this._handleClick}
         data-lucky-numbers={luckyNumbers()}>
      Click for your lucky numbers!
    </div>
  );
}

private _handleClick(event: MouseEvent) {
  const node = event.currentTarget as Element;
  const luckyNums = node.getAttribute("data-lucky-numbers");
  console.log(`Today's lucky numbers are: ${luckyNums}`);
}
```

---

# Development tips: JSX

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

# Development tips: Real Nodes

- Focusing nodes
  - Markup in `render` is virtual
  - Need to store reference to actual node with `afterCreate` or `afterUpdate`

---

# Development tips: Real Nodes

```
private _realNode: Element = null;

private _storeThisNode(node: Element): void {
  this._realNode = node;
}

render() {
  return (
    <div afterCreate={this._storeThisNode}>...</div>;
  )
}
```

---

# Development tips: JSX

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

#  Development tips: ViewModels

- Rethinking APIs
  - More collections
  - More Accessors
  - View properties instead of events
  - readonly properties
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

# Development challenges: Styling within View

CSS Object

```
const CSS = {
  base: "my-widget",
  title: "my-widget__title"
};
```

Object referenced in JSX

```
<div class={CSS.base}/>
  <h1 class={CSS.title}>Hello world</h1>
</div>
```

---

# Our Tools

- IDEs
- Node/NPM
- Grunt
- Other

![Tasks](images/tools.gif)

---

# Tools: IDEs

- Visual Studio Code
  - Plugins :D
- WebStorm

---

# Tools: Tasks

- Grunt
- installing npm/grunt
- compile ts/sass

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
