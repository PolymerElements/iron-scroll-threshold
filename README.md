[![Published on NPM](https://img.shields.io/npm/v/@polymer/iron-scroll-threshold.svg)](https://www.npmjs.com/package/@polymer/iron-scroll-threshold)
[![Build status](https://travis-ci.org/PolymerElements/iron-scroll-threshold.svg?branch=master)](https://travis-ci.org/PolymerElements/iron-scroll-threshold)
[![Published on webcomponents.org](https://img.shields.io/badge/webcomponents.org-published-blue.svg)](https://webcomponents.org/element/@polymer/iron-scroll-threshold)

## &lt;iron-scroll-threshold&gt;

`iron-scroll-threshold` is a utility element that listens for `scroll` events from a
scrollable region and fires events to indicate when the scroller has reached a pre-defined
limit, specified in pixels from the upper and lower bounds of the scrollable region.
This element may wrap a scrollable region and will listen for `scroll` events bubbling
through it from its children.  In this case, care should be taken that only one scrollable
region with the same orientation as this element is contained within. Alternatively,
the `scrollTarget` property can be set/bound to a non-child scrollable region, from which
it will listen for events.

Once a threshold has been reached, a `lower-threshold` or `upper-threshold` event will
be fired, at which point the user may perform actions such as lazily-loading more data
to be displayed. After any work is done, the user must then clear the threshold by
calling the `clearTriggers` method on this element, after which it will
begin listening again for the scroll position to reach the threshold again assuming
the content in the scrollable region has grown. If the user no longer wishes to receive
events (e.g. all data has been exhausted), the threshold property in question (e.g.
`lowerThreshold`) may be set to a falsy value to disable events and clear the associated
triggered property.

See: [Documentation](https://www.webcomponents.org/element/@polymer/iron-scroll-threshold),
  [Demo](https://www.webcomponents.org/element/@polymer/iron-scroll-threshold/demo/demo/index.html).

## Usage

### Installation
```
npm install --save @polymer/iron-scroll-threshold
```

### In an html file
```html
<html>
  <head>
    <script type="module">
      import '@polymer/iron-scroll-threshold/iron-scroll-threshold.js';
    </script>
  </head>
  <body>
    <iron-scroll-threshold id="ironScrollTheshold">
      <div>content</div>
    </iron-scroll-threshold>

    <script>
      const ironScrollTheshold = document.querySelector('#ironScrollTheshold');
      ironScrollTheshold.addEventListener('lower-threshold', () => {
        console.log('lower-threshold triggered');
        // load async stuff. e.g. XHR
        setTimeout(() => {
          ironScrollTheshold.clearTriggers();
        });
      });
    </script>
  </body>
</html>
```
### In a Polymer 3 element
```js
import {PolymerElement, html} from '@polymer/polymer';
import '@polymer/iron-scroll-threshold/iron-scroll-threshold.js';

class SampleElement extends PolymerElement {
  static get template() {
    return html`
      <iron-scroll-threshold id="ironScrollTheshold" on-lower-threshold="_loadMoreData">
        <div>content</div>
      </iron-scroll-threshold>
    `;
  }

  _loadMoreData: function() {
    console.log('lower-threshold triggered');
    // load async stuff. e.g. XHR
    setTimeout(() => {
      this.$.ironScrollTheshold.clearTriggers();
    });
  }
}
customElements.define('sample-element', SampleElement);
```

## Contributing
If you want to send a PR to this element, here are
the instructions for running the tests and demo locally:

### Installation
```sh
git clone https://github.com/PolymerElements/iron-scroll-threshold
cd iron-scroll-threshold
npm install
npm install -g polymer-cli
```

### Running the demo locally
```sh
polymer serve --npm
open http://127.0.0.1:<port>/demo/
```

### Running the tests
```sh
polymer test --npm
```
