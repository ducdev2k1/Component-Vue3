<script setup lang="ts">
  import { ref, computed } from 'vue';

  interface IAvatar {
    size?: number | string;
    fullName: string;
    onlyColor?: boolean;
  }

  const props = withDefaults(defineProps<IAvatar>(), {
    size: 44,
    fullName: '',
    onlyColor: false,
  });

  const size = computed(() => props.size);
  const defaultColor = ref<string>('#0A6CCB');
  const onlyColor = computed(() => props.onlyColor);

  // Hàm tạo màu theo tên
  const hashStringToColor = (str: string): string => {
    let hash = 0;
    for (let i = 0; i < str.length; i++) {
      hash = str.charCodeAt(i) + ((hash << 5) - hash);
    }
    const color = `#${(hash & 0x00ffffff).toString(16).padStart(6, '0')}`;
    return color;
  };

  // Màu nền dựa trên tên user
  const backgroundColor = computed(() => hashStringToColor(props.fullName));

  // Lấy chữ cái đầu từ fullName
  const initials = computed(() => {
    const nameParts = props.fullName.trim().split(' ');
    let name = nameParts.length > 1 ? nameParts[0][0] + nameParts[1][0] : nameParts[0].substring(0, 2);
    return name.toUpperCase();
  });
</script>

<template>
  <div class="c-avatar">
    <svg :width="size" :height="size" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
      <circle cx="50" cy="50" r="50" :fill="onlyColor ? defaultColor : backgroundColor" />
      <text
        x="50"
        y="55"
        text-anchor="middle"
        dominant-baseline="middle"
        fill="white"
        style="user-select: none"
        :style="`font-size: ${size}px;`">
        {{ initials }}
      </text>
    </svg>
  </div>
</template>

<style scoped>
  .c-avatar {
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }
</style>
