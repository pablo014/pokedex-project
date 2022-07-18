<script setup lang="ts">
import { defineProps, withDefaults, computed } from "vue"
import _ from "lodash"

interface Props {
  name: String,
  id: Number
}

const props = withDefaults(defineProps<Props>(), {
  name: '',
  id: 0,
})

const capName = _.capitalize(props.name)


</script>
<template>
  <div v-if="id" class="flex items-center border rounded pl-4">
    <slot name="image"></slot>
    <div class="w-full flex">
      <slot id="id">
        <div class="pr-2">
          {{ id }}
        </div>
      </slot>
      <slot name="name">
        {{ capName }}
      </slot>
    </div>
  </div>
</template>