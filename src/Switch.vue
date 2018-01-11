<template>
  <label class="switch" :class="classObject">
    <input
      ref="input"
      type="checkbox"
      class="checkbox"
      v-model="checked"
      @change="input"
      v-bind="{
        id,
        name,
        value,
        disabled,
        required,
        class: inputClass
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
    inputClass: [String, Number, Array, Object],
    trueValue: true,
    falseValue: true,
    value: {
      type: [String, Number, Boolean],
      default: 'on'
    }
  },

  data() {
    const dt = this.getDateType()
    const t = this.getTrue()
    const f = this.getFalse()
    return {
      checked: this.initCheck(this.model, t, dt),
      dt,
      t,
      f
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
    getTrue() {
      return this.trueValue === undefined ? this.value : this.trueValue
    },
    getFalse() {
      return this.falseValue === undefined ? null : this.falseValue
    },
    initCheck(model, value, dt) {
      if (dt === 2) {
        return model.includes(value)
      }

      if (dt === 1) {
        return model
      }

      return model === value
    },
    set(checked, model, t, f, dt) {
      if (dt === 2) {
        const i = model.indexOf(value)
        const has = i !== -1
        if (checked && !has) {
          model.push(t)
        } else if (!checked && has) {
          model.splice(i, 1)
        }
        this.$emit('change', model)
      } else if (dt === 1) {
        this.$emit('change', checked)
      } else {
        this.$emit('change', checked ? t : f)
      }
    },
    input() {
      this.set(this.checked, this.model, this.t, this.f, this.dt)
    }
  },

  watch: {
    value(v) {
      const dt = this.getDateType()
      switch (dt) {
        case 2:
          this.checked = this.model.includes(v)
          break
        case 1:
          this.checked = v
          break
        default:
          this.checked = v === this.trueValue
      }
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
    display: inline-flex;
    width: 100%;
    height: 100%;
    // position: absolute;
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
  display: inline-flex;
  vertical-align: middle;
  align-items: center;
  justify-content: center;
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
