> # How to convert `px` to `rem` using scss

Say we have the follow html

```html
<p id="px">22 px</p>
<p id="rem">rem equivalent to 22px</p>
```

And we want to apply to both of them `font-size` of 22px and the equivalent of 22px to rem.

```scss
@mixin font-size($pixels) {
  $rem: ($pixels * 0.0625);
  font-size: $rem + rem;
  font-size: $pixels + px;
}

#px {
  font-size: 22px;
}

#rem {
  @include font-size(22)
}
```

 Explanation: The `mixin` receives the pixels and apply to the element the pixels and the equivalent to rem.

 