<script lang="ts" setup>
import { replaceVariables } from '@scalar/oas-utils/helpers'
import type { Server } from '@scalar/types/legacy'
import { computed } from 'vue'

import type { ServerVariableValues } from './types'

const props = defineProps<{
  server?: Server
  variables?: ServerVariableValues
}>()

/** Server url with HTML to highlight variables */
const formattedServerUrl = computed(() => {
  // Get the server URL
  const url = props.server?.url ?? ''

  // Remove HTML
  const urlWithoutHtml = url.replace(/(<([^>]+)>)/gi, '')

  // Replace variables with values (if available)
  return replaceVariables(urlWithoutHtml, (matchedVariable: string) => {
    const value = props.variables?.[matchedVariable] ?? ''

    return `<span class="base-url-variable">${
      value !== '' ? value : `{${matchedVariable}}`
    }</span>`
  })
})
</script>
<template>
  <span
    class="base-url"
    v-html="formattedServerUrl" />
</template>
<style scoped>
.base-url {
  color: var(--scalar-color-1);
  cursor: pointer;
  display: inline-block;
  font-size: var(--scalar-mini);
  min-width: 0;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.base-url :deep(.base-url-variable) {
  color: var(--scalar-color-1);
}
</style>
