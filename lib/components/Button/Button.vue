<template>
  <button
    class="button"
    :class="classes"
    :disabled="disabled || loading"
    :type="type"
    v-on="$listeners"
  >
    <div class="button__content">
      <slot />
    </div>
    <div class="button__icon" v-if="$slots.icon">
      <slot name="icon" />
    </div>
    <div class="button__loader" v-if="loading">
      <v-spinner
        :color="inverted ? 'white' : 'secondary'"
        :size="spinnerSize.size"
        :stroke="spinnerSize.stroke"
      />
    </div>
  </button>
</template>

<script>
import VSpinner from '../Spinner';

export default {
  name: 'VButton',
  props: {
    type: {
      type: String,
      default: 'button',
    },
    appearance: {
      type: String,
      default: 'default',
      validator(appearance) {
        return (
          [
            'default',
            'primary',
            'secondary',
            'subtle',
            'danger',
          ].indexOf(appearance) > -1
        );
      },
    },
    size: {
      type: String,
      default: 'xl',
      validator(appearance) {
        return ['xs', 'sm', 'md', 'lg', 'xl'].indexOf(appearance) > -1;
      },
    },
    loading: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    iconFirst: {
      type: Boolean,
      default: false,
    },
    inverted: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    classes() {
      return [
        `button--${this.appearance}`,
        `button--size-${this.size}`,
        { 'button--loading': this.loading },
        { 'button--icon-only': !this.$slots.default },
        { 'button--icon-first': this.iconFirst },
        { 'button--inverted': this.inverted },
      ];
    },
    spinnerSize() {
      switch (this.size) {
        case 'sm':
          return { size: 16, stroke: 5 };
        case 'md':
          return { size: 18, stroke: 5 };
        case 'lg':
          return { size: 20, stroke: 5 };
        default:
          return { size: 24, stroke: 5 };
      }
    },
  },
  components: { VSpinner },
};
</script>

<style lang="postcss">
.button {
  @apply appearance-none text-base font-semibold bg-transparent inline-flex items-center rounded;
  cursor: pointer;
  height: 48px;
  justify-content: center;
  margin: 0;
  outline: 0;
  padding: 0 24px;
  position: relative;
  text-decoration: none;
  vertical-align: middle;
  white-space: nowrap;
}
.button[disabled] {
  @apply bg-control-disabled text-control-disabled;
  cursor: not-allowed;
}

.button__icon {
  display: flex;
  margin-left: 4px;
}

.button--default {
  @apply bg-control-default border-control border-control-default text-primary;
}
/* .button--default:focus {
  box-shadow: 0 0 0 2px var(--color-background-button-default-focus);
} */
.button--default:hover:not([disabled]) {
  @apply border-control-hover;
}
.button--default:active:not([disabled]) {
  @apply bg-control-default;
}
.button--default[disabled] {
  @apply border-none;
}

.button--subtle {
  @apply text-primary;
}
/* .button--subtle:focus {
  box-shadow: 0 0 0 2px var(--color-background-button-subtle-focus);
} */
.button--subtle:hover:not([disabled]) {
  @apply bg-control-subtle-hover;
}
.button--subtle:active:not([disabled]) {
  @apply bg-control-subtle-hover;
}
.button--subtle.button--inverted {
  color: white;
}
.button--subtle.button--inverted[disabled] {
  color: var(--color-control-typo-disabled);
  background-color: var(--color-control-bg-inverted);
}
.button--subtle.button--inverted:hover:not([disabled]) {
  background-color: var(--color-control-bg-inverted-hover);
}

.button--primary {
  @apply bg-control-primary text-white;
}
/* .button--primary:focus {
  box-shadow: 0 0 0 2px var(--color-background-button-primary-focus);
} */
.button--primary:hover:not([disabled]) {
  @apply bg-control-primary-hover;
}
.button--primary:active:not([disabled]) {
  @apply bg-control-primary;
}

.button--secondary {
  @apply bg-control-secondary text-primary;
}
/* .button--secondary:focus {
  box-shadow: 0 0 0 2px var(--color-background-button-primary-focus);
} */
.button--secondary:hover:not([disabled]) {
  @apply bg-control-secondary-hover;
}
.button--secondary:active:not([disabled]) {
  @apply bg-control-secondary;
}
.button--secondary.button--inverted {
  color: var(--color-typo-inverted);
  background-color: var(--color-control-bg-inverted);
}
.button--secondary.button--inverted:hover:not([disabled]) {
  background-color: var(--color-control-bg-inverted-hover);
}
.button--secondary.button--inverted[disabled] {
  color: var(--color-control-typo-disabled);
  background-color: var(--color-control-bg-inverted);
}

.button--danger {
  @apply bg-control-danger text-white;
}
/* .button--danger:focus {
  box-shadow: 0 0 0 2px var(--red-300);
} */
.button--danger:hover:not([disabled]) {
  @apply bg-control-danger-hover text-white;
}
.button--danger:active:not([disabled]) {
  @apply bg-control-danger;
}

.button[disabled].button--loading .button__content {
  visibility: hidden;
  opacity: 0;
}

.button__loader {
  position: absolute;
}

.button--icon-first {
  flex-direction: row-reverse;
}
.button--icon-first .button__icon {
  margin-left: 0;
  margin-right: 4px;
}

.button--icon-only {
  padding: 0;
  width: 48px;
}

.button--icon-only .button__icon {
  margin-left: 0;
  margin-right: 0;
}

.button--icon-only.button--size-sm {
  border-radius: 3px;
  width: 24px;
}
.button--icon-only.button--size-md {
  border-radius: 3px;
  width: 32px;
}
.button--icon-only.button--size-lg {
  width: 40px;
}
.button--icon-only.button--size-xl {
  width: 48px;
}

.button--size-xs {
  font-size: 14px;
  height: auto;
  padding: 0;
}
.button--size-sm {
  border-radius: 3px;
  font-size: 14px;
  height: 24px;
  padding: 0 4px;
}
.button--size-md {
  border-radius: 3px;
  height: 32px;
  padding: 0 8px;
}
.button--size-lg {
  height: 40px;
  padding: 0 16px;
}
.button--size-xl {
  height: 48px;
  padding: 0 24px;
}
</style>
