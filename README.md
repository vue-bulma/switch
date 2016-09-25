# Switch

Switch component for Vue Bulma.


## Installation

```
$ npm install vue-bulma-switch --save
```


## Examples

```vue
<template>
  <div>
    <p>
      <vb-switch type="success" size="large" :value="value" checked @change="updateValue"></vb-switch>
    </p>
    <p>
      {{ value }}
    </p>
  </div>
</template>

<script>
// do not use below code, because `Switch` is svg tag.
// import Switch from 'vue-bulma-switch'
import VbSwitch from 'vue-bulma-switch'

export default {
  components: {
    VbSwitch
  },

  data () {
    return {
      value: false

  methods: {
    updateValue (val) {
      this.value = val
    }
  }
}
</script>
```


## Badges

![](https://img.shields.io/badge/license-MIT-blue.svg)
![](https://img.shields.io/badge/status-stable-green.svg)

---

> [fundon.me](https://fundon.me) &nbsp;&middot;&nbsp;
> GitHub [@fundon](https://github.com/fundon) &nbsp;&middot;&nbsp;
> Twitter [@_fundon](https://twitter.com/_fundon)

