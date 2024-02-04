<template>
  <div class="test-page w-full p-[100px] min-h-[100dvh]">
    <ButtonCommon class-extra="!w-[200px]" type-btn="option1" :is-disabled="isDisabled" @clickBtn="isDisabled = !isDisabled">
      <div class="flex items-center justify-center gap-1">
        Toggle Disabled
      </div>
    </ButtonCommon>
    <ButtonCommon class-extra="!w-[200px]" type-btn="option1" :is-disabled="isDisabled">
      <div class="flex items-center justify-center gap-1">
        Primary Buttons
      </div>
    </ButtonCommon>
    <ButtonCommon class-extra="!w-[200px]" type-btn="option2" :is-disabled="isDisabled">
      <div class="flex items-center justify-center gap-1">
        Primary Buttons
      </div>
    </ButtonCommon>
    <ButtonCommon class-extra="!w-[200px]"
                  type-btn="option-custom"
                  :borders="['10px', '2px']"
                  deg="135deg"
                  :colors="colorsBtn"
                  :is-disabled="isDisabled"
    >
      <div class="flex items-center justify-center gap-1">
        Primary Buttons
      </div>
    </ButtonCommon>
    <ButtonCommon class-extra="!w-fit"
                  type-btn="option1"
                  :borders="['10px', '2px']"
                  :loading="stateLoading"
                  deg="135deg"
                  @clickBtn="clickLoading"
                  :is-disabled="isDisabled"
    >
        <span>Primary Buttons</span>
    </ButtonCommon>
  </div>
</template>

<script setup>
import iconLoading from '~/assets/images/icons/icon-loading.svg'
const isDisabled = ref(false)
const colorsBtn = {
  default: {
    borders: ['#FF512F', '#F09819'],
    bgs: ['rgba(255,81,47,0.45)', 'rgba(240,152,25,0.45)'],
    text: '#FFF'
  },
  hover: {
    borders: ['#FF512F', '#F09819'],
    bgs: ['transparent', 'transparent'],
    text: '#FFF'
  },
  pressed: {
    borders: ['#FF512F', '#FF512F'],
    bgs: ['#FF512F', '#FF512F'],
    text: '#627784'
  },
  disabled: {
    borders: ['#122126', '#122126'],
    bgs: ['#122126', '#122126'],
    text: '#627784'
  }
}
const stateLoading = ref({isLoading: true, show: false, icon: iconLoading, isText: true})
const clickLoading = () => {
  stateLoading.value = {
    isLoading: true, show: !stateLoading.value.show, icon: iconLoading, isText: !stateLoading.value.isText
  }
}
</script>

<style lang="scss" scoped>
.test-page {
  background-image: url("@/assets/images/pages/bg-test.png");
  @apply flex gap-4;
}
</style>