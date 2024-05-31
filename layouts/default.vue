<template>
  <div class="min-h-screen flex flex-col bg-slate-900">
    <Header class="sticky top-0" />

    <main class="flex-grow">
      <slot />
    </main>

    <Footer />
  </div>
</template>