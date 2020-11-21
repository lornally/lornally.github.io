> stylus看起来比sass还要更简洁

- stylus

```stylus
body 
  font 14px/1.5 Helvetica, arial, sans-serif
  >logo 
    border-radius 5px
```

- stylus output

```css
body {
  font: 14px/1.5 Helvetica, arial, sans-serif;
}
body >logo { //差异在这一行, 这个是对的.
  border-radius: 5px;
}
```

- sass

```
body 
  font: 14px/1.5 Helvetica, arial, sans-serif
  >logo 
    border-radius: 5px
```

- sass output

```css
body {
  font: 14px/1.5 Helvetica, arial, sans-serif;
}
body>logo { //差异在这一行, 这个也是对的. 
  border-radius: 5px;
}
```

