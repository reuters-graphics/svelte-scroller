# svelte-scroller ([demo](https://svelte.dev/repl/76846b7ae27b3a21becb64ffd6e9d4a6?version=3))

A scroller component for Svelte apps that is modified for the Reuters Graphics rig.

## Installation

```bash
yarn add https://github.com/reuters-graphics/svelte-scroller
```


## Create your custom scroller component
Create a svelte component file in your project's js folder with the file extension `.svelte` and copy the example below into it. For ease of getting this working fast, name the file `mySvelteScroller.svelte`.

```html
<script>
  import Scroller from 'svelte-scroller';
  export let isEmbedded = false;

  let index, offset, progress;
</script>

<style>
  section { height: 100vh; }
</style>

<Scroller {isEmbedded} bind:index bind:offset bind:progress>
  <div slot="background">
    <p>
      This is the background content. It will stay fixed
      in place while the foreground scrolls over the top.
    </p>

    <p>Section {index + 1} is currently active.</p>
  </div>

  <div slot="foreground">
    <section>This is the first section.</section>

    {#if isEmbedded}
      Show something here for the embed version.
    {/if}

    <section>This is the second section.</section>

    {#if isEmbedded}
      For example, breaking apart the background scroll elements and inlcuding static images.
    {/if}
    
    <section>This is the third section.</section>

  </div>
</Scroller>
```

You must have one `slot="background"` element and one `slot="foreground"` element — see [composing with &lt;slot&gt;](https://svelte.dev/tutorial/slots) for more info.

## Import your component and get it working on a page in the rig

Copy the following code into your app.js file. This assumes that you have a variable there called `isEmbedded` per the default rig inititialization. 

Then, create a div in article.ejs with the a class name `YourElementSelectorHere`. You can use your own selector name, but update the selector in the target paramenter below to match.

```html
import MySvelteScroller from './mySvelteScroller.svelte';

new MySvelteScroller({
  target: document.querySelector('.YourElementSelectorHere'),
  props: {
    isEmbedded: isEmbedded
  }
})

```

## Parameters

The following parameters are available:

| parameter | default   | description                                                                                                                                                                                                         |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| top       | 0         | The vertical position that the top of the foreground must scroll past before the background becomes fixed, as a proportion of window height                                                                         |
| bottom    | 1         | The inverse of `top` — once the bottom of the foreground passes this point, the background becomes unfixed                                                                                                          |
| threshold | 0.5       | Once a section crosses this point, it becomes 'active'                                                                                                                                                              |
| query     | 'section' | A CSS selector that describes the individual sections of your foreground                                                                                                                                            |
| parallax  | false     | If `true`, the background will scroll such that the bottom edge reaches the `bottom` at the same time as the foreground. This effect can be unpleasant for people with high motion sensitivity, so use it advisedly |
| isEmbedded  | false     | If `true`, the fixed position functionality and background slot will be turned off and only the foreground slot will be displaed.  |


## `index`, `offset`, `progress` and `count`

By binding to these properties, you can track the user's behaviour:

* `index` — the currently active section
* `offset` — how far the section has scrolled past the `threshold`, as a value between 0 and 1
* `progress` — how far the foreground has travelled, where 0 is the top of the foreground crossing `top`, and 1 is the bottom crossing `bottom`
* `count` — the number of sections

You can rename them with e.g. `bind:index={i}`.



## Configuring webpack

If you're using webpack with [svelte-loader](https://github.com/sveltejs/svelte-loader), make sure that you add `"svelte"` to [`resolve.mainFields`](https://webpack.js.org/configuration/resolve/#resolve-mainfields) in your webpack config. This ensures that webpack imports the uncompiled component (`src/index.html`) rather than the compiled version (`index.mjs`) — this is more efficient.

If you're using Rollup with [rollup-plugin-svelte](https://github.com/rollup/rollup-plugin-svelte), this will happen automatically.



## License

[LIL](LICENSE)
