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
Prefex render helpers with `render`.
Put the logic to show or not call the helper in the main `render` fuction.

This makes it easier see what the render method does and makes sure the render helpers have a single responsibility.
```
renderTooltip: ->
  <ReactPopover placement="right">
    Very important
  </ReactPopover>

renderReferenceUrl: ->
  <a className="external-reference-create"
    href={@createReferenceUrl()}
    target="_blank">
    Escalate
  </a>

render: ->
  <div>
    {@renderTooltip() if @state.showTooltip}
    {@renderReferenceUrl() if @createReferenceUrl()}
  </div>
```
