<template>
  <div
    class="
      py-2
      rounded-md
      lg:max-h-24
      lg:h-20
      flex
      items-center
      w-full
    "
  >
    <div class="flex flex-row justify-between w-full">
      <div class="rounded-full my-auto">
        <img
          class="w-20 object-cover rounded-full"
          :src="user[6] || '/img/user_icon.png'"
          :alt="user[5]"
        />
      </div>
      <div class="flex flex-col w-full justify-center items-center">
        <div class="p-2 flex flex-row w-full text-sm text-gray-400 justify-between">
          <div class="p-px">
            {{ user[5] || "Unknown User" }} ({{ user[0] }})
          </div>
          <div class="p-px text-center">
            Level {{ user[1] }}
          </div>
        </div>
        <div class="p-2 w-full my-auto">
          <ItemProgressBar :min="user[3]" :max="user[4]" :progress="user[2]" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Array,
      default() {
        return [
          '504965310823006228',
          86,
          'Joshua | Rem-Chan',
          '/img/joshua.jpg',
        ]
      },
    },
  },
}
</script>