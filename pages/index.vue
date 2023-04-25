<template>
  <div class="bg-black">
    <Container>
      <div class="flex justify-center items-center h-screen">
        <div class="flex flex-col gap-2">
          <img
            src="@/assets/images/swelogo.png"
            alt="Sex With Ex Logo"
            loading="lazy"
          />
          <nuxt-link
            class="bg-red-700 hover:bg-red-600 px-3 py-2 rounded text-white text-center"
            to="battleboard"
          >
          Battleboard
          </nuxt-link>
        </div>
      </div>
    </Container>
  </div>
</template>

<script>
export default {
  setup() {
    return {};
  },
};
</script>

<style lang="scss" scoped>
</style>