**Components**
provide a reusable way to combine a template wuth action handling behavior.

used for:
1. charts
2. Tabs
3. Tree Widgets
4. Buttons with confirmations
5. Date range selectors
6. Wrap libraries and services

Components follow very closely with W3C Custom Elements spec.
### Generating a Component Using `Ember CLI`:
  ```
    $ ember generare component <component-name>
  ```
  Component are defined in `app/components` and extend from `Ember.Component` and must use hyphen in the namespace to differentiate them from standard HTML tags and conform to web component specs.

  *Defining the Component Logic*
  Components have their own properites, fuctions, and behaviors.
  ```
    export default Ember.Component.extend({
      percentage: Ember.computed('itemPrice', 'orderPrice', function() {
        return this.get('itemPrice') / this.get('orderPrice') * 100'
        })
      });
  ```
  *Customizing the Component's Template:*
  Defined in `app/templates/components/template-name.hbs`
  ```
  <span>{{percentage}}%</span>
  //template name matches the component name.
  ```
  *Rendering a component frpm a Template:*
  Components are called by name and can be passed additional data.
  ```
    {{#each model.items as |lineItem}}
    <tr>
    ...
      <td>{{item-percentage itemPrice=lineItem.price orderPrice=model.price}}</td>
    ...
    </tr>
    ```
  This renders the item-percentage component to this location and passes the item and order prices to the component.
  *Condiitonals*
  Sometimes your app may only want to display portions of a template if a certain property exists.  You may do so using the `{{if}}` helper to confitionally render a block:
    ```
    {{#if person}}
      Welcome back, <b>{{person.firstName}} {{person.lastName}}</b>!
    {{/if}}
    ```
Handlebars will not render the block if *falsy*.  In our case, we can use conditionals and an Ember Computed Macro `gte` (greater than or equal to) macro that returns true if the given property value is greater than or equal to the value given.
  ```
  export default Ember.Component.extend({
    isImportant: Ember.computed.gte('percentage', 50),
    ...
  ```
`Isimportant` will be true if the percentage value is eaual to or greater than 50%.  In the components template, we will use the Handlebars {{if}} helper to dynamically set or unset an 'important' class:
  ```
  <span class="{{id isImportant 'important'}}">
    {{percentage}}%
  </span>
  ```
*Adding a Component Action*
Components can hadle actions just as routes.  We'll handle a 'toggleDetaiils' action to toggle an 'isShowingDetails' component property
```
  export default Ember.Component.extend({
    actions: {
      toggleDetails() {
        this.toggleProperty('showDetails');
        // toggleProperty() switches a property's value between trie and false wach time it's called, like a light switch
    }
  ...
  ```
  *Mapping the toggleDetails Action*
  Clicking the span should toggle the details, so the acton is mapped o the span element:
  ```
    <span class="{{id isImportant 'important'}}" {{action 'toggleDetails'}}>
      {{percentage}}%
    </span>
  ```
  Then, use a conditional inside of the template to change what's rendered on the page depending on the value of the toggled property.
  ```
    <span class="{{id isImportant 'important'}}" {{action 'toggleDetails'}}>
      {{#if showDetails}}
        ${{itemPrice}} / ${{orderPrice}}
      {{else}}
        {{percentage}}%
      {{/if}}
    </span>
  ```
