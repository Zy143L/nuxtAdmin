<!--
 * @Author: NMTuan
 * @Email: NMTuan@qq.com
 * @Date: 2024-03-11 14:39:36
 * @LastEditTime: 2024-03-12 08:36:12
 * @LastEditors: NMTuan
 * @Description: 
 * @FilePath: \nuxtAdmin\components\com\form\select.vue
-->
<template>
    <USelectMenu v-model="formData[field.key]" :options="field.options" value-attribute="value"
        :multiple="field.multiple" :disabled="field.disabled" :placeholder="field.placeholder" />
</template>
<script setup>
const field = inject('field')
const formData = inject('formData')
</script>
