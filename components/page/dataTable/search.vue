<!--
 * @Author: NMTuan
 * @Email: NMTuan@qq.com
 * @Date: 2024-03-11 16:38:23
 * @LastEditTime: 2024-03-18 14:33:22
 * @LastEditors: NMTuan
 * @Description: 
 * @FilePath: \nuxtAdmin\components\page\dataTable\search.vue
-->
<template>
    <div class="flex">
        <!-- 搜索 -->
        <ComForm :fields="fields" v-model="q" horizontal :submit="submit"></ComForm>
        <!-- 按钮 -->
        <UButtonGroup orientation="horizontal">
            <UButton @click="submit" icon="i-ri-search-2-line">{{ $t('page.dataTable.search') }}</UButton>
            <UDropdown :items="items" :popper="{ placement: 'bottom-end', arrow: true }" :ui="{ width: 'w-auto' }">
                <UButton icon="i-heroicons-chevron-down-20-solid" class="border-l dark:border-gray-800" />
            </UDropdown>
        </UButtonGroup>
        <!-- 高级搜索 -->
        <USlideover :model-value="isOpen" @close="handlerClose">
            <ComCard :label="$t('page.dataTable.advSearch')" :close="handlerClose">
                <ComForm :fields="advFields" v-model="q" :submit="submit"></ComForm>

                <template #footer>
                    <div class="flex justify-between">
                        <UButton variant="ghost" color="gray" class="mr-4" @click="reset">
                            {{ $t('page.dataTable.reset') }}
                        </UButton>
                        <div class="flex items-center">
                            <UButton variant="ghost" color="gray" class="mr-4" @click="handlerClose">
                                {{ $t('page.dataTable.cancel') }}
                            </UButton>
                            <UButton @click="submit">
                                {{ $t('page.dataTable.search') }}
                            </UButton>
                        </div>
                    </div>
                </template>
            </ComCard>
        </USlideover>
    </div>
</template>
<script setup>
const { t } = useI18n()
const props = defineProps({
    modelValue: {
        type: Object,
        default: () => { }
    },
    fields: {
        type: Array,
        default: () => []
    },
    advFields: {
        type: Array,
        default: () => []
    }
})
const emits = defineEmits(['update:modelValue'])

const q = ref({})   // 查询项
const oriQ = JSON.stringify(props.modelValue)  // 原始表单值
const isOpen = ref(false)

onMounted(() => {
    console.log('onMounted')
})

const submit = () => {
    handlerClose()
    const newQ = JSON.parse(JSON.stringify(props.modelValue))
    props.fields.forEach(item => {
        // 注意，这里===空时，也需要刷新
        if (q.value[item.key] !== undefined) {
            newQ[item.key] = q.value[item.key]
        }
    })
    props.advFields.forEach(item => {
        // 注意，这里===空时，也需要刷新
        if (q.value[item.key] !== undefined) {
            newQ[item.key] = q.value[item.key]
        }
    })
    emits('update:modelValue', JSON.parse(JSON.stringify(newQ)))
}

const reset = () => {
    console.log('reset', q.value)
    handlerClose()
    q.value = {}
    emits('update:modelValue', JSON.parse(oriQ))
}

const handlerClose = () => {
    isOpen.value = false
}
const items = computed(() => {
    let items = []
    if (Array.isArray(props.advFields) && props.advFields.length > 0) {
        items.push(
            { label: t('page.dataTable.advSearch'), icon: 'i-ri-search-eye-line', click: () => isOpen.value = true },
        )
    }

    items.push(
        {
            label: t('page.dataTable.resetSearch'),
            icon: 'i-ri-refresh-line',
            click: () => {
                reset()
            }
        }

    )
    return [items]
})

// 监听组件外的变动. 更新到组件内
watchEffect(() => {
    q.value = JSON.parse(JSON.stringify(props.modelValue))
})


</script>
