<!-- .slide: data-background="../reveal.js/img/title.png" class="center" -->

# Deep Dive on How ArcGIS API for JavaScript Widgets Were Built

### Matt Driscoll – [@driskull](https://twitter.com/driskull)
### JC Franco – [@arfncode](https://twitter.com/arfncode)

---

# Agenda

- History
- Accessor
- TypeScript
- Lifecycle
- Porting
- Tools
- Resources
- Q & A

---

# History

- Dijit
- Why we changed

---

# Accessor

---

# TypeScript

- Leverage ES6 (syntactic sugar)
- Interfaces
- Typing
- const and let vs var
- remove binding for () => {}

---

# Internal development lifecycle

---

# Porting

---

# Porting: Real Nodes

---


# Porting: JSX

- focussing nodes
  - storing reference to them
- Using JSX to hide nodes
  - toggling classes
  - joining classes
  - using es6 templates
  - JSX key attribute
  - accessibleHandler
  - JSX storing data on attributes

---

# Porting: ViewModels

- Rethinking APIs
  - More collections
  - More Accessors
  - View properties instead of events
  - readonly properties
- Support modules
  - Offloading logic where appropriate. More modular
- Autocasting

---

# Porting: Styling

---

# Tools

- IDEs
- NPM
- Grunt

---

## Additional Resources

- [JavaScript Sessions at DevSummit](https://devsummit.schedule.esri.com/#search/sessions/q:javascript)
- [Documentation - 4.0 beta](https://developers.arcgis.com/javascript/beta/)

---

# Use the source luke

[GitHub Code](https://github.com/jcfranco/dev-summit-2017-widgets-deep-dive)

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
