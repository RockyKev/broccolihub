<template>
  <!-- <div class="container">
    <div>

      <h1 class="title">
        broccolihub
      </h1>
      <h2 class="subtitle">
        My excellent Nuxt.js project
      </h2>
      <div class="links">
        <a href="https://nuxtjs.org/" target="_blank" class="button--green">
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
    </div>
  </div> -->
  <div>
    <Nav />
    <Header />
    <Timer />
    <Section />
    <Footer />
  </div>
</template>

<script>
import Header from '~/components/Header.vue'
import Nav from '~/components/Nav.vue'
import Timer from '~/components/Timer.vue'
import Section from '~/components/Section.vue'
import Footer from '~/components/Footer.vue'
export default {
  components: {
    Header,
    Nav,
    Timer,
    Section,
    Footer
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
</style>
