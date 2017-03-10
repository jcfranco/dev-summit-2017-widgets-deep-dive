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
- Development challenges
- Widget API Design Tips
- Porting 3.x => 4.x tips
- Tools we use
- Resources
- Q & A

---

# Prereqs

- [Accessor](https://developers.arcgis.com/javascript/latest/guide/implementing-accessor/index.html) ('esri/core/Accessor`)
- TypeScript

---

# Prereqs: TypeScript

- Leverage ES6 (syntactic sugar)
- Interfaces
- Typing
- const and let vs var
- remove binding for () => {}
- [TypeScript Setup](https://developers.arcgis.com/javascript/latest/guide/typescript-setup/index.html)

---

# How we got here

- Dijit to now
  - 3.x: Dijit
  - 4.x: Abstracted!
- Why?
    - todo: gains
    - API Consistency

<img src="images/dos.gif" width="300">

---

# Our development lifecycle

How do we go about developing widgets?

---

# Development lifecycle

- Request/idea/port
- API design and approval
- Kickoff UI/UX design
- Develop ViewModel
- Develop View
- Write tests
- Pull request
- Pull request reviewed
- API merge!!!

---

# Development challenges

![challenges](images/challenges.gif)

---

# Widget API Design Tips

todo: matt

- things to consider for collections/accessor

Go through all the things we do to port a widget to 4x.

Let's go deep!

![tips](images/tips.gif)

---

- rethink APIs
- restrucuring
- porting locate/track
- porting search
- portiong home

---

# Our Tools

---

# Tasks

todo: jc? do we need this?

- installing npm/grunt
- compile ts/sass

---

# Porting: Real Nodes

todo: matt

---

# Design Tips: JSX

- focusing nodes
  - storing reference to them
- Using JSX to hide nodes
  - toggling classes
  - joining classes
  - using es6 templates
  - JSX key attribute
  - accessibleHandler
  - JSX storing data on attributes

---

#  Design Tips: ViewModels

- Rethinking APIs
  - More collections
  - More Accessors
  - View properties instead of events
  - readonly properties
- Support modules
  - Offloading logic where appropriate. More modular
- Autocasting

---

#  Design Tips: Styling

- CSS to Sass
  - Variables
  - Theming
  - Mixins
  - etc.
- BEM
- Icon font / SVG
- Organized class names, CSS object

todo: matt - cover BEM and CSS object.

---

# Tools

- IDEs
- NPM
- Grunt

---

# Tools: IDEs

- Visual Studio Code
- WebStorm

---

# Tools: Node

- Node
- NPM

---

# Tools: Tasks

- Grunt

---

## Additional Resources

- [JavaScript Sessions at DevSummit](https://devsummit.schedule.esri.com/#search/sessions/q:javascript)
- [Documentation - 4.3](https://developers.arcgis.com/javascript/)

---

# Use the source luke

[GitHub Code](http://esriurl.com/deepdive)

---

# Please Take Our Survey!

1. Download the Esri Events app and go to DevSummit
2. Select the session you attended
3. Scroll down to the "Feedback" section
4. Complete Answers, add a Comment, and Select "Submit"

<img src="images/submit-feedback.png" width="200">

---

# Questions?

---

![Thank you!](images/thanks.gif)

---

<!-- .slide: data-background="../reveal.js/img/end.png" -->
