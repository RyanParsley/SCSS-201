class: middle, center, title

# SCSS: 201
## Proper care and feeding of your styles
### Ryan Parsley
#### 2023-04-12

???

Hey, it's me again. This time I want to talk about a different angle to think about css architecture.

Famously, scss is missused by well intentioned beginners. 

How do we guage a *good* use of scss?

---

class: middle, center


# Why do we like SCSS!?

???


I'll frame that by starting with the following 2 questions:

Should we even use scss in 2023?
If so, why?

I assert that if we agree on why we use scss, we're likely to agree on how to use scss. 

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

I bring this up not just to set up a joke but to note that there's nothing you can do with scss
that you can't do with regular css. 

SCSS is not to CSS ans JS is to HTML (that's css in js frameworks).

SCSS is to CSS as TS is to JS.

SCSS compiles to CSS and it's important to rember that and be mindful of how.

---

background-image: url(./assets/deleteCSS.jpg)
background-size: 700px auto

???

Devs live in fear of editing css.

It's like the new perl. Write once update never.

Why do they feel that way!?

---

class: middle, center

# CSS is simple.
## That's what makes it hard. 

???

There's this duality of css where devs disparage it for being trivial yet live in fear of it.

CSS is a blunt instrument. The elegance of good CSS comes from how you wield it and not from language features. 

Good CSS repeats itself... sometimes if it helps express intent. 

The most natural way to DRY up CSS is to simply leverage the cascade. 

That can be elegant if you do it right, but has it's own set of problems to come to terms with. 

---

class: middle

> ## "Sass helps keep large stylesheets well-organized and makes it easy to share design within and across projects."
> — [the docs](https://sass-lang.com/documentation/)

---

class: middle, center

# Ergonomics
## ...is why we like SCSS

???

Designing and arranging things people use so that the people and things... 

**interact most efficiently and safely**

You want/need code reuse uncoupled from semantics? 

SCSS provides a means for that that doesn't govern your markup.

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

# Functions

> They make it easy to abstract out common formulas and behaviors in a readable way.

???

Once upon a time I maintained my own naive pixel based grid system. 

I'd noodle away in google docs to derive sensible values and manually plug the results into css.

This was not a good time. 

---

# Functions

```scss
@function invert($color, $amount: 100%) {
  $inverse: change-color($color, $hue: hue($color) + 180);
  @return mix($inverse, $color, $amount);
}

$primary-color: #036;
.header {
  background-color: invert($primary-color, 80%);
}
```

---

# Functions

## When to use functions?

> While it’s technically possible for functions to have side-effects like setting global variables, this is strongly discouraged. Use mixins for side-effects, and use functions just to compute values.

???

Rarely. Day to day feature work, styling your app, you probably don't want a function, but when you need them, they're great.

You probably mostly want functions if you're building/maintaining a css framework. 

They can be great for DRYing up your source code, but could lead to generating more than you bargain for. Also, if you find
yourself generating tons of similar code, that could be a smell that your approach is off. Sass functions can be "too much rope" sometimes. 

Do you really want to generate a dozen px values for column widths, or does flexbox do what you need with less?

So, what's a mixin?

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

# Mixins

> Mixins allow you to define styles that can be re-used throughout your stylesheet. They make it easy to avoid using non-semantic classes like .float-left, and to distribute collections of styles in libraries.

???

And according to the documentation, that's exactly why they exist!

---

# Mixins
 
## That looks like a function, what's the difference!?

???

Do you want to genereate a value, or do you want to generate css.

Typically, unless you're building a component liberary, your responsibilities will stop at generating css. 

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

# Placeholders

```scss
.action-buttons, .reset-buttons {
  box-sizing: border-box;
  border-top: 1px rgba(0, 0, 0, 0.12) solid;
  padding: 16px 0;
  width: 100%;
}
.action-buttons:hover, .reset-buttons:hover {
  border: 2px rgba(0, 0, 0, 0.5) solid;
}

.action-buttons {
  color: #4285f4;
}

.reset-buttons {
  color: #cddc39;
}
```
???

You can wire up a linter to pester you to refactor your code, or leverage scss to enable
you to not need to. This feels like a more expressive way to write the css that ultimately want.

A problem with css "best practices" is the stakes often feel too low to champion. Especially, 
when contrasted with the ease of use of some less ideal solutions.

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
class: middle, center

# Don't just generate a bunch of bad css

???

You're DRYing up your source code... but potentially generating a ton of CSS.

A somewhat common problem is to abuse scss.

too much nesting

too much css

brittle selectors

---

# Lint your styles

```json
{
  'scss/no-duplicate-mixins': true,
  'scss/no-global-function-names': true
  'scss/at-mixin-pattern': [
    '^(-?[a-z][a-z0-9]*)(-[a-z0-9]+)*$',
    {
      message: 'Expected mixin name to be kebab-case',
    },
  ],
}
```

???

this is not strictly an scss thing, you can and should lint your css too

SCSS is a proper code, lint it as such

---

# References

- [stylelint](https://stylelint.io/)
- [stylelint's SCSS standard config](https://github.com/stylelint-scss/stylelint-config-standard-scss)
- [SCSS Docs: Variables](https://sass-lang.com/documentation/variables)
