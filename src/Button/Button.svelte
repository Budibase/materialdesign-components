<script>
  import { onMount } from "svelte"
  import Icon from "../Common/Icon.svelte"
  import ripple from "../Common/Ripple.js"
  import ClassBuilder from "../ClassBuilder.js"

  const cb = new ClassBuilder("button", ["primary", "medium", "text"])

  export let _bb
  export let onClick

  export let variant = "text"
  export let colour = "primary"
  export let size = "medium"

  export let href = ""
  export let icon = ""
  export let trailingIcon = false
  export let fullwidth = false

  export let text = ""
  export let disabled = false

  onMount(() => {
    let ctx = _bb.getContext("BBMD:button:context")
    extras = [ctx]
  })

  let extras = ""
  let modifiers = {}
  let customs = { size, colour }

  if (!href) modifiers = { variant }

  $: props = { modifiers, customs, extras }

  $: blockClasses = cb.build({ props })
  const labelClass = cb.elem("label")

  const clicked = () => _bb.call(onClick)

  $: renderLeadingIcon = !!icon && !trailingIcon
  $: renderTrailingIcon = !!icon && trailingIcon
</script>

{#if href}
  <a class={blockClasses} {href} on:click={clicked}>
    <span class={labelClass}>{text}</span>
  </a>
{:else}
  <button
    use:ripple={{ colour }}
    class={blockClasses}
    class:fullwidth
    {disabled}
    on:click={clicked}>
    {#if renderLeadingIcon}
      <Icon context="button__icon" {icon} />
    {/if}
    <span class={labelClass}>{text}</span>
    {#if renderTrailingIcon}
      <Icon context="button__icon" {icon} />
    {/if}
  </button>
{/if}

<style>
  .mdc-button:not(.fullwidth) {
    width: fit-content;
  }
  .fullwidth {
    width: 100%;
  }
</style>
