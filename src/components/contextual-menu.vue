<template>
  <div v-if="options !== null">
    <v-popover :placement="placement" offset="2" :trigger="trigger" :disabled="disabled">
      <div class="menu-toggle" :class="{ disabled }">
        {{ text }}
        <v-icon v-if="icon" :name="icon" />
        <slot />
      </div>

      <template slot="popover">
        <ul class="ctx-menu">
          <li v-for="(option, id) in options" :key="id">
            <button
              v-close-popover
              type="button"
              :class="{ disabled: option.disabled }"
              :disabled="option.disabled"
              @click.stop="optionClicked(option, id)"
            >
              <v-icon v-if="option.icon" :name="option.icon" size="18"></v-icon>
              {{ option.text }}
            </button>
          </li>
        </ul>
      </template>
    </v-popover>
  </div>
</template>

<script>
export default {
  name: "VContextualMenu",
  props: {
    text: {
      type: String,
      default: ""
    },
    icon: {
      type: String,
      default: "more_vert"
    },
    options: {
      type: [Array, Object],
      default: null
    },
    trigger: {
      type: String,
      default: "click"
    },
    placement: {
      type: String,
      default: "right-start"
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    optionClicked(option, id) {
      if (!option.disabled) this.$emit("click", id);
    }
  }
};
</script>

<style>
.v-popover .trigger {
  display: block !important;
}
</style>

<style lang="scss" scoped>
.menu-toggle {
  cursor: pointer;
  display: flex;
  align-items: center;

  i {
    color: var(--lighter-gray);
    transition: color var(--fast) var(--transition);
  }

  &:hover i {
    color: var(--dark-gray);
    transition: none;
  }

  &.disabled {
    color: var(--lighter-gray);
    &:hover i {
      color: var(--lighter-gray);
    }
  }
}
.ctx-menu {
  list-style: none;
  padding: 0;
  width: var(--width-small);
  li {
    display: block;

    &:not(:last-of-type) {
      border-bottom: 1px solid var(--lightest-gray);
    }
  }
  i {
    color: var(--light-gray);
    margin-right: 8px;
    transition: color var(--fast) var(--transition);
  }
  button {
    display: flex;
    align-items: center;
    padding: 8px 4px;
    color: var(--darker-gray);
    width: 100%;
    height: 100%;
    transition: color var(--fast) var(--transition);

    &.disabled {
      color: var(--lighter-gray);
      i {
        color: var(--lighter-gray);
      }

      &:hover {
        color: var(--lighter-gray);
        i {
          color: var(--lighter-gray);
        }
      }
    }

    &:hover {
      color: var(--accent);
      transition: none;
      i {
        color: var(--accent);
        transition: none;
      }
    }
  }
}
</style>
