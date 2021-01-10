<template>
  <div>
    <!-- <router-link
      :to="{ name: 'inc', params: { increment: Number(increment) + 1 } }"
      >Increment at root</router-link
    > -->
    <child-component />
    <child-component />
    <div style="height: 200px"></div>
  </div>
</template>

<script>
import { provide, toRef, toRefs, watchEffect } from 'vue'
import ChildComponent from './ChildComponent.vue'
import { useRoute, useRouter } from '../../src'

export default {
  components: { ChildComponent },
  name: 'ProvideComponent',
  props: ['increment'],
  setup(props) {
    const increment = toRef(props, 'increment')
    const route = useRoute()
    const router = useRouter()

    provide('increment', increment)

    watchEffect(() => {
      console.log('prop', increment.value)
    })

    watchEffect(() => {
      console.log(
        'router.currentRoute',
        router.currentRoute.value.params.increment
      )
    })

    watchEffect(() => {
      console.log('route.params', route.params.increment)
    })

    return { increment }
  },
}
</script>
