# provider-behavior

Behavior to provide data for components. You can use this Behavior for polymer elements which manage data from API, automatically your data is stored in a closure for state control of your application. Based on [redux](https://github.com/reactjs/redux), but only using the mono-state pattern (Singleton with sugar!).

## Usage

### Data binding from provider

```html
<provider-element auto data="{{dataFromProvider}}"></provider-element>
<ol>
  <template is="dom-repeat" items="[[dataFromProvider]]">
    <li>[[item.value]]</li>
  </template>
</ol>
```
> provider-element is using the provider-behavior, it automatically gets data from the ```iron-ajax``` element and sets result in its data property.

### Using a provider method from an external element and reflecting the updated data

```html
<dom-module id="provider-element">
  <template>
    <iron-ajax
      id="api"
      url="assets/data.json"
      handle-as="json">
    </iron-ajax>
  </template>

  <script>
    Polymer({
      is: "provider-element",

      behaviors: [Polymer.ProviderBehavior],

      add: function (newItem, reflect) {
        this.set(this.data.concat([newItem]), reflect || false);
      }
    });
  </script>
</dom-module>
```

> You can set data and update all providers which are using the same data in the store. If you want to get that, only pass a second param to `true` as value.

```html
<dom-module id="smart-component">
  <template>
    <provider-element id="provider" auto data="{{dataFromProvider}}"></provider-element>
    <dumb-component collection="[[dataFromProvider]]" on-add="addElement" on-add-reflect="addAndReflect"></dumb-component>
  </template>

  <script>
    Polymer({
      is: "smart-component",

      addElement: function (e) {
        this.$.provider.add(e.detail);
      },

      addAndReflect: function (e) {
        this.$.provider.add(e.detail, true);
      }

    });
  </script>
</dom-module>
```
> provider-element contains an add method to add new data in the store and reflects this change to all providers which are using this data.

## Install the Polymer-CLI

First, make sure you have the [Polymer CLI](https://www.npmjs.com/package/polymer-cli) installed. Then run `polymer serve -o ` to serve your application locally.
