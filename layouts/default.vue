<template>
  <div class="bgCover">
    <slot />
  </div>
</template>

<style lang="scss" scoped>
.bgCover {
  @apply h-screen w-screen bg-cover bg-center bg-no-repeat;
  background-image: url("@/assets/images/hero.webp");
}

.bgCover::before {
  @apply absolute inset-0 bg-black opacity-50;
}

.bgCover::after {
  @apply absolute inset-0 bg-gradient-to-t from-black to-transparent;
}
</style>
