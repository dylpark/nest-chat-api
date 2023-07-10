<script setup lang="ts"></script>

<template>
  <header></header>
  <main>
    <div class="flex flex-col items-center justify-center h-screen w-screen gap-2 bg-gray-100">
      <div class="flex w-1/4 rounded-xl p-4 bg-gray-600 mr-10">
        <div class="text-white">It's over Anakin, <br />I have the high ground.</div>
      </div>
      <div class="flex w-1/4 rounded-xl p-4 bg-blue-500 ml-10">
        <div class="text-white">You underestimate my power!</div>
      </div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
