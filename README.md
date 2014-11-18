CJSX Style Guide
================

A Style Guide for writing clean and readable CJSX

## Table of Contents
* [Source Code Layout](#source-code-layout)
* [Complex renders](#complex-renders)

## Source Code Layout

* <a name="one-line"></a>
  Elements can span on one line as long as they fit. 
<sup>[[link](#one-line)]</sup>
  
  ```Coffee
  # Good
  <MightyComponent className="awesome-class" items={items} />
  
  # Bad
  <MightyComponent className="awesome-class" items={items} anotherProperty={wayTooLong} andAnother={wayTooLong} />
  
  # Good
  <MightyComponent className="awesome-class"
    items={items}
    anotherProperty={wayTooLong}
    andAnother={wayTooLong} />
  ```

## Complex renders
### Splitting it up
* <a name="prefix-with-render"></a>
  Prefex render helpers with `render`. 
<sup>[[link](#prefix-with-render)]</sup>

  ```Coffee
  # Good
  renderMightyComponent: ->
    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent() if @state.showMightyComponent}
    </div>

  # Bad
  mightyComponent: ->
    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@mightyComponent() if @state.showMightyComponent}
    </div>
```

* <a name="put-show-logic-in-render"></a>
  Put the logic to show or not call the helper in the main `render` fuction. This makes it easier see what the render method does and makes sure the render helpers have a single responsibility.
<sup>[[link](#put-show-logic-in-render)]</sup>

  ```Coffee
  # Good
  renderMightyComponent: ->
    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent() if @state.showMightyComponent}
    </div>

  # Bad
  renderMightyComponent: ->
    if @state.showMightyComponent
      <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent()}
    </div>
  ```
