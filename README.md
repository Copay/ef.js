# ef.js
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](https://raw.githubusercontent.com/ClassicOldSong/ef.js/master/LICENSE) [![npm](https://img.shields.io/npm/dt/ef.js.svg?style=flat-square)](https://www.npmjs.com/package/ef.js) [![Build status](https://img.shields.io/travis/TheNeuronProject/ef.js.svg?style=flat-square)](https://travis-ci.org/ClassicOldSong/ef.js) [![FOSSA Status](https://app.fossa.io/api/projects/git%2Bhttps%3A%2F%2Fgithub.com%2FTheNeuronProject%2Fef.js.svg?type=shield)](https://app.fossa.io/projects/git%2Bhttps%3A%2F%2Fgithub.com%2FTheNeuronProject%2Fef.js?ref=badge_shield)
<img align="right" width="95" height="95" title="logo of ef" src="https://user-images.githubusercontent.com/10512422/39509793-19724b72-4e1a-11e8-8abc-231e48641242.png">

(maybe) An elegant HTML template engine & basic framework

ef.js is a static template framework for browsers with which you can write your UI without concerning about the logic, or writing logic without concerning about the UI.

ef.js also provides a simple template-engine assisting you in creating component modules with data binding at ease, while you may also apply your favourite template-engine compatible with ef.js's AST.

[Official Website (WIP)](https://ef.js.org)

Demo:
+ [TodoMVC](https://classicoldsong.github.io/todomvc-efjs/) - [repo](https://github.com/ClassicOldSong/todomvc-efjs)
+ [dbmon](https://classicoldsong.github.io/js-repaint-perfs/ef/opt.html) - [repo](https://github.com/ClassicOldSong/js-repaint-perfs)
+ The [official website](https://github.com/ClassicOldSong/ef.js.org) is also written with ef.js

Related projects:
+ [ef-core](https://github.com/TheNeuronProject/ef-core) - Core of ef.js (without parser)
+ [Neonclear](https://neon.atm.re) - An ef.js based progressive UI framework (WIP)
+ [bPlayer-ef](https://bplayer-ef.ccoooss.com) - The ef.js based web audio player
+ [eft-parser](https://github.com/TheNeuronProject/eft-parser) - Parser for eft templates
+ [rollup-plugin-eft](https://github.com/ClassicOldSong/rollup-plugin-eft) - Import .ef and .eft templates directly from your code, taking the advantage of ef.js into your project with zero configuration.
+ [SublimeEFMLHighlighter](https://github.com/ClassicOldSong/SublimeEFMLHighlighter) - EFML syntax highlighter for SublimeText3
+ [VSCodeEFMLHighlighter](https://marketplace.visualstudio.com/items?itemName=ClassicOldSong.efml) - EFML syntax highlighter for VSCode
+ [AtomEFMLHighlighter](https://atom.io/packages/efml) - EFML syntax highlighter for Atom
+ [PrismEFMLSyntax](https://github.com/ClassicOldSong/PrismEFMLSyntax) - EFML syntax for [Prism](http://prismjs.com/)

Community projects:
+ [EFML.vim](https://github.com/deluxghost/EFML.vim) by [deluxghost](https://github.com/deluxghost) - EFML (\*.ef, \*.eft) syntax highlighting for Vim
+ [Kefjs](https://github.com/cubesky/Kefjs) by [cubesky](https://github.com/cubesky) - A Kotlin/JS Wrapper for ef.js
+ [parcel-plugin-eft](https://github.com/oott123/parcel-plugin-eft) by [oott123](https://github.com/oott123) - Add ef.js template support for parcel bundler
+ [xml2efml](https://github.com/tcdw/xml2efml) - by [tcdw](https://github.com/tcdw) - Convert XML/HTML snippets to EFML

## CDN
[CDNJS](https://cdnjs.com/libraries/ef.js) | [jsDelivr](https://cdn.jsdelivr.net/npm/ef.js/dist/ef.min.js) | [UNPKG](https://unpkg.com/ef.js)

For dev versions:

[CDNJS](https://cdnjs.com/libraries/ef.js) | [jsDelivr](https://cdn.jsdelivr.net/npm/ef.js/dist/ef.dev.js) | [UNPKG](https://unpkg.com/ef.js/dist/ef.dev.js)

## Usage
``` javascript
import { create, onNextRender, inform, exec, bundle, setParser, parseEft, t, version } from 'ef.js'
// or you can use import * as ef from 'ef.js'

version // Version string of ef.js

setParser(someparser) // Change the default parser for ef.js so you can use a different type of template
parseEft('Your awesome template') // Get ef.js ast using default parser

const templateString = 'Your awesome template'
const ast = [/* AST which supported by ef */]

const data = {
  $data: {/* Binding data */},
  $methods: {/* Binding methods */}
}

const template1 = create(template)
const template2 = create(ast)
const template3 = t`
>component1
>component2
.Your awesome template
`

const component1 = new template1() // Create a component without data
const component2 = new template2(data) // Create a component and then updates it's data
const component3 = new template3(data, {component1, component2}) // Use component1 and component2 as custom components in template3

onNextRender(callback) // Cache operations to execute on next render
inform() // Tell ef to cache operations **USE WITH CARE**
exec() // Tell ef to execute all cached operations **USE WITH CARE**
exec(true) // Force execute cached operations **USE WITH CARE**
bundle(callback) // Wrapper for inform() and exec()

component1.$element // The DOM element of component1
component2.$element // The DOM element of component2

component1.$data.something = 'something new' // Update the binding data 'something'
component2.$methods.someMethod = ({e, value, state}) => {
  state.$data.something = 'something new'
  console.log('Event target', e.target)
  console.log('Value passed', value)
} // Update binding method

const logData = val => console.log('Subscribed data updated:', val)
component1.$subscribe('info.data', logData) // Observe a value
component1.$unsubscribe('info.data', logData) // Stop observing a value

component1.$update(data) // Update the whole component state

component1.$refs // Get all referenced nodes

component1.mountingPoint = component2 // Mount component2 to 'mountingPoint' on component1
component1.mountingPoint = null // Detach the mounted component

component1.listMP.push(componet2) // Mount component2 to list 'listMP' mounting point on component1

component1.$mount(...) // Mount method called by ef when trying to mount
compinent1.$umount() // Unmount from parent
component1.$destroy() // Destroy the component when not needed for more memory

```

### ef.js template language (EFML) format
EFML is a completely **logic-free** template language where no logic code is allowed, just as HTML does. EFML provides easy entry points in data binding and events handling.

EFML is the first language to be parsed into the ef-supporting AST.

**Note:** EFML is **strict to indents**. Wrong indents lead to a parsing error.

Here is an example.

```
Tree structure
Lines not starting with >#%@.|+- are considered as comments
The escape character of EFML is '&', for prevention of conflicts with js escapes.
Except for changes of the characters, all the usages remain the same in all versions.
this is a comment
Lines starting with '>' stand for a new tag
>div
  Lines with exactly one indent after a tag definition are considered all the things belonging to the defined tag
  Lines starting with '#' represent attributes
  Mustaches are used for data-bindings
  Content inside mustaches after '=' is to be the default value for the binding
  Content without mustaches stands for static data which you can not modify through ef.js
  #class = {{class = some class name}}
  #style = {{attr.style = background: #ECECEC}}
  #id = testdiv
  #some-attr = some text
  #content
  Here comes a example of properties, lines starting with '%'
  %title = Welcome, {{name}}
  %anotherProperty = text
  Lines starting with '@' stand for events
  Contents after ':' are considered as value passed to the handler
  @click = updateInfo:{{binding.value}} and static value
  modifier keys now can be bound easily
  @mousedown.shift.alt.ctrl.meta = select
  bind to keys is also easy
  @keypress.13 = submit
  use '.prevent' to `preventDefault`, '.stop' to `stopPropagation`, '.stopImmediate' to `stopImmediatePropagation`
  @keydown.8.prevent.stop = stopbackspace
  use '.capture' to capture an event
  @submit.capture.stopImmediate = submit
  Lines starting with '.' stand for text nodes
  .Name: {{name}}&nJob: {{job}}
  >pre
    Lines starting with '|' stand for multiline text
    |Line 1
    |Line 2
    |Line 3
  >br
  Lines starting with '-' stand for single node mounting point
  -node1
  '.' after a tag name stands for class names for the tag
  >p.some.{{binding.class}}.class.names
    '#' at the end of a tag name stands for the reference name of the node
    Mustaches after a dot will be bound to 'class' automatically
    >span.{{emergency = emergency}}#notice_box
      .Notice: {{notice}}
    .some text
  -node2
  Lines starting with '+' stand for multi node mounting points
  +list1
```

For standalone eft parser see [eft-parser](https://github.com/ClassicOldSong/eft-parser).

### Fragments

Since the version 0.9.0, ef.js has supported fragments. A normal template could only have one entry tag, while fragment templates can have multiple, and mounting points can even be put at root level:

```
>div
  .A root level tag
-rootLevelMountingPoint
>p
  .Another root level tag
+rootLevelListMountingPoint
.Root level text node
```

Use them just like normal templates, and behaviors are always the same. A single text node will be treated as fragments as well.

### Helpers

ef.js also provides some helpers for creating `Fragments` and `TextFragments`.

```typescript
new ef.Fragment(Array<EFComponents | string>)
```
This line creates a fragment containing given ef components, while

```typescript
new ef.TextFragment(string)
```
this line creates a single `TextFragment` which only contains the given text. Text on `TextFragment` components can be modified with `.text` property.

### Property Mapping

ef.js components are not always that easy to use, so after v0.9.5, a stable version of property mapping helper is bundled with ef.js. For documents, refer to [the comments](https://github.com/TheNeuronProject/ef-core/blob/master/src/lib/register-props.js#L53-L61) for now. It would be extremely useful when being used with JSX.

## JSX

JSX support has been attached to ef.js, since v0.9.0. Demo [here](https://codepan.net/gist/192a1870d23e05d775d3667389162e63).

### JSX Fragments

ef.js comes with support for JSX fragments. You can create fragments just like what you do in React:
```jsx
<>
  <h1>Hello JSX!</h1>
  <MyCustomComponent>Now ef.js comes with JSX fragment support!</MyCustomComponent>
</>
```

Note that JSX fragments are not the same as ef fragments. No ef bindings can be set on JSX fragments in the meantime.

### With Transpilers

**Babel:** As documented [here](https://babeljs.io/docs/en/babel-preset-react), you can customize your jsx pragama when using babel. For example:
```cson
{
  "presets": [
    [
      "@babel/preset-react",
      {
        "pragma": "ef.createElement", // default pragma is React.createElement
        "pragmaFrag": "ef.Fragment", // default is React.Fragment
        "throwIfNamespace": false // defaults to true
      }
    ]
  ]
}
```

**Buble:** Currently buble can only set custom `createElement` pragma, so you need to import `ef.js` as `React` currently in order to have JSX Fragment support. A [pull request on custom `Fragment` pragma](https://github.com/bublejs/buble/pull/199) has been merged but not yet released.

## Run a test
``` bash
git clone https://github.com/ClassicOldSong/ef.js.git
cd ef.js
npm install
npm run dev
```
Then you can test it out in the opened browser window.

## Build from source
``` bash
git clone https://github.com/ClassicOldSong/ef.js.git
cd ef.js
npm install
npm run build && npm run prod
```
Then you can get the fresh-built `ef.min.js` in the `dist` folder.

**Note:** All debugging messages are disabled in the production version.

## License
[MIT](http://cos.mit-license.org/)


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bhttps%3A%2F%2Fgithub.com%2FTheNeuronProject%2Fef.js.svg?type=large)](https://app.fossa.io/projects/git%2Bhttps%3A%2F%2Fgithub.com%2FTheNeuronProject%2Fef.js?ref=badge_large)
