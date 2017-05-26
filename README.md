# d3-loom

This is, sort of, a d3 plugin to create a "loom" visualization

[![The words spoken by the Fellowship member during all 3 movies](lotr.png "The words spoken by the Fellowship member during all 3 movies")](http://bl.ocks.org/nbremer/6599644129c034d0cb17fcdc452c310b)

## API Reference

<a href="#loom" name="loom">#</a> d3.<b>loom</b>()

Constructs a new loom generator with the default settings.

<a href="#_loom" name="_loom">#</a> <i>loom</i>(<i>data</i>)

Computes the loom layout for the specified *data*. The length of the returned array is the same as *data*, however, due to sorting of the strings, to reduce overlap, the ordering can be different than the initial data. 

Typically a dataset for the *loom* contains 3 values; the outer entity, the inner entity and the value that connects these two (e.g. outer = location, inner = person, value = days that person stayed in the location):

```js
var data = [
  {
    outer: "Amsterdam",
    inner: "Alexander",
    value: 679
  },
  {
    outer: "Bangkok",
    inner: "Brendan",
    value: 124
  },
  //...and so on
];
```

The return value of *loom*(*data*) is an array of *looms*, where each loom represents the connection between an entity in the center of the loom (the *inner*) and the entity on the outside of the loom (the *outer*) and is an object with the following properties:

* `inner` - the inner subgroup
* `outer` - the outer subgroup

Both the inner and outer subgroup are also objects. The inner has the following properties:

* `name` - the [name](#loom_inner) of the inner entity
* `offset` - the [horizontal offset](#loom_widthInner) of the inner string's endpoint from the center
* `x` - the horizontal location of the inner entity
* `y` - the vertical location of the inner entity

And the outer has the following properties:

* `startAngle` - the [start angle](#string_startAngle) of the arc in radians
* `endAngle` - the [end angle](#string_endAngle) of the arc in radians
* `value` - the numeric [value](#loom_value) of the arc
* `index` - the zero-based [sorted index](#loom_sortGroups) of the arc
* `subindex` - the zero-based [sorted sub-index](#loom_sortSubgroups) of the arc
* `innername` - the [name](#loom_inner) of the connected inner entity
* `outername` - the [name](#loom_outer) of the outer entity

The *looms* are passed to [d3.string](#string) to display the relationships between the inner and outer entities.

The *looms* array also defines a two secondary arrays. The first, called *groups*, is an array representing the outer entities. The length of the array is the number of unique outer entities and is an object with the following properties:

* `startAngle` - the [start angle](#string_startAngle) of the arc in radians
* `endAngle` - the [end angle](#string_endAngle) of the arc in radians
* `value` - the numeric [value](#loom_value) of the arc
* `index` - the zero-based [sorted index](#loom_sortGroups) of the arc
* `outername` - the [name](#loom_outer) of the outer entity

The *groups* are passed to [d3.arc](https://github.com/d3/d3-shape#arc) to produce a donut chart around the circumference of the loom layout.

The other array, called, *innergroups*, is an array represting the inner entities. The length of the array is the number of unique inner entities and is an object with the following properties:

* `name` - the [name](#loom_inner) of the inner entity
* `offset` - the [horizontal offset](#loom_widthInner) of the inner string's endpoint from the center
* `x` - the horizontal location of the inner entity
* `y` - the vertical location of the inner entity

The *innergroups* are used to create the textual representation of the inner entities in the center of the loom layout.

<a href="#loom_padAngle" name="loom_padAngle">#</a> <i>loom</i>.<b>padAngle</b>([<i>angle</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L195 "Source")

If *angle* is specified, sets the pad angle (i.e. the white space) between adjacent outer groups to the specified number in radians and returns this loom layout. If *angle* is not specified, returns the current pad angle, which defaults to zero.

<a href="#loom_inner" name="loom_inner">#</a> <i>loom</i>.<b>inner</b>([<i>inner</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L199 "Source")

The *inner* represents the name/id/... textual value of the entities that will be placed in the center. If *inner* is specified, sets the inner accessor to the specified function and returns this string generator. If *inner* is not specified, returns the current inner accessor, which defaults to:

```js
function inner(d) {
  return d.inner;
}
```

<a href="#loom_outer" name="loom_outer">#</a> <i>loom</i>.<b>outer</b>([<i>outer</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L203 "Source")

The *outer* represents the name/id/... textual value of the entities that will be placed around the loom along a circle. If *outer* is specified, sets the outer accessor to the specified function and returns this string generator. If *outer* is not specified, returns the current outer accessor, which defaults to:

```js
function outer(d) {
  return d.outer;
}
```

<a href="#loom_value" name="loom_value">#</a> <i>loom</i>.<b>value</b>([<i>value</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L207 "Source")

The *value* represents the numeric value that is the connection between the inner and outer entity. It is the value that determines the width of the strings on the outside. If *value* is specified, sets the value accessor to the specified function and returns this string generator. If *value* is not specified, returns the current value accessor, which defaults to:

```js
function value(d) {
  return d.value;
}
```

<a href="#loom_heightInner" name="loom_heightInner">#</a> <i>loom</i>.<b>heightInner</b>([<i>height</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L211 "Source")

This *height* gives the vertical distance between the inner entities in the center. It is the value that determines the width of the strings on the outside. If *height* is specified, sets the heightInner to the specified number and returns this loom generator. If height is not specified, returns the current heightInner value, which defaults to 20 pixels.

<a href="#loom_widthInner" name="loom_widthInner">#</a> <i>loom</i>.<b>widthInner</b>([<i>width</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L215 "Source")

This *width* gives the horizontal distance between the inner endpoints of the strings in the center. It is the value that determines the width of the gap that is created so the text of the inner entities does not overlap the strings. If *width* is specified, sets the widthInner to the specified function or number and returns this loom generator. If width is not specified, returns the current widthInner value, which defaults to 30 pixels.

<a href="#loom_emptyPerc" name="loom_emptyPerc">#</a> <i>loom</i>.<b>emptyPerc</b>([<i>value</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L219 "Source")

This *value* gives the percentage of the circle that will be empty to create space in the top and bottom. If *value* is specified, sets the current emptyPerc to the specified number in the range [0,1] and returns this loom generator. If value is not specified, returns the current emptyPerc value, which defaults to 0.2.

<a href="#loom_sortGroups" name="loom_sortGroups">#</a> <i>loom</i>.<b>sortGroups</b>([<i>compare</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L223 "Source")

If *compare* is specified, sets the group comparator to the specified function or null and returns this loom layout. If *compare* is not specified, returns the current group comparator, which defaults to null. If the group comparator is non-null, it is used to sort the outer groups/entities by their total value (i.e. the sum of all the inner strings). See also [d3.ascending](https://github.com/d3/d3-array#ascending) and [d3.descending](https://github.com/d3/d3-array#descending).

<a href="#loom_sortSubgroups" name="loom_sortSubgroups">#</a> <i>loom</i>.<b>sortSubgroups</b>([<i>compare</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L227 "Source")

If *compare* is specified, sets the subgroup comparator to the specified function or null and returns this loom layout. If *compare* is not specified, returns the current subgroup comparator, which defaults to null. If the subgroup comparator is non-null, it is used to sort the subgroups (i.e. the separate strings) within each outer entity by their value. See also [d3.ascending](https://github.com/d3/d3-array#ascending) and [d3.descending](https://github.com/d3/d3-array#descending).

<a href="#loom_sortLooms" name="loom_sortLooms">#</a> <i>loom</i>.<b>sortLooms</b>([<i>compare</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L231 "Source")

If *compare* is specified, sets the loom comparator to the specified function or null and returns this loom layout. If *compare* is not specified, returns the current loom comparator, which defaults to null. If the loom comparator is non-null, it is used to sort the [strings](#_loom) by their value; this only affects the *z*-order of these inner strings. See also [d3.ascending](https://github.com/d3/d3-array#ascending) and [d3.descending](https://github.com/d3/d3-array#descending).

<a href="#string" name="string">#</a> d3.<b>string</b>() [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L240 "Source")

Creates a new string generator with the default settings.

<a href="#_string" name="_string">#</a> <i>string</i>(<i>arguments…</i>) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L263 "Source")

Generates a string for the given *arguments*. The *arguments* are arbitrary; they are simply propagated to the string's generator's accessor functions along with the `this` object. If the string generator has a context, then the string is rendered to this context as a sequence of path method calls and this function returns void. Otherwise, a path data string is returned.

Typically, only the [radius](#string_radius), [thicknessInner](#string_thicknessInner), and [pullOut](#string_pullOut) should be adjusted when used on conjunction with the [loom](#loom), because all the other accessors will work with the default settings.

<a href="#string_radius" name="string_radius">#</a> <i>string</i>.<b>radius</b>([<i>radius</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L343 "Source")

If *radius* is specified, sets the radius accessor to the specified function and returns this string generator. If *radius* is not specified, returns the current radius value, which defaults to 100 pixels.

The *radius* represents the inner radius of the loom and is typically set to a single number. It is advised to always set this value different from the default, depending on the space available within your svg.

<a href="#string_thicknessInner" name="string_thicknessInner">#</a> <i>string</i>.<b>thicknessInner</b>([<i>thickness</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L367 "Source")

If *thickness* is specified, sets the thicknessInner to the specified number and returns this string generator. If *thickness* is not specified, returns the current thicknessInner value, which defaults to 0 pixels. The thicknessInner defines the "height", or thickness, that the strings will have at their endpoints next to the inner entities.

<a href="#string_pullout" name="string_pullout">#</a> <i>string</i>.<b>pullout</b>([<i>pullout</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L379 "Source")

If *pullout* is specified, sets the pullout to the specified number and returns this string generator. If *pullout* is not specified, returns the current pullout value, which defaults to 50 pixels. The pullout defines how far the two circle halves will be placed outward horizontally.

<a href="#string_inner" name="string_inner">#</a> <i>string</i>.<b>inner</b>([<i>inner</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L371 "Source")

If *inner* is specified, sets the inner accessor to the specified function and returns this string generator. If *inner* is not specified, returns the current source accessor, which defaults to:

```js
function inner(d) {
  return d.inner;
}
```

When the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *inner* object. Thus this accessor function does not have to be set separately, but just use the default.

<a href="#string_outer" name="string_outer">#</a> <i>string</i>.<b>outer</b>([<i>outer</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L375 "Source")

If *outer* is specified, sets the outer accessor to the specified function and returns this string generator. If *outer* is not specified, returns the current outer accessor, which defaults to:

```js
function outer(d) {
  return d.outer;
}
```

When the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *outer* object. Thus this accessor function does not have to be set separately, but just use the default.

<a href="#string_startAngle" name="string_startAngle">#</a> <i>string</i>.<b>startAngle</b>([<i>angle</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L247 "Source")

If *angle* is specified, sets the start angle accessor to the specified function and returns this string generator. If *angle* is not specified, returns the current start angle accessor, which defaults to:

```js
function startAngle(d) {
  return d.startAngle;
}
```

The *angle* is specified in radians, with 0 at -*y* (12 o'clock) and positive angles proceeding clockwise. When the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *startAngle* value within the *outer* object. In that case this accessor function does not have to be set separately, but just use the default.

<a href="#string_endAngle" name="string_endAngle">#</a> <i>string</i>.<b>endAngle</b>([<i>angle</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L251 "Source")

If *angle* is specified, sets the end angle accessor to the specified function and returns this string generator. If *angle* is not specified, returns the current end angle accessor, which defaults to:

```js
function endAngle(d) {
  return d.endAngle;
}
```

The *angle* is specified in radians, with 0 at -*y* (12 o'clock) and positive angles proceeding clockwise. When the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *endAngle* value within the *outer* object. In that case this accessor function does not have to be set separately, but just use the default.

<a href="#string_x" name="string_x">#</a> <i>string</i>.<b>x</b>([<i>x</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L355 "Source")

If *x* is specified, sets the x accessor to the specified function and returns this string generator. If *x* is not specified, returns the current x accessor, which defaults to:

```js
function x(d) {
  return d.x;
}
```

The *x* defines the horizontal location where the inner entities are placed, typically in the center of the loom. When the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *x* value within the *inner* object. In that case this accessor function does not have to be set separately, but just use the default.

<a href="#string_y" name="string_y">#</a> <i>string</i>.<b>y</b>([<i>y</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L359 "Source")

If *y* is specified, sets the y accessor to the specified function and returns this string generator. If *y* is not specified, returns the current y accessor, which defaults to:

```js
function y(d) {
  return d.y;
}
```

The *y* defines the vertical location where the inner entities are placed. They are typically placed in a column like fashion in the center, one above the other. When the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *y* value within the *inner* object. In that case this accessor function does not have to be set separately, but just use the default.

<a href="#string_offset" name="string_offset">#</a> <i>string</i>.<b>offset</b>([<i>offset</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L363 "Source")

If *offset* is specified, sets the offset accessor to the specified function and returns this string generator. If *offset* is not specified, returns the current offset accessor, which defaults to:

```js
function offset(d) {
  return d.offset;
}
```

The *offset* defines the horizontal space between the inner end points of the strings, so that the text of the inner entities does not overlap the strings. It is typically set through the [widthInner](#loom_widthInner) accessor of the loom and propagates through to the string function. Therefore, when the string generator is used in conjunction with the *loom*, the resulting loom array will contain an *offset* value within the *inner* object. In that case this accessor function does not have to be set separately, but just use the default.

<a href="#string_context" name="string_context">#</a> <i>string</i>.<b>context</b>([<i>context</i>]) [<>](https://github.com/nbremer/d3-loom/blob/master/loom.js#L383 "Source")

If *context* is specified, sets the context and returns this string generator. If *context* is not specified, returns the current context, which defaults to null. If the context is not null, then the [generated string](#_string) is rendered to this context as a sequence of [path method](http://www.w3.org/TR/2dcontext/#canvaspathmethods) calls. Otherwise, a [path data](http://www.w3.org/TR/SVG/paths.html#PathData) string representing the generated string is returned. See also [d3-path](https://github.com/d3/d3-path).
