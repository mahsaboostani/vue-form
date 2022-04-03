<template>
  <component
    v-for="option in options"
    :key="option.value"
    :is="vertical ? 'div' : 'span'"
  >
    <BaseRadio
      :label="option.label"
      :value="option.value"
      :name="name"
      :modelValue="modelValue"
      @update:modelValue="$emit('update:modelValue', $event)"
      v-bind="$attrs"
    />
  </component>
</template>

<script>
import BaseRadio from "@/components/BaseRadio.vue";
export default {
  name: "BaseGroupRadio",
  components: {
    BaseRadio,
  },
  props: {
    options: {
      type: Array,
      reauired: true,
    },
    modelValue: {
      type: [String, Number],
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    vertical: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
