<template>
  <div>
    <svg
      viewBox="0 0 24 24"
      :width="size"
      :height="size"
      stroke="currentColor"
      stroke-width="2"
      fill="none"
      stroke-linecap="round"
      stroke-linejoin="round"
      class="css-i6dzq1"
    >
      <g color="#FF9E9E">
        <polyline points="3 6 5 6 21 6"></polyline>
        <path
          d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
        ></path>
      </g>
    </svg>
  </div>
</template>
<script>
export default {
  props: {
    size: {
      type: String,
      default() {
        return '24'
      },
    },
  },
}
</script>


