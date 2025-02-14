<script setup lang="ts">
import { computed } from 'vue';
import AntIcons from '@/components/iNetCustom/Icons/AntIcons.vue';
import Lucide from '@/components/iNetCustom/Lucide/Lucide.vue';
import { LucideCreativeCommons } from 'lucide-vue-next';

export type Icon = keyof typeof LucideCreativeCommons;

interface IProps {
  iconName?: string;
  lucideIcon?: string;
  title?: string;
  iconRight?: boolean;
  htmlType?: string;
  loading?: boolean;
}

const props = withDefaults(defineProps<IProps>(), {
  iconName: '',
  title: '',
  iconRight: false,
  loading: false,
});

const iconName = computed(() => props.iconName as string);
const lucideIcon = computed(() => props.lucideIcon as any);
const title = computed(() => props.title as string);
const iconRight = computed(() => props.iconRight);
const htmlType = computed(() => props.htmlType || 'button');
const loading = computed(() => props.loading as boolean);
</script>

<template>
  <a-button
    class="c-btn"
    :html-type="htmlType"
    :class="{
      'flex-row-reverse': iconRight,
    }">
    <AntIcons :icon-name="iconName" v-if="iconName.length > 0 && !loading" />
    <Lucide :icon="lucideIcon" class="w-4 h-4 mr-1" v-if="props.lucideIcon && !loading" />
    <AntIcons icon-name="LoadingOutlined" v-if="loading" />
    <slot name="beforeTitle"></slot>
    {{ title }}
    <slot name="title" />
  </a-button>
</template>

<style scoped lang="scss"></style>
