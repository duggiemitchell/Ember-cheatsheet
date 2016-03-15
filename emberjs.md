**Components in Ember** provide a reusable way to combine a template with action handling behavior.

Used for:
1. Charts
2. Tabs
3. Tree Widgets
4. Buttons with confirmations
5. Date range selectors
6. Wrap libraries and services

They follow very closely with W3C Custom Elements specification.  This is something to be super-excited about given web components are on their way (or already available in) to your favorite browser.  What makes them truly amazing for my fellow Frontend developers is they completely encapsulate all of their HTML and CSS; meaning the styles you write will render as intended and handles the whole "separation of concerns" with external Javascript.  Of you have not read up or played around with Web Components, I recommend trying [Component Kitchen](https://component.kitchen/) for a gentle introduction. 
##### Generating a Component Using `Ember CLI`:
  ```javascript
    $ ember generate component <component-name>
  ```
  Component are defined in `app/components` and extend from `Ember.Component`.  Within the definition, a hyphen must be used in the namespace to differentiate them from standard HTML tags and conform to web component specs.

  ##### Defining the Component Logic
  Components have their own properties, functions, and behaviors.
  ```javascript
    export default Ember.Component.extend({
      percentage: Ember.computed('itemPrice', 'orderPrice', function() {
        return this.get('itemPrice') / this.get('orderPrice') ** 100'
    })
  });
  ```
##### Customizing the Component's Template:

Defined in `app/templates/components/template-name.hbs`
  ```javascript
  <span>
    {{percentage}}%
  </span>
//template name matches the component name.
  ```
##### Rendering a Component from a Template:

Components are called by name and can be passed additional data.
```javascript
  {{#each model.items as |lineItem}}
  <tr>
  ...
    <td>{{item-percentage itemPrice=lineItem.price orderPrice=model.price}}</td>
  ...
  </tr>
    ```
This renders the item-percentage component to this location and passes the item and order prices to the component.
##### Conditonals in Ember

Sometimes, you may want your application to display portions of a template only if a certain property exists.  In Ember, you may do so using the `{{if}}` helper to  render a block:
  ```javascript
  {{#if person}}
    Welcome back, <b>{{person.firstName}} {{person.lastName}}</b>!
  {{/if}}
```
Handlebars will not render the block if *falsy*.  In our case, we can use conditionals and an Ember Computed Macro: `gte` (stands for greater than or equal to) that returns *true* if the given property value is greater than or equal to the value given.
  ```javascript
  export default Ember.Component.extend({
    isImportant: Ember.computed.gte('percentage', 50),
    ...
  });
  ```
`isImportant` will be *true* if the percentage value is equal to or greater than 50%.  In the components template, we will use the Handlebars {{if}} helper to dynamically set or unset an `important` class:
  ```javascript
  <span class="{{id isImportant 'important'}}">
    {{percentage}}%
  </span>
  ```
##### Adding a Component Action
Components can hadle actions... just as routes.  We will create a `toggleDetaiils` action to toggle an `isShowingDetails` component property:

  ```javascript
    export default Ember.Component.extend({
      actions: {
        toggleDetails() {
          this.toggleProperty('showDetails');
        }
      }
      ...
    });
  ```
  ##### Mapping the `toggleDetails` Action
  Clicking the span should toggle the details, so the action is mapped on the `span` element:

  ```javascript
  <span class="{{if isImportant 'important'}}" {{action 'toggleDetails'}}>
    {{percentage}}%
  </span>
  ```
  Then, using a conditional inside of the template, we can change what is rendered to the user contingent upon the value of the toggled property:
  ```javascript
    <span class="{{id isImportant 'important'}}" {{action 'toggleDetails'}}>
      {{#if showDetails}}
        ${{itemPrice}} / ${{orderPrice}}
      {{else}}
        {{percentage}}%
      {{/if}}
    </span>
  ```
