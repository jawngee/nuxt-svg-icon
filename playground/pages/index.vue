<template>
  <div>
    Nuxt module playground!
    <Home width="32px" fill="#F00" />
    <svg-icon-home width="32px" fill="green" />
    sweet
    <svg-icon-home width="32px" />
  </div>
  <div>
    url encoded svg:
    <img :src="HomeUri" width="32px" />
  </div>
  <div>
    with clashing ids (left visible, right invisible):
    <br />
    <svg-icon-homeid width="32px" />
    <svg-icon-sub-homeid width="32px" />
  </div>
</template>

<script setup>
  import Home from '~/assets/home.svg'
  import HomeUri from '~/assets/home-ns.svg?url_encode'
</script>
