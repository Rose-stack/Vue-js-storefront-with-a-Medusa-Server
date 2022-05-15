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
export default {
  name: 'DefaultLayout'
}
</script>