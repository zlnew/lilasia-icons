# Lilasia Icons

This package provides a simple Vue.js component to easily incorporate Material Symbols icons into your project. Material Symbols are a versatile set of icons suitable for various applications.

## Installation

You can install the package via npm:

```sh
npm install lilasia-icons
```

or via yarn:

```sh
yarn add lilasia-icons
```

## Usage

First, import the `Icon` component into your Vue application:

```vue
<script setup lang="ts">
import 'lilasia-icons/dist/style.css'
import Icon from 'lilasia-icons'
</script>

<template>
  <div>
    <Icon name="home" />
  </div>
</template>
```

## Props

The `Icon` component accepts the following props:

- `name` (string, required): The name of the Material Symbol icon.
- `filled` (boolean, optional): Whether the icon should be filled. Default is `false`.
- `weight` (number | string, optional): The weight of the icon. Default is `400`.
- `grade` (number | string, optional): The grade of the icon. Default is `0`.
- `opticalSize` (number | string, optional): The optical size of the icon. Default is `48`.
- `size` (number | string, optional): The size of the icon. Default is `24`.

## Example

```vue
<script setup lang="ts">
import 'lilasia-icons/dist/style.css'
import Icon from 'lilasia-icons'
</script>

<template>
  <div>
    <Icon name="add_circle" :size="30" filled />
    <Icon name="delete" :size="30" />
    <Icon name="search" :size="30" />
  </div>
</template>
```
