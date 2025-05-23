<script setup lang="ts">
import { computed, ref, watch } from "vue";
import DepthIndicator from "./DepthIndicator.vue";
import UpCaret from "./UpCaret.vue";
const props = defineProps<{
  color?: string;
  titleHtml: string;
  expanded: boolean;
  numChildren: number;
  groupStyle: "group" | "section";
  path: string;
}>();

const button = ref();

const emit = defineEmits<{
  (event: "click", e: MouseEvent): void;
  (event: "hovering", isHovering: boolean): void;
}>();

const styleObject = computed(() => {
  if (props.color) {
    return {
      backgroundColor: `rgba(${props.color}, 0.5)`,
    };
  }
  return {};
});

const click = (e: MouseEvent) => emit("click", e);
</script>

<template>
  <button
    ref="button"
    class="flex flex-row items-center dark:bg-opacity-60 bg-opacity-50"
    :class="{
      'bg-gray-500 dark:bg-gray-900': !color,
      'rounded-full px-2 py-px': groupStyle === 'group',
      'px-1': groupStyle !== 'group',
    }"
    :style="styleObject"
    @mouseover.passive="emit('hovering', true)"
    @mouseleave.passive="emit('hovering', false)"
  >
    <DepthIndicator :depth="path.split(',').length" />
    <div class="flex flex-row flex-grow items-center justify-center">
      <span
        class="eventTitle text-sm font-semibold whitespace-nowrap"
        v-if="titleHtml"
        v-html="titleHtml"
      >
      </span>
      <span
        class="eventTitle text-sm font-semibold whitespace-nowrap ml-1"
        v-if="!expanded"
        >({{ numChildren }})</span
      >
    </div>
    <UpCaret v-if="expanded" />
  </button>
</template>

<style scoped>
.eventTitle {
  font-family: "LXGW WenKai";
}
</style>
