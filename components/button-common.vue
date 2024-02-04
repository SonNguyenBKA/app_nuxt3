<template>
  <button class="btn-common" :style="{...styles, ...optionColors}"
          :class="[classExtra, props.typeBtn]"
          :disabled="isDisabled"
          @click="$emit('clickBtn')">
    <div v-if="!props.loading.isLoading" class="btn-common__child">
      <slot/>
    </div>
    <div v-else class="btn-common__child">
      <div class="flex justify-center items-center gap-1">
        <img v-if="props.loading.show" class="icon-loading" :src="props.loading.icon" alt="loading">
        <slot v-if="props.loading.isText" />
      </div>
    </div>
  </button>
</template>

<script setup>
import {checkTrueAbs} from "~/utils/utils.js";
import iconLoading from "@/assets/images/icons/icon-loading.svg";
definePageMeta({
  name: 'ButtonCommon'
})
const props = defineProps({
  classExtra: {type: String, default: ''},
  borders: {type: Array, default: () => ['4px', '1px']}, // radius -- width
  deg: {type: String, default: '90deg'},
  isDisabled: {type: Boolean, default: false},
  typeBtn: {type: String, default: 'option1'},
  colors: {type: Object, default: () => {}},
  loading: {type: Object, default: () => ({
      isLoading: false,
      show: false,
      icon: iconLoading,
      isText: false
    })}
})
// const checkLoading = computed(() => {
//   return checkTrueAbs(props.loading)
// })
const optionColors = computed(() => {
  let colors = null
  const colorsList = {
    option1: {
      default: {
        borders: ['#1BBEF5FF', '#1BECA6FF'],
        bgs: ['#1BECA633', '#1BECA633'],
        text: '#FFF'
      },
      hover: {
        borders: ['#1BBEF5FF', '#1BECA6FF'],
        bgs: ['transparent', 'transparent'],
        text: '#FFF'
      },
      pressed: {
        borders: ['#1BBEF5FF', '#1BECA6FF'],
        bgs: ['#1BBEF5FF', '#1BECA6FF'],
        text: '#627784'
      },
      disabled: {
        borders: ['#122126', '#122126'],
        bgs: ['#122126', '#122126'],
        text: '#627784'
      }
    },
    option2: {
      default: {
        borders: ['transparent', 'transparent'],
        bgs:  ['transparent', 'transparent'],
        text: '#AEE5EA'
      },
      hover: {
        borders: ['#225D67', '#225D67'],
        bgs: ['#225D67', '#225D67'],
        text: '#7FC8D2'
      },
      pressed: {
        borders: ['#122126', '#122126'],
        bgs: ['#122126', '#122126'],
        text: '#627784'
      },
      disabled: {
        borders: ['#122126', '#122126'],
        bgs: ['#122126', '#122126'],
        text: '#627784'
      }
    }
  }
  console.log(props.colors)
  switch (props.typeBtn) {
    case 'option1':
      colors = colorsList.option1
          break
    case 'option2':
      colors = colorsList.option2
      break
    case 'option-custom':
      colors = props.colors
      break
    default:
      colors = colorsList.option1
      break
  }
  return {
    '--color-border-default-1': colors.default.borders[0],
    '--color-border-default-2': colors.default.borders[1],
    '--color-bg-default-1': colors.default.bgs[0],
    '--color-bg-default-2': colors.default.bgs[1],
    '--color-text-default': colors.default.text,

    '--color-border-hover-1': colors.hover.borders[0],
    '--color-border-hover-2': colors.hover.borders[1],
    '--color-bg-hover-1': colors.hover.bgs[0],
    '--color-bg-hover-2': colors.hover.bgs[1],
    '--color-text-hover': colors.hover.text,

    '--color-border-pressed-1': colors.pressed.borders[0],
    '--color-border-pressed-2': colors.pressed.borders[1],
    '--color-bg-pressed-1': colors.pressed.bgs[0],
    '--color-bg-pressed-2': colors.pressed.bgs[1],
    '--color-text-pressed': colors.pressed.text,

    '--color-border-disabled-1': colors.disabled.borders[0],
    '--color-border-disabled-2': colors.disabled.borders[1],
    '--color-bg-disabled-1': colors.disabled.bgs[0],
    '--color-bg-disabled-2': colors.disabled.bgs[1],
    '--color-text-disabled': colors.disabled.text,
  }
})
const styles = {
  '--border-radius': props.borders[0],
  '--border-width': props.borders[1],
  '--deg': props.deg
}
</script>

<style lang="scss" scoped>
.btn-common {
  @apply relative [width:inherit] h-full;
  background: linear-gradient(var(--deg), var(--color-bg-default-1) 0%, var(--color-bg-default-2) 100%);
  border-radius: var(--border-radius);
  transition: 0.5s;
  &__child {
    @apply py-3 px-6 min-h-[50px] w-full text-[24px] font-bold;
    color: var(--color-text-default);
  }
  &::before {
    content: "";
    position: absolute;
    z-index: 2;
    inset: 0;
    border-radius: var(--border-radius);
    padding: var(--border-width);
    background: linear-gradient(var(--deg), var(--color-border-default-1) 0%, var(--color-border-default-2) 100%);
    -webkit-mask: linear-gradient(#fff 0 0) content-box,
    linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
  }

  &:hover {
    background: linear-gradient(var(--deg), var(--color-bg-hover-1) 0%, var(--color-bg-hover-2) 100%);
    .btn-common__child {
      color: var(--color-text-hover);
    }
  }

  &:active {
    background: linear-gradient(var(--deg), var(--color-bg-pressed-1) 0%, var(--color-bg-pressed-2) 100%);
    &:before {
      background: linear-gradient(var(--deg), var(--color-border-pressed-1) 0%, var(--color-border-pressed-2) 100%);
    }
    .btn-common__child {
      color: var(--color-text-pressed);
    }
  }

  &:disabled {
    background: linear-gradient(var(--deg), var(--color-border-disabled-1) 0%, var(--color-border-disabled-2) 100%);
    cursor: not-allowed;

    &:before {
      background: linear-gradient(var(--deg), var(--color-border-disabled-1) 0%, var(--color-border-disabled-2) 100%);
    }

    .btn-common__child {
      color: var(--color-text-disabled);
    }
  }
}

</style>