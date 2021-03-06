HTML `privacy-policy` custom element and its children, to display privacy policies.

# Development / edition

- open `./index.html` in a browser, refresh the page as change are made

> Alternatively, you can use
> [npm:serve](https://www.npmjs.com/package/serve) (installed
> globaly). This way you can use, `serve .` to serve the local folder.

This repository is using:

- HTML document, in which is the content of the policy, and its markup
- HTML custom-elements, to describe each element of the policy
- web-components (`./index.js`), to add features (ex: numbering of sections)

# Usage

You can import and define the elements link this in your .html files (also in a .js files).

```html
<script type="module">
	import {
	PrivacyPolicy,
	PrivacyPolicySection
	} from './index.js'

	customElements.define('privacy-policy', PrivacyPolicy)
	customElements.define('privacy-policy-section', PrivacyPolicySection)
</script>
```

The `./index.css` file is optional, but can provide a default styling for a policy.

The `./index.html` shows the actual content (and markup) or the privacy policy.

## custom-elements list

In order to describe the content of the privacy policy, a few HTML custom elements are defined.

This way it is possible:
- use them as wrappers, arrount for the content elements of the
  policy, to describe it
- to target them with css, and lay out the content
- to define some of these custom-elements as web-components, and add
  javascript based functionalities to these markup elements

### General remarks

Be sure to use minimal HTML elements when creating/updating/deleting
content; for example:

```html
<privacy-policy-section number="a">
  <p>This extra paragraph HTML tag is not needed.</p>
</privacy-policy-section>

<privacy-policy-section number="b">
  No extra paragraph here, simpler to read.
</privacy-policy-section>

<privacy-policy-section number="b">
  <p>Here we choose to use two paragraph; first one.</p>
  <p>And the second one here; followed by an organised list (with numbers).</p>
  <ol>
    <li>One list element</li>
    <li>Second list element</li>
  </ol>
</privacy-policy-section>
```

But be sure to wrap text in HTML element when needed:
- `<br/>` at the end of a line, to put a line break
- `<p></p>` element after each other, when a separation between text paragraphs is needed.
- `<ul>` & `<ol>` to list some elements, only at the bottom of the HTML tree
- `<a>` anchor elements to create links
- `<strong>` to display text in bold

> Note: other HTML elements should not be needed, such as `<div>`. New
> default HTML elements used (for semantic reasons), should be listed
> above.

### `<privacy-policy>`

The root element, defining the privacy policies displayed on a
page/site. There should be only one, per policy, with the list of
sections inside.

Parameters:

- `date` [string]: a text to be displayed as date; ex: 2020/14/11
- `name` [string] (default: `Privacy Policy`): a text, name of the "page", policy

Children:

- `privacy-policy-list`, can eventually be the only child element,
  inside this element; as a privacy policy is a list of policies.
- `privacy-policy-header`, [autogenerated] by this web-component (with
  name and date). Should not have to be added manually.

Example, in an `./index.html` file:
```html
<privacy-policy date="2020/14/11" name="Privacy Policy"></privacy-policy>
```

### `<privacy-policy-list>`

Parameters: none

Children:

- `privacy-policy-section`; only possible child, as policy list is a list of policy sections

A parent element, to display neatly its list of children, `privacy-policy-section`.

### `<privacy-policy-section>`

Parameters:

- `number` [string]: a text to be displayed in front of the section element; used visually as the number of the section; ex: `A`, `a`, `3`, `IV` (should be kept short to not brake the layout).

Children (one of each maximum, as direct child):

- `privacy-policy-section-number` [autogenerated], from the number parameters
- `privacy-policy-section-title`, to give a title to this section
- `privacy-policy-list`, a child policy list, to encapsulate
  "sub-sections" of policies, to this current section (the parent).

Examples:

```html
<privacy-policy-list>
  <privacy-policy-section number="I"></privacy-policy-section>
  <privacy-policy-section number="II"></privacy-policy-section>
  <privacy-policy-section number="III"></privacy-policy-section>
</privacy-policy-list>
```
```html
<privacy-policy-list>
  <privacy-policy-section number="1">
    <privacy-policy-section-title>
      A section title
    </privacy-policy-section-title>
    <privacy-policy-list>
      <privacy-policy-section number="a"></privacy-policy-section>
      <privacy-policy-section number="b"></privacy-policy-section>
    </privacy-policy-list>
  </privacy-policy-section>
  <privacy-policy-section number="2"></privacy-policy-section>
  <privacy-policy-section number="3"></privacy-policy-section>
</privacy-policy-list>
```

### `<privacy-policy-section-title>`

Used to show visually the title of a section. There should be only one per section, and it should be used as first element of a section.

Parameters: none

Children: none; just plain text (not even `<p>` tags)

Example:
```html
<privacy-policy-section number="I">
  <privacy-policy-section-title>
    The section title
  </privacy-policy-section-title>
  <privacy-policy-list>
    <privacy-policy-section number="1"></privacy-policy-section>
    <privacy-policy-section number="2"></privacy-policy-section>
  </privacy-policy-list>
</privacy-policy-section>
```

## autogenerated custom-elements

These additional elements are added by the other webcomponents, to generate the UI.X.

- they should not be added manually in the HTML markup
- they can be used to be targeted with CSS, for styling purpose

### `<privacy-policy-section-number>`

Inserted by the `privacy-policy-section` element inserts it from its
`number` parameter.

### `<privacy-policy-header>`

Inserted by the `privacy-policy` element, from its parameters.
