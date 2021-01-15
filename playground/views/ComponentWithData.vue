<template>
  <div>
    <p>Here is the data: {{ fromApi }}</p>
    other {{ other }}
    <button @click="$router.push({ hash: '#' + Date.now() })">
      new navigation
    </button>
  </div>
</template>

<script>
import { defineComponent, toRefs, reactive } from 'vue'
import { getData, delay } from '../api'
import { onBeforeRouteUpdate } from '../../src'

export default defineComponent({
  name: 'ComponentWithData',
  async setup() {
    const data = reactive({ other: 'old', fromApi: null })

    onBeforeRouteUpdate(async (to, from, next) => {
      data.fromApi = await getData()
      next()
    })

    // onBeforeRouteResolve(async (to, from) => {
    //   console.log('waiting 1s')
    //   await delay(200)
    //   console.log('done waiting')

    //   // can throw
    //   data.fromApi = await getData()
    // })

    console.log('waiting 1s')
    await delay(200)
    console.log('done waiting')

    data.fromApi = await getData()

    return {
      ...toRefs(data),
    }
  },
  // async beforeRouteEnter(to, from, next) {
  //   console.log('this in beforeRouteEnter', this)
  //   await delay(300)
  //   next(vm => {
  //     console.log('got vm', vm)
  //     vm.other = 'Hola'
  //   })
  // },
})
</script>
