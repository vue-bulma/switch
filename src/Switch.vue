<template>
  <label class="switch" :class="classObject">
    <input
      ref="input"
      type="checkbox"
      v-model="checked"
      @change="input"
      v-bind="{
        id,
        name,
        value,
        disabled,
        required
      }"
    >
  </label>
</template>

<script>
export default {
  model: {
    prop: 'model',
    event: 'change'
  },
  props: {
    id: String,
    name: String,
    disabled: Boolean,
    required: Boolean,
    type: String,
    size: String,
    model: [String, Number, Boolean, Array],
    value: {
      type: [String, Number, Boolean],
      default: 'on'
    }
  },

  data() {
    const dataType = this.getDateType()
    return {
      checked: this.initCheck(this.model, this.value, dataType),
      dataType
    }
  },

  computed: {
    classObject() {
      const { type, size, checked } = this
      return {
        [`is-${type}`]: type,
        [`is-${size}`]: size,
        checked
      }
    }
  },

  methods: {
    getDateType() {
      return Array.isArray(this.model)
        ? 2
        : typeof this.model === 'boolean' ? 1 : 0
    },
    initCheck(model, value, t) {
      if (t === 2) {
        return model.includes(value)
      }

      if (t === 1) {
        return model
      }

      return model === value
    },
    set(checked, model, value, t) {
      if (t === 2) {
        const i = model.indexOf(value)
        const has = i !== -1
        if (checked && !has) {
          model.push(value)
        } else if (!checked && has) {
          model.splice(i, 1)
        }
        this.$emit('change', model)
      } else if (t === 1) {
        this.$emit('change', checked)
      } else {
        this.$emit('change', checked ? value : null)
      }
    },
    input() {
      this.set(this.checked, this.model, this.value, this.dataType)
    }
  }
}
</script>

<style lang="scss">
@import '~bulma/sass/utilities/initial-variables';
@import '~bulma/sass/utilities/derived-variables.sass';

.switch {
  --height: $size-normal;

  input {
    opacity: 0;
    display: inline-block;
    width: 100%;
    height: 100%;
    position: absolute;
    z-index: 1;
    cursor: pointer;
  }

  appearance: none;
  position: relative;
  outline: 0;
  border-radius: calc(0.8 * var(--height));
  width: calc(1.625 * var(--height));
  height: var(--height);
  background-color: $border;
  border: 1px solid $border;
  cursor: pointer;
  box-sizing: border-box;
  display: inline-block;
  -webkit-tap-highlight-color: transparent;

  &:before,
  &:after {
    content: ' ';
    position: absolute;
    top: 0;
    left: 0;
    height: calc(var(--height) - 2px);
    border-radius: calc((var(--height) - 2px) / 2);
    transition: 0.233s;
  }

  &:before {
    width: calc(1.625 * var(--height) - 2px);
    background-color: $grey-lighter;
  }

  &:after {
    width: calc(var(--height) - 2px);
    background-color: #fff;
    box-shadow: 0 2px 3px rgba(17, 17, 17, 0.1);
  }

  &.checked {
    border-color: $text;
    background-color: $text;
    &:before {
      transform: scale(0);
    }
    &:after {
      transform: translateX(calc(0.625 * var(--height)));
    }
  }

  // Colors
  @each $name, $pair in $colors {
    $color: nth($pair, 1);
    &.is-#{$name} {
      &.checked {
        border-color: $color;
        background-color: $color;
      }
    }
  }

  // Sizes
  &.is-small {
    --height: $size-small;
  }
  &.is-medium {
    --height: $size-medium;
  }
  &.is-large {
    --height: $size-large;
  }
}
</style>
