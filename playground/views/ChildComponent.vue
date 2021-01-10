<template>
  <div>{{ injected }}</div>
  <router-link :to="newLocation">Increment {{ injected }}</router-link>
</template>

<script>
import { inject, watch, computed } from 'vue'
export default {
  name: 'ChildComponent',
  setup() {
    const injected = inject('increment')

    watch(injected, () => {
      console.log('route param changed', injected.value)
    })

    const newLocation = computed(() => ({
      name: 'inc',
      params: { increment: Number(injected.value) + 1 },
    }))

    return {
      injected,
      newLocation,
    }
  },
}
</script>
