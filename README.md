# Vue + Tailwind CSS Multiselect Component (vue-tailwind-multiselect)

A ~128 LOC, simple, no bullshit Vue 3 component for a multiselect dropdown using Tailwind CSS. Designed for modern UI needs. No complicated CSS or Bootstrap dependencies 🎉

Support for clear selections, bulk actions, and dynamic option management.

by [@ben_makes_stuff](https://x.com/ben_makes_stuff)

[![Buy me a coffee](https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=☕&slug=ben_makes_stuff&button_colour=FFDD00&font_colour=000000&font_family=Lato&outline_colour=000000&coffee_colour=ffffff)](https://www.buymeacoffee.com/ben_makes_stuff)

## Usage

### Quickstart

1. Copy VueTailwindMultiselect.vue into your project.
1. Make sure the dependencies in `package.json` are installed (vue, @vueuse/components, and tailwind)
1. Use the following code example to get started:

```vue
<template>
  <VueTailwindMultiselect
    id="example-dropdown"
    name="example"
    :options="dropdownOptions"
    v-model="selectedValues"
    placeholder="Select options..."
    :required="true"
  />
</template>

<script setup>
import VueTailwindMultiselect from './VueTailwindMultiselect.vue'

const dropdownOptions = [
  { name: 'Option 1', value: 'option1' },
  { name: 'Option 2', value: 'option2' },
  { name: 'Option 3', value: 'option3' }
]

const selectedValues = ref([])
</script>
```

## Slots and Customization

No. Just copy and paste the component into your project and make as many edits as you want.

## Features

- **Multi-select**: Select multiple options at once.
- **Dropdown toggle**: Open/close dropdown with smooth state handling.
- **Dynamic options**: Render options dynamically from props.
- **Select all/clear**: Bulk actions for managing selections.
- **Accessibility**: Keyboard and screen reader friendly.
- **Disabled state**: Prevent interactions when necessary.

## Props

| Prop         | Type    | Required | Default  | Description                                      |
|--------------|---------|----------|----------|--------------------------------------------------|
| `options`    | Array   | ✅        | N/A      | Array of options with `{ name, value }` format. |
| `id`         | String  | ✅        | N/A      | Unique ID for the dropdown trigger.             |
| `name`       | String  | ✅        | N/A      | Name attribute for the checkboxes.              |
| `required`   | Boolean | ✅        | N/A      | Whether the selection is required.              |
| `disabled`   | Boolean | ❌        | `false`  | Disables the dropdown when `true`.              |
| `placeholder`| String  | ✅        | N/A      | Placeholder text when no options are selected.  |

## Model

This component uses `v-model` to bind the selected values as an array.

If the options are: `[{ name: 'Test', value: 'test_value' }]` and the user selects `Test`, the output will be the selected *value*: `['test_value']`

This conforms to the HTML spec for select elements, unlike other libraries that spit out the entire option object.
