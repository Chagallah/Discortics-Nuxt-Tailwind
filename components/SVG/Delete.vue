<template>
  <div>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      :width="size"
      :height="size"
      viewBox="0 0 17 17"
      fill="none"
      stroke="currentColor"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
      class="feather feather-x"
    >
      <g color="">
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M1.69841 1.69841C1.96296 1.43386 2.39188 1.43386 2.65643 1.69841L8.5 7.54198L14.3436 1.69841C14.6081 1.43386 15.037 1.43386 15.3016 1.69841C15.5661 1.96296 15.5661 2.39188 15.3016 2.65643L9.45801 8.5L15.3016 14.3436C15.5661 14.6081 15.5661 15.037 15.3016 15.3016C15.037 15.5661 14.6081 15.5661 14.3436 15.3016L8.5 9.45801L2.65643 15.3016C2.39188 15.5661 1.96296 15.5661 1.69841 15.3016C1.43386 15.037 1.43386 14.6081 1.69841 14.3436L7.54198 8.5L1.69841 2.65643C1.43386 2.39188 1.43386 1.96296 1.69841 1.69841Z"
          fill="#FF9999"
          stroke="#FF9999"
          stroke-width="1.4"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
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
        return '20'
      },
    },
  },
}
</script>


