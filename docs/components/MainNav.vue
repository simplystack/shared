<template>
  <header class="sticky z-10 top-0 left-0 w-full flex items-center bg-primary shadow h-16">
    <div class="container">
      <div class="flex items-center justify-between">
        <div class="inline-flex items-center lg:w-1/5">
          <NuxtLink to="/" class="inline-flex items-center">
            <Logo width="24" />
            <span class="text-lg font-semibold ml-2">@simplystack/ui</span>
          </NuxtLink>
        </div>
        <ToggleMode @change="handleThemeModeChange" />
      </div>
    </div>
  </header>
</template>

<script>

export default {
  methods: {
    handleThemeModeChange (mode) {
      const document = window.document.documentElement

      document.setAttribute('data-mode', mode)
    }
  }
}
</script>
