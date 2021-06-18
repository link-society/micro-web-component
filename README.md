# Micro Web Component

This library creates an easy interface to build very simple static web components.

At [Link Society](https://link-society.com), we use it within our
[Hugo](https://gohugo.io) websites to add interactivity to some parts.

This library has no dependency but requires browser support of the following
standards:

 - [customElements.define](https://caniuse.com/mdn-api_window_customelements)
 - [document.importNode](https://caniuse.com/mdn-api_document_importnode)
 - [ES6 Class](https://caniuse.com/es6-class)
 - [HTML Templates](https://caniuse.com/template)

## Usage

To integrate the script (less than 1kb) within your HTML page, copy the following code:

```html
<script type="application/javascript" crossorigin="anonymous" src="https://link-society.github.io/micro-web-component/index.js"></script>
```

### Basic Web Component

First create your template:

```html
<template id="foobar-tmpl">
  <div class="foo"></div>
  <div class="bar"></div>
</template>
```

Finally, create your Web Component:

```html
<script type="application/javascript">
  MicroWebComponent.extends({
    tag: 'foobar',
    template: 'foobar-tmpl',
    render(instance) {
      instance.querySelector('.foo').innerHTML = 'foo';
      instance.querySelector('.bar').innerHTML = 'bar';
    }
  });
</script>
```

Now you can use your Web Component everywhere in the page:

```html
<foobar></foobar>
```

### With jQuery

If you included [jQuery](https://jquery.com), you are free to use it:

```html
<script type="application/javascript">
  MicroWebComponent.extends({
    tag: 'foobar',
    template: 'foobar-tmpl',
    render(instance) {
      $(instance).find('.foo').html('foo')
      $(instance).find('.bar').html('bar')
    }
  });
</script>
```

### With Hugo

If you are, like us, using Hugo to generate your static website and have some
data in the `data` folder:

```toml
# data/foobar.toml
[foo]

text = "hello there"

[bar]

text = "general kenobi"
```

You can include that data in your template:

```html
<template id="foobar-tmpl">
  <div class="container" data-foobar="{{ .Site.Data.foobar | jsonify }}">
    <div class="foo"></div>
    <div class="bar"></div>
  </div>
</template>
```

And fetch it within your web component:

```html
<script type="application/javascript">
  MicroWebComponent.extends({
    tag: 'foobar',
    template: 'foobar-tmpl',
    render(instance) {
      const foobar = JSON.parse(instance.querySelector('.container').dataset.foobar)
      // or with jQuery:
      //
      // const foobar = $(instance).find('.container').data('foobar')

      instance.querySelector('.foo').innerHTML = foobar.foo.text
      instance.querySelector('.bar').innerHTML = foobar.bar.text
    }
  });
</script>
```

## About

### License

This library is released under the terms of the [CC0 license](https://creativecommons.org/publicdomain/zero/1.0),
in other words: the public domain.

### What this library is

 - a simple Web Component implementation for static websites

### What this library is not or won't support

 - a replacement for Vue.JS / React
 - data binding
 - templating engine
