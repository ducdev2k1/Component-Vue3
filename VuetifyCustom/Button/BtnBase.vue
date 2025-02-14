<script setup lang="ts">
  import { EnumLocation } from '@/utils/my-enum';
  import { computed } from 'vue';

  defineOptions({
    inheritAttrs: false,
  });

  interface IProps {
    title?: string | any;
    iconRight?: boolean;
    loading?: boolean;
    icon?: string;
    iconMdi?: string;
    iconClass?: string;
    loadingClass?: string;
    tooltip?: string;
    locationTooltip?: EnumLocation;
    disabled?: boolean;
  }

  const props = withDefaults(defineProps<IProps>(), {
    title: '',
    loading: false,
    iconClass: 'w-4 h-4',
    loadingClass: 'w-4 h-4',
    locationTooltip: EnumLocation.top,
    disabled: false,
  });

  const title = computed(() => props?.title as string);
  const loading = computed(() => props?.loading as boolean);
  const icon = computed(() => props?.icon as any);
  const iconClass = computed(() => props?.iconClass as string);
  const loadingClass = computed(() => props?.loadingClass as string);
  const tooltip = computed(() => props?.tooltip as string);
  const locationTooltip = computed(() => props?.locationTooltip as EnumLocation);
  const disabled = computed(() => props?.disabled as boolean);
  const iconMdi = computed(() => props?.iconMdi as string);
</script>

<template>
  <v-btn
    class="c-btn"
    variant="elevated"
    :class="{
      'gap-1': title?.length > 0,
    }"
    v-bind="$attrs"
    :disabled="disabled">
    {{ title }}
    <template #prepend>
      <template v-if="!$slots.prepend">
        <Lucide v-if="icon?.length > 0 && !loading" :icon="icon" :class="iconClass" />
        <v-icon v-if="iconMdi?.length > 0 && !loading" :class="iconClass">{{ iconMdi }}</v-icon>
        <circular-loader v-if="loading" :loading-class="loadingClass" width="2" :class="loadingClass" />
      </template>
      <template v-else>
        <slot v-if="$slots.prepend && !loading" name="prepend" />
        <circular-loader
          v-if="loading"
          :loading-class="loadingClass"
          width="2"
          :class="loadingClass"
          color-loading="primary" />
      </template>
    </template>
    <template v-if="$slots.title">
      <slot name="title" />
    </template>
    <template v-if="$slots.content">
      <slot name="content" />
    </template>
    <template v-if="$slots.append">
      <slot name="append" />
    </template>
    <v-tooltip v-if="tooltip?.length > 0" :location="locationTooltip" :text="tooltip" activator="parent" />
  </v-btn>
</template>
