<template>
  <div class="pt-24">
    <div
      class="container px-3 mx-auto flex flex-wrap flex-col md:flex-row items-center"
    >
      <!--Left Col-->
      <div
        class="flex flex-col w-full md:w-2/5 justify-center items-start text-center md:text-left"
      >
        <!-- <p class="uppercase tracking-loose w-full">What business are you?</p> -->
        <h1 class="my-4 text-5xl font-bold leading-tight">
          broccoli Hub
        </h1>
        <p class="leading-normal text-2xl mb-8">
          Where people who like to pretend they are giants and tiny tree
          enthusiasts can unite
        </p>

        <!-- <button
          class="mx-auto lg:mx-0 hover:underline bg-white text-gray-800 font-bold rounded-full my-6 py-4 px-8 shadow-lg"
        >
          Subscribe
        </button> -->
      </div>
      <!--Right Col-->
      <div class="w-full md:w-3/5 py-6 text-center flex justify-center">
        <!-- <img class="w-full md:w-4/5 z-50" src="hero.png" /> -->
        <Logo />
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>

<style scoped>
.container {
  @apply leading-normal tracking-normal text-green-700;
}
</style>
