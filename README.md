CJSX Style Guide
================

A Style Guide for writing clean and readable CJSX

## This repository is no longer maintained.

Since we're no longer using CJSX in our codebase, we're no longer contributing to this repo.
If you'd like to make this repository your own, please let us know. We'll be happy to transfer it to a new maintainer!

## Table of Contents
* [Source Code Layout](#source-code-layout)
* [Complex renders](#complex-renders)
* [Significant whitespace](#significant-whitespace)
* [Don't inline methods](#dont-inline-methods)
* [Usage of method factories for handlers](#usage-of-method-factories-for-handlers)
* [Usage of quotes in CJSX elements](#usage-of-quotes-in-cjsx-elements)

## Source Code Layout

* <a name="one-line"></a>
  An element can span on one line as long as it fits.  If it does not fit, each attribute must appear on its own line below the tag, indented one level.
<sup>[[link](#one-line)]</sup>

  ```Coffee
  # Good
  <MightyComponent className="awesome-class" items={items} />

  # Good
  <MightyComponent className="awesome-class" items={items}>
    [.. content ..]
  </MightyComponent>

  # Good
  <MightyComponent
      className="awesome-class"
      items={items}
      anotherProperty={wayTooLong}
      andAnother={wayTooLong} />

  # Good
  <MightyComponent
      className="awesome-class"
      items={items}
      anotherProperty={wayTooLong}
      andAnother={wayTooLong}>
    [.. content ..]
  </MightyComponent>


  # Bad
  <MightyComponent className="awesome-class" items={items} anotherProperty={wayTooLong} andAnother={wayTooLong} />

  # Bad
  <MightyComponent className="awesome-class" items={items} anotherProperty={wayTooLong} andAnother={wayTooLong} >
    [.. content ..]
  </MightyComponent>

  # Bad
  <MightyComponent className="awesome-class"
                   items={items}
                   anotherProperty={wayTooLong}
                   andAnother={wayTooLong} />

  ```

* <a name="space-at-end"></a>
  If an element utilizes a self-closing tag, it must have a space character before the `/>` characters.  Additionally, the `/>` should always go on the same line as the last attribute in the tag.
<sup>[[link](#space-at-end)]</sup>
  
  ```Coffee
  # Good
  <MightyComponent className="awesome-class" items={items} />

  # Good
  <MightyComponent
      className="awesome-class"
      items={items}
      anotherProperty={wayTooLong}
      andAnother={wayTooLong} />

  # Bad
  <MightyComponent className="awesome-class"/>

  # Bad
  <MightyComponent
      className="awesome-class"
      items={items}
      anotherProperty={wayTooLong}
      andAnother={wayTooLong}/>

  # Bad
  <MightyComponent className="awesome-class"
                   items={items}
  />

  # Bad
  <MightyComponent
      className="awesome-class"
      items={items}
  />

  ```


## Complex renders
* <a name="prefix-with-render"></a>
  Prefix render helpers with `render`.
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
  Do not put the logic to show or not call the helper in the main `render` fuction. This introduces cyclomatic complexity and is more difficult to read when having multipe if/unless statements – use guards instead.
<sup>[[link](#put-show-logic-in-render)]</sup>

  ```Coffee
  # Good
  renderMightyComponent: ->
    return null unless @state.showMightyComponent

    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent()}
    </div>

  # Bad
  renderMightyComponent: ->
    if @state.showMightyComponent
      <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent()}
    </div>

  # Bad
  renderMightyComponent: ->
    <MightyComponent>Very important</MightyComponent>

  render: ->
    <div>
      {@renderMightyComponent() if @state.showMightyComponent}
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
      <em>This</em>
      {' '}is a{' '}
      <a href="/link">link</a>.
    </div>

  # Bad
  render: ->
    # renders 'This is a link.'
    <div>
      <em>This</em>{' '}
      is a
      {' '}<a href="/link">link</a>.
    </div>

  # Bad
  render: ->
    # renders 'Thisis alink.'
    <div>
      <em>This</em>
      is a
      <a href="/link">link</a>.
    </div>

  # Bad (alternative)
  window.SPACE = ' '
  render: ->
    # renders 'This is a link.'
    <div>
      <em>This</em>
      {SPACE}is a{SPACE}
      <a href="/link">link</a>.
    </div>

  # Bad
  render: ->
    # renders 'This is a link.'
    <div>
      <em>This</em>
      &#32;is a&#32;
      <a href="/link">link</a>.
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

Advanced usage, methods with arguments: Since Coffeescript will call the method on render when arguments are passed, the handler method should return the actual method to be called. Use a method factory for this.

## Usage of method factories for handlers
Often times in CJSX/CoffeeScript you're in need of methods that return methods. We call these methods factories. To be clear about the purpose of the method, it is required to always add the `Factory` postfix to these method names.

The first arrow should always be a -> (for all methods on a component React binds the current component to `@`), but the second one can vary based on the context you need. If you need to call @ in your method, please make sure to use a fat (=>) arrow. Otherwise a -> would suffice.

```Coffee
  # Good
  <a onClick={@handleClickFactory('first')} />

  handleClickFactory: (element) -> =>
    @setState clicked: element

  # Good
  <a onClick={@handleClickFactory('first')} />

  handleClickFactory: (element) -> ->
    alert(element)

  # Bad
  <a onClick={@handleClick('first')} />

  handleClick: (element) -> ->
    alert(element)

  # Bad
  <a onClick={-> @handleClick('first')} />

  handleClick: (element) ->
    @setState clicked: element

```

## Usage of quotes in CJSX elements
Use double quotes instead of single quotes when writing CJSX elements:

  ```Coffee
  # Good
  render: ->
    <a className="link" href="/page" />

  # Bad
  render: ->
    <a className='link' href='/page' />
```

It's fine to use single quotes on non-CJSX elements

