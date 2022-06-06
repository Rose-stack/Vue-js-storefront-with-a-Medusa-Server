<template>
  <div>
    <app-navbar />
    <main>
      <div class="container">
        <Nuxt />
      </div>
    </main>
    <app-footer />
  </div>
</template>

<script>
import "@storefront-ui/vue/styles.scss"; // vuestorefront UI styles.
export default {
  name: "DefaultLayout",
};
</script>
