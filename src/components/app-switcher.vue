<template>
  <div
    class="dropdown primary"
    :class="{ 'dropdown--is-open': isopen }"
    @click.stop
  >
    <div class="dropdown__label" @click="toggle">
      <div class="inline-item">
        <img
          v-if="options.length !== 0"
          :src="`${publicPath}${currentOption.icon}`"
          :alt="currentOption.productName"
        />
        <img
          v-else
          :src="`${publicPath}manage-account.png`"
          alt="Manage Account"
        />
        <div class="inline-item product-namecontainer">
          {{
            options.length !== 0 ? currentOption.productName : `Mange Account`
          }}
        </div>
        <span v-if="options.length !== 0" class="dropdown__icon">
          <svg
            width="12"
            height="12"
            viewBox="0 0 12 12"
            fill="none"
            stroke="currentColor"
          >
            <!-- xmlns="http://www.w3.org/200/svg" -->
            <path
              d="M0.000137116 3.99998L4.9995 0L9.99984 3.99478"
              transform="translate(11 8) rotate(-180)"
            />
          </svg>
        </span>
      </div>
      <div
        v-if="options.length !== 0"
        v-show="isOpen"
        class="dropdown__options primary"
      >
        <div
          class="dropdown__options primary"
          :class="{ 'dropdown__option--is-current': index === currentIndex }"
          v-for="(option, index) in options"
          :key="option.productId"
          @click="selectItem(option, index)"
        >
          <div class="dropdown__option--hover-state">
            <div class="inline-item">
              <img
                :src="`${publicPath}${option.icon}`"
                :alt="option.productName"
              />
            </div>
            <div class="inline-item">
              {{ option.productName }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'AppSwitcher',
    props: {
      currentIndex: {
        type: Number,
        required: true,
      },
      options: {
        type: Array,
        required: false,
        default: () => [],
      },
    },
    data: () => ({
      isOpen: false,
      publicPath: process.env.BASE_URL,
    }),
    created() {
      var vm = this;
      document.addEventListener('click', function() {
        vm.isOpen = false;
      });
    },
    computed: {
      currentOption() {
        return this.options[this.currentIndex];
      },
    },
    methods: {
      toggle() {
        if (this.options.length !== 0) {
          this.isOpen = !this.isOpen;
          this.$emit('app-switcher-open', this.isOpen);
        }
      },
      selectItem(option, index) {
        this.isOpen = false;
        this.$emit('app-switcher-open', this.isOpen);
        this.$emit('option-click', option, index);
      },
    },
  };
</script>

<style lang="scss" scoped>
  $primary: #1c5786;

  .dropdown {
    position: relative;
    display: block;
    width: 240px;
    height: 64px;
    filter: brightness(90%);

    &.dropdown--is-open {
      filter: brightness(100%);
      box-shadow: 0 0 1px 0 rgba(0, 0, 0, 0.52),
        0 6px 6px -3px rgba(0, 0, 0, 0.07), 0 10px 14px 1px rgba(0, 0, 0, 0.11),
        0 4px 18px 3px rgba(0, 0, 0, 0.06);
    }
  }

  .dropdown__label {
    z-index: 1;
    position: relative;
    padding: 1em;
    cursor: pointer;
    user-select: none;
    font-weight: 900;
    text-overflow: ellipsis;

    .product-name-container {
      width: 128px;
      text-overflow: ellipsis;
      overflow: hidden;
      white-space: nowrap;
    }

    .dropdown__icon {
      display: inline-block;
      transform: rotate(0);
      transform-origin: 50% 50%;
      transition: transform 0.3s ease-in-out;
      vertical-align: middle;
    }
  }

  .dropdown--is-open .dropdown__icon {
    transform: rotate(180deg);
  }

  .dropdown__options {
    z-index: 10;
    position: absolute;
    top: 100%;
    left: 0;
    min-width: 100%;
    height: 1800px;
  }

  .dropdown__option {
    z-index: 1;
    position: relative;
    height: 50px;
    padding: 3px 8px;
    cursor: pointer;
    user-select: none;

    .dropdown__option--hover-state {
      height: 42px;
      width: 224px;
      padding: 6px 8px;
      border-radius: 4px;

      &:hover {
        background-color: darken($primary, 10%);
      }
    }

    .dropdown__option--is-current {
      font-weight: 900;
    }
  }

  .inline-item {
    display: inline-block;
    vertical-align: middle;
  }

  .inline-item img {
    width: 30px;
    margin-right: 10px;
    border: 1px solid rgba(255, 255, 255, 0.7);
    border-radius: 6px;
  }
</style>
