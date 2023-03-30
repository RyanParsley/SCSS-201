class: middle, center, title

# SCSS: 201
## Proper care and feeding of your styles
### Ryan Parsley
#### 2023-04-12

---

class: middle, center


# Why do we like SCSS!?

???

If we're going to talk for a half an hour about how to do sass right,
maybe we should start by laying a foundation of what we expect sass to give us.

---

# Why do we like SCSS!?

- **Is it nesting?**

???

Not really, gratuitous nesting makes for a bad time and even leads to CSS caused performance issues. 

---

# Why do we like SCSS!?

- ~~Is it nesting?~~
- **Is it variables?**

???

No, CSS has had that for days

---

# Why do we like SCSS!?

- ~~Is it nesting?~~
- ~~Is it variables?~~
- **Liskov substitution principle?**

???

Now you're just being silly

---

class: middle, center

# CSS is simple.
## That's what makes it hard. 

---

background-image: url(./assets/deleteCSS.jpg)
background-size: 700px auto

---

class: middle, center

# Ergonomics
## ...is why we like SCSS

???

Designing and arranging things people use so that the people and things... 

**interact most efficiently and safely**

---

# Better know a variable

> - Sass variables are all compiled away by Sass. CSS variables are included in the CSS output.
> - CSS variables can have different values for different elements, but Sass variables only have one value at a time.
> 
> — [the docs](https://sass-lang.com/documentation/variables)

???

If you want to use Sass responsibly, you should be aware of what's happening under the hood.

Sass can help you write cleaner css, but it often helps developers write gobs of worse css that they never consider. 

---

# Mixins

```scss
@mixin reset-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

@mixin horizontal-list {
  @include reset-list;

  li {
    display: inline-block;
    margin-left: 2em;
    margin-right: 2em;
  }
}

nav ul {
  @include horizontal-list;
}
```

???

To my sensibilities, this is what you want when you're stuck with a utility class. 

---

# Why is that cool?

```html
<!-- html implication of utility class -->

<nav>
  <ul class="reset-list horizontal-list">...</ul>
</nav>
```

```html
<!-- with mixin -->

<nav>
  <ul>...</ul>
</nav>

```

???

arguments in favor of abandoning semantics are often around the maintainability
of semantic html at scale, but mixin and placeholders largely solve for that. 

---

# Placeholders
> It looks and acts a lot like a class selector, but it starts with a % and it's not included in the CSS output.

???

What else can I say?

---

# Placeholders

```scss
%toolbelt {
  box-sizing: border-box;
  border-top: 1px rgba(#000, .12) solid;
  padding: 16px 0;
  width: 100%;

  &:hover { border: 2px rgba(#000, .5) solid; }
}

.action-buttons {
  @extend %toolbelt;
  color: #4285f4;
}

.reset-buttons {
  @extend %toolbelt;
  color: #cddc39;
}
```

???

Again, we have reusable styles that are not coupled with the semantics of the markup

... yet still compatible with semantic markup. 

---

# Maps

```scss
$font-weights: ("regular": 400, "medium": 500, "bold": 700);

map.get($font-weights, "medium"); // 500
```

???
Say goodbye to a lot of magic string flavored css issues. 

If you copy/paste from the DS color pallete, you're on a doomed trajectory with respect to upgrades.

Here you end up with code that easier to refactor.
Assured to be consistent.
and is resistent to typos.

---

# When to use functions

???

Rarely

---

# Don't just generate a bunch of bad css

???

A somewhat common problem is to abuse scss.

too much nesting

too much css

brittle selectors

---

# Lint your styles

```scss
example of lint error
```

???

this is not strictly an scss thing, you can and should lint your css too

SCSS is a proper code, lint it as such

---

# References

- [stylelint](https://stylelint.io/)
- [stylelint's SCSS standard config](https://github.com/stylelint-scss/stylelint-config-standard-scss)
- [SCSS Docs: Variables](https://sass-lang.com/documentation/variables)
