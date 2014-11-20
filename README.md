CJSX Style Guide
================

A Style Guide for writing clean and readable CJSX

## Table of Contents
* [Source Code Layout](#source-code-layout)
* [Complex renders](#complex-renders)
* [Significant whitespace](#significant-whitespace)
* [Don't inline methods](#don-t-inline-methods)

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

  **NOTE**: When an element spans over multiple lines, the tags should all be aligned horizontally, like they are in the last example.

## Complex renders
* <a name="prefix-with-render"></a>
  Prefex render helpers with `render`.
<sup>[[link](#prefix-with-render)]</sup>

  ```Coffee
  # Good
  renderMightyComponent: ->
    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent()}
    </div>

  # Bad
  mightyComponent: ->
    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@mightyComponent()}
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

* <a name="use-array-for-classes"></a>
  Use arrays to deal with toggling multiple classes.
<sup>[[link](#use-array-for-classes)]</sup>

  ```Coffee
  # Good
  render: ->
    input_classes = [
      'active' if @props.isActive
      'pull-left'
      'spec-input'
    ].join(' ')

    <input className={input_classes} />

  # Bad
  render: ->
    input_classes = React.addons.classSet
      'active': @props.isActive
      'pull-left': true
      'spec-input': true

    <input className={input_classes} />

  # Bad
  render: ->
    <input className="#{active if @state.isActive} pull-left spec-input" />
  ```

## Significant whitespace
CJSX handles significant whitespace differently then HTML, there is a nice discussion about this here: https://github.com/facebook/react/pull/480 (about JSX but applies to cjsx).

  ```Coffee
  # Good
  render: ->
    # renders 'This is a link.'
    <div>
      This is a
      {' '}<a href="/link">link</a>.
    </div>

  # Bad
  render: ->
    # renders 'This is alink.'
    <div>
      This is a
      <a href="/link">link</a>.
    </div>

  # Bad (alternative)
  window.SPACE = ' '
  render: ->
    # renders 'This is a link.'
    <div>
      This is a
      {SPACE}<a href="/link">link</a>.
    </div>

  # Bad
  render: ->
    # renders 'This is a link.'
    <div>
      This is a
      &#32;<a href="/link">link</a>.
    </div>
   ```

## Don't inline methods
Define methods on the component instead of inlining them.

  ```Coffee
  # Good
  handleClick: ->
    # implementation
    
  render: ->
    <a onClick={@handleClick} />
  
  # Bad
  render: ->
    <a onClick={=> #implementation} />
   ```
