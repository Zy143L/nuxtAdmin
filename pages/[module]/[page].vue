<!--
 * @Author: NMTuan
 * @Email: NMTuan@qq.com
 * @Date: 2024-02-29 09:18:04
 * @LastEditTime: 2024-03-23 15:19:12
 * @LastEditors: NMTuan
 * @Description: 
 * @FilePath: \nuxtAdmin\pages\[module]\[page].vue
-->

<template>
    <!-- <ModuleComponent /> -->
    <PageComponent />
</template>

<script setup>
const route = useRoute()
const routeStore = useRouteStore()
const { token } = useAuth()

const baseURL = inject('baseURL')
const { module, page } = route.params
const pageInfo = computed(() => {
    return routeStore.routes.find(route => route.route === `${module}__${page}`) || {}
})
provide('pageInfo', pageInfo)

const url = computed(() => {
    let url
    if (pageInfo.value.fetchUrl) {
        url = pageInfo.value.fetchUrl
    } else {
        url = pageInfo.value.path
    }
    return `${baseURL}${url}`
})

// 获取数据
const pageFetch = (query = {}, watch = []) => {
    return useLazyFetch(url.value, {
        method: pageInfo.value.fetchType || 'GET',
        headers: {
            Authorization: token.value
        },
        query,
        watch
    })
}
provide('pageFetch', pageFetch)

const pageActions = computed(() => {
    return routeStore.routes.filter(route => route.route.startsWith(`${module}__${page}__`) && route.__type === 'action')
})
provide('pageActions', pageActions)

const pageChildren = computed(() => {
    return routeStore.routes.filter(
        route => route.route.startsWith(`${module}__${page}__`) && route.__type !== 'action'
            && route.route.split('__').length === 3
    )
})
provide('pageChildren', pageChildren)
</script>
