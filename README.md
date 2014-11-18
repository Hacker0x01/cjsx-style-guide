CJSX Style Guide
================

A Style Guide for writing clean and readable CJSX

## Table of Contents
* [Source Code Layout](#source-code-layout)

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
