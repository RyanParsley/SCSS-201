# SCSS: 201
## Proper care and feeding of your styles
### Ryan Parsley
### 2023-04-12

---

# Lint your styles

```scss
example of lint error
```

???

SCSS is a proper code, lint it as such

---

# Better know a variable

> - Sass variables are all compiled away by Sass. CSS variables are included in the CSS output.
> - CSS variables can have different values for different elements, but Sass variables only have one value at a time.
> 
> — [the docs](https://sass-lang.com/documentation/variables)

---

# Maps

- enums for css

???

https://sass-lang.com/documentation/values/maps

---

# The point of placeholders

They compile away if you don't use them

???

What else can I say?

---

# When to use functions

---

# Don't just generate a bunch of bad css

???

A somewhat common problem is to abuse scss.

too much nesting

too much css

brittle selectors

---

# References

- [stylelint](https://stylelint.io/)
- [stylelint's SCSS standard config](https://github.com/stylelint-scss/stylelint-config-standard-scss)
- [SCSS Docs: Variables](https://sass-lang.com/documentation/variables)
