# Web Components Open Styling

## Problem Statement

(Some synthesis of the generalized used cases along with a summary of
the insufficiencies of what we currently have.)

## Use Cases

### Developers want to style shadow roots externally

- This centres the page author.
- Related: page authors would like to style components with a richer API.

> A team using a CSS-based design system wants to integrate a third
> party web component into a page. They want to be able to style a
> complex structure within the shadow root.

### Developers want to use external styles in their shadow roots

- This centres the component author.
- Phrased another way: component authors would like to allow their
  element to receive default styles from the document.
- Related: app developers (authors of the page and components) would
  like to use a unified styling system like a utility CSS methodology
  to style elements inside and outside of shadow roots.

> A team building an application is using web components. They want to
> use a utility CSS library to style both the application and the
> components.

### Developers want to share styles between their shadow roots

- This centres component authors.

## Why are CSS Parts insufficient for the task?

- CSS Parts allow you to compose upon defaults set by the component. In
  some cases, it might be desirable to set the defaults for the
  component to compose.
- CSS Parts are one-dimensional for the page author.
- CSS Parts being accessed via a pseudo-element mean that they cannot
  easily be integrated into a legacy system.

## Why is slotted content insufficient for the task?

- Slotted content allows you to compose upon defaults set by the
  component. In some cases, it might be desirable to set the defaults
  for the component to compose.
- Slotted content is one-dimensional for the component author.

## Exploring where styles are cascaded.

### Host styles being cascaded before the shadow root

In this case, the styles act as easily overrideable defaults. A host can
choose to enforce styles by using `!important`. While it is notable that
this is very natural to how Cascade layers work, this might not always
be an option for legacy styles which might be read-only. It is possible
to enforce read-only styles as effectively `!important` through the use
of sub-layers and `all: revert-layer !important`.

### Host styles are cascaded in the same context/layer as the shadow root

In this case, the styles might not be easily overrideable as they now
contend with the styles in the shadow root in respect to specificity and
order of appearance. The latter would vary based on where the styles
were being set as it’s not a given. Since shadow roots generally don’t
need to deal with specificity, this might be an easy win for the host’s
styles which increases the challenge for the component author to express
what is important (see the past few decades of styling with CSS).

### Host styles are cascaded after the shadow root

In this case, they would work the same as how styles for `::part()`,
slotted content (i.e. from the host; not set with `::sloted()`),
and `:host()` work. In this case, they would effectively be a
replacement for those styling APIs since they would have not value.
