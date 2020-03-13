# Folio SCSS

This repository contains a set of sass stylesheets with elements and tools that intend to be reusable. The file structure is inspired by [inuitcss](https://github.com/inuitcss/inuitcss), and the following guidelines are a reminder from [Harry Roberts](docs/csswizardry-namespaces.md) of the naming convention which this project intends to stick to.

The Namespaces
--------------

In no particular order, here are the individual namespaces and a brief
description. We’ll look at each in more detail in a moment, but the
following list should acquaint you with the kinds of thing we’re hoping
to achieve.

-   `o-`{.highlighter-rouge}: Signify that something is an Object, and
    that it may be used in any number of unrelated contexts to the one
    you can currently see it in. Making modifications to these types of
    class could potentially have knock-on effects in a lot of other
    unrelated places. Tread carefully.
-   `c-`{.highlighter-rouge}: Signify that something is a Component.
    This is a concrete, implementation-specific piece of UI. All of the
    changes you make to its styles should be detectable in the context
    you’re currently looking at. Modifying these styles should be safe
    and have no side effects.
-   `u-`{.highlighter-rouge}: Signify that this class is a Utility
    class. It has a very specific role (often providing only one
    declaration) and should not be bound onto or changed. It can be
    reused and is not tied to any specific piece of UI. You will
    probably recognise this namespace from libraries and methodologies
    like [SUIT](https://suitcss.github.io/).
-   `t-`{.highlighter-rouge}: Signify that a class is responsible for
    adding a Theme to a view. It lets us know that UI Components’
    current cosmetic appearance may be due to the presence of a theme.
-   `s-`{.highlighter-rouge}: Signify that a class creates a new styling
    context or *Scope*. Similar to a Theme, but not necessarily
    cosmetic, these should be used sparingly—they can be open to abuse
    and lead to poor CSS if not used wisely.
-   `is-`{.highlighter-rouge}, `has-`{.highlighter-rouge}: Signify that
    the piece of UI in question is currently styled a certain way
    because of a state or condition. This stateful namespace is
    gorgeous, and comes from [SMACSS](https://smacss.com/). It tells us
    that the DOM currently has a temporary, optional, or short-lived
    style applied to it due to a certain state being invoked.
-   `_`{.highlighter-rouge}: Signify that this class is the worst of the
    worst—a hack! Sometimes, although incredibly rarely, we need to add
    a class in our markup in order to force something to work. If we do
    this, we need to let others know that this class is less than ideal,
    and hopefully temporary (i.e. “do not bind onto this”).
-   `js-`{.highlighter-rouge}: Signify that this piece of the DOM has
    some behaviour acting upon it, and that JavaScript binds onto it to
    provide that behaviour. If you’re not a developer working with
    JavaScript, leave these well alone.
-   `qa-`{.highlighter-rouge}: Signify that a QA or Test Engineering
    team is running an automated UI test which needs to find or bind
    onto these parts of the DOM. Like the JavaScript namespace, this
    basically just reserves hooks in the DOM for non-CSS purposes.

Even from this short list alone, we can see just how much more
information we can communicate to developers simply by placing a
character or two at the front of our existing classes.

It is probably worth noting at this point that these namespaces do not
exist for encapsulation and sandboxing of styles, but for clarity and
informative reasons. [Ben Frain](https://twitter.com/benfrain)’s
[FUN](http://benfrain.com/fun-css-naming-convention-explained/)
convention utilises namespacing as a means of soft encapsulation.

Object Namespaces: `o-`{.highlighter-rouge}
-------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.o-object-name[<element>|<modifier>] {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.o-layout {}

  .o-layout__item {}

.o-layout--fixed {}
```

</div>

The `o-`{.highlighter-rouge} namespace for Objects is a very useful one
for any teams who use Object-Oriented CSS.

OOCSS is fantastic in that it teaches us to abstract out the repetitive,
shared, and purely structural aspects of a UI into reusable *objects*.
This means that things like layout, wrappers and containers, the [Media
Object](http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code/),
etc. can all exist as non-cosmetic styles that handle the skeletal
aspect of a lot of UI components, without ever actually looking like
designed ‘things’.

This leads to much DRYer and drastically smaller stylesheets, but does
bring with it one problem: how do we know which classes might be purely
structural, and therefore possibly being used in an open-ended number of
instances?

This poses problems on projects quite frequently. Picture the following
example.

Imagine you’re a developer new to a project, and you have no intimate
knowledge of the CSS or what its classes mean or do. You’re asked by a
Product Owner to add some padding around the testimonials that appear on
the site. You right click, Inspect Element, and you see this:

<div class="highlighter-rouge">

``` {.highlight}
<blockquote class="media  testimonial">
</blockquote>
```

</div>

Now, it should be fairly clear here that what you should do is go and
find the `.testimonial {}`{.highlighter-rouge} ruleset in your CSS and
add the padding there. However, using DevTools, you find that adding the
padding to the `.media {}`{.highlighter-rouge} ruleset has exactly the
outcome you expected. Perfect! Let’s go and add that into the source CSS
file.

The issue here is that `.media`{.highlighter-rouge} is an abstraction
(it’s actually the poster child of Nicole Sullivan’s OOCSS) which, by
definition, is a reusable and non-cosmetic design pattern that can
underpin any number of different UI components. Sure, altering the
padding of it in this instance gave us the desired results, but it also
may have just unintentionally broken 20 other pieces of UI elsewhere.

Because objects don’t belong to any one specific component, and can
underpin several vastly different components, it is incredibly risky to
ever modify one. This is why we should introduce a namespace, to let
other developers know that this class forms an abstraction and that any
changes here will be reflected in every object sitewide. The object
itself does not necessarily have anything to do with the
implementation-specific bit of the UI that you are trying to change.

By adding a leading `o-`{.highlighter-rouge} to the classes for our
objects, we can tell other developers about their universal nature, and
hopefully avoid ever having people binding onto them and breaking
things. If you ever see a class that begins with
`o-`{.highlighter-rouge}, alarm bells should ring and you should know to
stay well away from it.

<div class="highlighter-rouge">

``` {.highlight}
<blockquote class="o-media  testimonial">
</blockquote>
```

</div>

-   Objects are abstract.
-   They can be used in any number of places across the project—places
    you might not have even seen.
-   Avoid modifying their styles.
-   Be careful around anything with a leading `o-`{.highlighter-rouge}.

Component Namespaces: `c-`{.highlighter-rouge}
----------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.c-component-name[<element>|<modifier>] {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.c-modal {}

  .c-modal__title {}

.c-modal--gallery {}
```

</div>

Components are some of the safest types of selectors we will encounter.
Components are finite, discrete, implementation-specific parts of our UI
that most people (users, designers, developers, the business) would be
able to identify: “This is a button”; “This is the date picker”; etc.

Usually when we make changes to a Component’s ruleset, we will
immediately see those changes happening every- (and only) where we’d
expect. Unlike with Objects, changing the padding on the
`.c-modal__content`{.highlighter-rouge} should not affect anything else
in the site other than the content area of our modal. Where Objects are
implementation-agnostic, Components are implementation-specific.

If we revisit the previous example, and introduce the Object and
Components’ namespaces, we’d be left with this:

<div class="highlighter-rouge">

``` {.highlight}
<blockquote class="o-media  c-testimonial">
</blockquote>
```

</div>

Now I can tell *purely* from this HTML that any changes I make to the
`.o-media`{.highlighter-rouge} class may be felt throughout the entire
site, but any changes I make to the `.c-testimonial`{.highlighter-rouge}
ruleset will only modify testimonials, and nothing else.

-   Components are implementation-specific bits of UI.
-   They are quite safe to modify.
-   Anything with a leading `c-`{.highlighter-rouge} is a specific
    *thing*.

Utility Namespaces: `u-`{.highlighter-rouge}
--------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.u-utility-name {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.u-clearfix {}
```

</div>

You will most likely be familiar with the Utility notation because of
[SUIT](https://github.com/suitcss/utils). Utilities are complete [single
responsibility](http://csswizardry.com/2012/04/the-single-responsibility-principle-applied-to-css/)
rules which have a very specific and targeted task. It is also quite
common for these rules’ declarations to carry
`!important`{.highlighter-rouge} so as to guarantee they beat other less
specific ones. They do one thing in a very heavy-handed and inelegant
way. They are to be used as a last resort when no other CSS hooks are
available, or to tackle completely unique circumstances, e.g. using
`.u-text-center`{.highlighter-rouge} to centrally align one piece of
text once and once only. They are only one step away from inline styles,
so should be used sparingly.

Because of their heavy-handed approach, their global reusability, and
their exceptional use-case, it is incredibly important that we signal
Utilities to other developers. We do not want anyone trying to bind onto
these in future selectors. Take the following example, which actually
happened on a project I worked on. A number of months into a project, a
developer wrote this bit of CSS:

<div class="highlighter-rouge">

``` {.highlight}
.footer .text-center {
  font-size: 75%;
}
```

</div>

Here we can see a problem: the `.text-center`{.highlighter-rouge} class
now has two responsibilities when it appears anywhere inside
`.footer`{.highlighter-rouge}. It now has side effects, which are
something that Utilities should never, ever have.

By using a namespace, we can introduce a simple and unbreakable rule: if
it begins with `u-`{.highlighter-rouge}, never reassign to it.

Utilities should be defined once, and never need changing.

Another problem that the Utility namespace solves is that it actually
lets people know that there is a heavyweight rule being applied to the
section of the DOM. It will help explain why certain things might be
happening and hard to override. Take this example:

<div class="highlighter-rouge">

``` {.highlight}
<div class="font-size-large">
  ...

  <blockquote class="pullquote">
  </blockquote>

  ...
</div>
```

</div>

A developer inheriting this might be confused as to why the
`blockquote`{.highlighter-rouge}’s font size is different to what they
expected. This is because it’s inheriting the font size from a
`.font-size-large`{.highlighter-rouge} class used a little further up
the DOM tree. By adding a little more clarity to our classes, we can
more quickly identify any potential offenders: “Ah, here’s a Utility,
that must be what’s causing it.” (This is actually a fairly good example
of why we should use Utilities sparingly.)

<div class="highlighter-rouge">

``` {.highlight}
<div class="u-font-size-large">
  ...

  <blockquote class="c-pullquote">
  </blockquote>

  ...
</div>
```

</div>

Please see this post’s sister article [Immutable
CSS](http://csswizardry.com/2015/03/immutable-css/) for more detail on
these kinds of rule.

-   Utilities are style heavyweights.
-   Alert people as to their existence.
-   Never reassign to anything that carries a leading
    `u-`{.highlighter-rouge}.

Theme Namespaces: `t-`{.highlighter-rouge}
------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.t-theme-name {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.t-light {}
```

</div>

When we work with [Stateful
Themes](https://speakerdeck.com/csswizardry/4half-methods-for-theming-in-s-css?slide=29)
(that is to say, themes that we toggle on and off) we normally do so by
adding a class to the `body`{.highlighter-rouge} element. Examples of
this approach to theming include style-switchers (a user can toggle
between different themes) and sub-sections of a site (all blog posts
have one theme colour, all news pages have another theme colour, etc.).
We simply add a class high up the DOM which then invokes that theme for
that particular page.

A simple way to denote any theme-related classes is to simply prepend
them with `t-`{.highlighter-rouge}. Seeing a `t-`{.highlighter-rouge}
class in your HTML should tell you that “Ah, right, the view probably
looks the way it currently does because we have a theme invoked.”

Now, all of the namespaces we’ve looked at so far are mainly of use to
us in our markup, but Theme namespaces are helpful in both our HTML and
our CSS. Seeing, for example, `.t-light`{.highlighter-rouge} in our
markup tells us that the entire DOM has a current state applied to it,
which is important to know whilst debugging. Seeing that class in our
CSS also tells us a lot: it helps to sandbox and isolate any chunks of
theme-related CSS inside namespaced rulesets:

<div class="highlighter-rouge">

``` {.highlight}
.c-btn {
  display: inline-block;
  padding: 1em;
  background-color: #333;
  color: #e4e4e4;

  .t-light & {
    background-color: #e4e4e4;
    color: #333;
  }

}
```

</div>

Here we can see that our buttons have a light grey text colour on top of
a dark grey background, but when we invoke the
`.t-light`{.highlighter-rouge} theme, those colours invert. Here we are
encapsulating the style information, which means that finding,
debugging, and modifying Theme rules becomes much simpler.

-   Theme namespaces are very high-level.
-   They provide a context or scope for many other rules.
-   It’s useful to signal the current condition of the UI.

Scope Namespaces: `s-`{.highlighter-rouge}
------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.s-scope-name {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.s-cms-content {}
```

</div>

Scoped contexts in CSS solve a very specific and particular problem:
please be entirely certain that you actually have this problem before
employing Scopes, because they can be misused and end up leading to
actively bad CSS.

Oftentimes it can be useful to set up a brand new styling context for a
particular section of your UI. A perfect example of this is areas of
user-generated content, where some long-form/prose HTML has come from a
CMS. The styling of this kind of content usually differs from the more
app-like UI around it. You may have a class-heavy UI architecture to
provide complex pieces of design like navigations, buttons, modals,
etc., and inside all of this you may have a simple blog post which is
populated via a CMS where the user writes plain text and cannot add any
classes or complexity.

For a really terse but effective example of Scoping styles, see [David
Bushell](https://twitter.com/dbushell)’s [Scoping Typography
CSS](http://dbushell.com/2012/04/18/scoping-typography-css/).

You **might** want to style this free-form text differently from the
rest of the surrounding UI, so you *might* employ a scoping context. For
example:

<div class="highlighter-rouge">

``` {.highlight}
<nav class="c-nav-primary">
  ...
</nav>

<section class="s-cms-content">

  <h1>...</h1>

  <p>...</p>

  <p>...</p>

  <ul>
    ...
  </ul>

  <p>...</p>

</section>

<ul class="c-share-links">
  ...
</ul>

<a href="" class="c-btn  c-btn--primary">Next article...</a>
```

</div>

Everything inside the `.s-cms-content`{.highlighter-rouge} is
inaccessible: we can’t get at the DOM to add any classes to the nodes
inside of it, so we *might* begin styling via a Scope. That *might* look
something like this:

<div class="highlighter-rouge">

``` {.highlight}
/**
 * Create a new styling context for any free-text CMS content (blog posts,
 * news pages, etc.).
 *
 * 1. Use a larger and more readable typeface for continuous prose.
 * 2. Force all headings to have the same appearance, regardless of their
 *    hierarchy.
 * 3. Make links inside long text more apparent.
 */
.s-cms-content {
  font: 16px/1.5 serif; /* [1] */

  h1, h2, h3, h4, h5, h6 {
    font: bold 100%/1.5 sans-serif; /* [2] */
  }

  a {
    text-decoration: underline; /* [3] */
  }

}
```

</div>

I cannot stress the word *might* enough here. [Nesting selectors is
bad](http://cssguidelin.es/#nesting): it leads to location-based
styling, meaning that styles are now tightly coupled to DOM structure;
it prevents people from being able to opt into styles, because nested
selectors are very dictatorial (i.e. “this **will** happen if you put
that in there”); having a type selector as a Key Selector creates very
greedy selectors, which can match more of the DOM than you intend; and
our specificity gets increased, meaning our Scope will override
previously defined styles, and in turn the Scope itself becomes harder
to override.

There’s a really good example we can grab from the Sass above. When
compiled, that code will give us this selector:
`.s-cms-content a {}`{.highlighter-rouge}. This selector is in charge of
adding underlines to links, and is also of a higher specificity than a
selector like `.c-btn {}`{.highlighter-rouge}. This means that if we
were to put a button inside of this Scope, it would get an
underline—this is something we probably don’t want. This simple example
outlines the potential for problems when working with Scopes, so tread
carefully.

Please make triple sure that that you need to employ a Scope before you
start writing lots of nested selectors. If you are unsure, it may be
best to err on the side of caution and leave Scopes out entirely.

Warnings aside, the actual `s-`{.highlighter-rouge} namespace becomes
incredibly useful for signalling to developers that an entire area of
the DOM is subject to one big caveat. Anything we see styled in here
might have an extra layer of styling applied to it in a pretty
opinionated and greedy manner.

-   Scopes are pretty rare: make triple sure you need them.
-   They rely entirely on nesting, so make sure people are aware of
    this.

Stateful Namespaces: `is-`{.highlighter-rouge}/`has-`{.highlighter-rouge}
-------------------------------------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.[is|has]-state {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.is-open {}

.has-dropdown {}
```

</div>

Stateful namespaces are lovely. They come from
[SMACSS](https://smacss.com/book/type-state), and they tell us about
short-lived or temporary states of the UI that need styling accordingly.

When looking at a piece of interactive UI (e.g. a modal overlay) through
developer tools, we’ll probably spend some time toggling things on and
off. Being able to see classes like `.is-open`{.highlighter-rouge}
appear and disappear in the DOM is a highly readable and very obvious
way of learning about state:

<div class="highlighter-rouge">

``` {.highlight}
<div class="c-modal  is-open">
  ...
</div>
```

</div>

It’s also incredibly handy in our CSS to tell people possible states
that a piece of UI can exist in, for example:

<div class="highlighter-rouge">

``` {.highlight}
.c-modal {
  ...

  &.is-open { ... }

}


  .c-modal__content {
    ...

    &.is-loading { ... }

  }
```

</div>

These classes work by chaining other classes, for example
`.c-modal.is-open`{.highlighter-rouge}. This heightened specificity
ensures that the State always takes prominence over the default styling.
It also means that we would never see a bare Stateful class on its own
in a stylesheet: it must always be chained to something.

The way in which States are different to BEM’s Modifiers is that States
are temporary. States (can) change from one moment to the next, perhaps
based on user action (e.g. `.is-expanded`{.highlighter-rouge}) or from
changes that are being pushed from a server (e.g.
`.is-updating`{.highlighter-rouge}).

-   States are very temporary.
-   Ensure that States are easily noticed and understood in our HTML.
-   Never write a bare State class.

Hack Namespaces: `_`{.highlighter-rouge} {#hack-namespaces-}
----------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
._<namespace>hack-name {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
._c-footer-mobile {}
```

</div>

In certain and usually quite rare circumstances, we might need to add a
class to our markup purely in order to help us hack or override
something. If we ever do that, we need to signal that this class is
hacky, it’s (hopefully) quite temporary, we want to get rid of it at
some point, therefore do not bind onto, reuse or otherwise interface
with it.

The reason for the leading underscore is simply to mirror the paradigm
of private variables in programming languages. Variables that are
private to the program should not be relied upon or reused by other
developers, and that’s the same with our Hack classes.

These types of class are pretty easy to spot in our codebase, so any
hacks will become very apparent, which is a [good
thing](http://csswizardry.com/2013/04/shame-css/).

<div class="highlighter-rouge">

``` {.highlight}
@media screen and (max-width: 30em) {

  /**
   * We need to force the footer to be a fixed height on smaller screens.
   */
  ._c-footer-mobile {
    height: 80px;
  }

}
```

</div>

-   Hacks are ugly—give them ugly classes.
-   Hacks should be temporary, do not reuse or bind onto their classes.
-   Keep an eye on the number of Hacks classes in your codebase.

JavaScript Namespaces: `js-`{.highlighter-rouge}
------------------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.js-component-name {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.js-modal {}
```

</div>

JavaScript namespaces are pretty common now, and most people tend to use
them. The idea is that—in order to properly separate our concerns—we
should never have styling and behaviour bound to the same hooks. To bind
both technologies onto the same hook means we can’t have one without the
other: our UI becomes all-or-nothing, which makes it very opinionated
and inflexible.

When I worked at [Sky](http://csswizardry.com/case-studies/bskyb/), we
had an incident where a developer had built a text-callout UI component
that had a distinct appearance, and some behaviour to fade text in and
out of it. A Product Owner asked that we reuse the same piece of UI
elsewhere, but we didn’t need to fade multiple pieces of text in and
out; it was just going to say the same thing all the time. Because the
component had been built with JS and CSS binding onto the same hook, it
meant that I couldn’t have a configuration of the component with its
look and feel but without its behaviour. It took a chunk of refactoring
to fix, and it could have been avoided simply by binding onto separate
hooks.

It also means that we can work a lot more safely. It means that CSS
developers can work and refactor freely without the worry that they will
break some JS, and vice versa. It separates our concerns and leaves each
team with its own hooks for its own purposes.

It’s probably also worth noting that because the JS namespace has
nothing at all to do with CSS, its format should be determined by your
JS engineers. If your JS team’s naming convention for variables etc. is
camel case, then they should be allowed to choose JS hooks like
`.jsModal`{.highlighter-rouge} if they so desire.

-   JavaScript and CSS are separate concerns—use separate hooks for
    them.
-   Giving different teams/roles different hooks makes for safer
    collaboration.

QA Namespaces: `qa-`{.highlighter-rouge}
----------------------------------------

Format:

<div class="highlighter-rouge">

``` {.highlight}
.qa-node-name {}
```

</div>

Example:

<div class="highlighter-rouge">

``` {.highlight}
.qa-error-login {}
```

</div>

An unusual, but potentially very useful namespace is this one, for your
QA team. When running automated UI tests with something like
[Selenium](http://www.seleniumhq.org/), or a headless browser, it is
quite common to do something like:

1.  Visit `site.dev/login`{.highlighter-rouge}
2.  Enter an incorrect username.
3.  Enter an incorrect password.
4.  Expect to see an error appear in the DOM.

I’ve had problems before where the authors of these automated UI tests
were binding onto CSS classes: e.g. “Does
`.message--error`{.highlighter-rouge} appear in the DOM?” The problem
with these tests looking out for style hooks is that simply refactoring
your CSS to use a different name can cause a test to fail, even if the
functionality is exactly the same. In a similar vein to our JS hooks,
automated UI tests should not be reliant on CSS classes. To do so breaks
our separation of concerns.

What we need to do is have the QA team bind onto a suite of their own
classes that we leave well alone. This means that if we start out with
this:

<div class="highlighter-rouge">

``` {.highlight}
<strong class="message  error  qa-error-login">
```

</div>

…and we refactor those nasty `.message`{.highlighter-rouge} and
`.error`{.highlighter-rouge} classes, we should be left with something
like this:

<div class="highlighter-rouge">

``` {.highlight}
<strong class="c-message  c-message--error  qa-error-login">
```

</div>

We can make all of the CSS changes we like, as long we we ensure that
the QA team’s hook stays in place.

-   Binding automated UI tests onto style hooks is too inexplicit—don’t
    do it.
-   Bind tests onto dedicated test classes.
-   Ensure that any UI refactoring doesn’t affect the QA team’s hooks.
