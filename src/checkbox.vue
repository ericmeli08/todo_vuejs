<template>
    <input type="checkbox" v-model="model"  :id="id" class="hidden" />
          <label :for="id" class="flex items-center cursor-pointer">
              <div class="w-8 h-8 border-2  border-gray-300 rounded grid place-items-center mr-5 "
                :class="{ 'bg-blue-500': model }">
                <span v-if="model" class="text-white font-bold text-lg w-fit h-fit m-0">✓</span>
              </div>
            <span v-if="label" class="text-xl font-bold"> {{ label }}</span>
          </label>
</template>

<script setup>
const model = defineModel()
const props = defineProps({
    label : String,
    id : {
        type: String,
        default: "checkbox"
    }
})
</script>