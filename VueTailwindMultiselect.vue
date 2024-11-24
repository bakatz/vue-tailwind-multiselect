<template>
  <div class="relative">
    <!-- Dropdown trigger -->
    <button
      type="button"
      :id="id"
      @click="toggleDropdown"
      data-dropdown-trigger
      class="block text-left w-full rounded-lg border-0 p-2.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-primary-600 sm:text-sm sm:leading-6"
      :disabled="disabled"
    >
      <span v-if="selectedCount <= 0" class="text-gray-400">{{ placeholder }}</span>
      <div v-else>
        <span v-for="selectedName in selectedNames" :key="selectedName" class="bg-primary-500 text-white text-xs font-medium me-2 px-2.5 py-0.5 rounded">{{ selectedName }}</span>
      </div>
      <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center pr-3">
        <!-- Chevron icon -->
        <svg class="h-5 w-5 text-gray-500 transition-transform" :class="{ '-rotate-180': isDropdownOpen }" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
          <path fill-rule="evenodd" d="M5.23 7.21a.75.75 0 011.06.02L10 11.168l3.71-3.938a.75.75 0 111.08 1.04l-4.25 4.5a.75.75 0 01-1.08 0l-4.25-4.5a.75.75 0 01.02-1.06z" clip-rule="evenodd" />
        </svg>
      </div>
    </button>

    <!-- Dropdown content -->
    <div v-show="isDropdownOpen" class="absolute z-10 w-full mt-1" v-on-click-outside="[closeDropdown, { ignore: ['[data-dropdown-trigger]'] }]">
      <div class="overflow-hidden rounded-lg border border-gray-300 bg-white shadow-lg">
        <!-- Action buttons -->
        <div class="border-b border-gray-300">
          <button @click.prevent="selectAll" :disabled="!isSelectAllEnabled" class="block w-full px-4 py-2 text-left text-sm hover:bg-gray-100 disabled:opacity-50 disabled:hover:bg-white">Select All</button>
          <button @click.prevent="clearSelection" :disabled="!isClearSelectionEnabled" class="block w-full px-4 py-2 text-left text-sm hover:bg-gray-100 disabled:opacity-50 disabled:hover:bg-white">Clear Selection</button>
        </div>

        <!-- Options list -->
        <div class="max-h-60 overflow-y-auto">
          <div v-for="option in options" :key="option.value" @click="toggleOption(option.value)" :class="['relative flex items-start px-4 py-2 hover:bg-gray-50 cursor-pointer', { 'bg-gray-50': isSelected(option) }]">
            <div class="flex h-6 items-center">
              <input type="checkbox" :id="`${id}-${option.value}`" :name="name" :value="option.value" :checked="isSelected(option)" class="h-4 w-4 rounded border-gray-300 text-primary-600 focus:ring-primary-500 cursor-pointer" :disabled="disabled" @click.stop />
            </div>
            <div class="ml-3 text-sm leading-6">
              <span class="font-medium text-gray-900">
                {{ option.name }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import { vOnClickOutside } from '@vueuse/components'

const model = defineModel({
  type: Array,
  default: () => []
})

const props = defineProps({
  options: {
    type: Array,
    required: true
  },
  id: {
    type: String,
    required: true
  },
  name: {
    type: String,
    required: true
  },
  required: {
    type: Boolean,
    required: true
  },
  disabled: {
    type: Boolean,
    required: false,
    default: false
  },
  placeholder: {
    type: String,
    required: true
  }
})

const isDropdownOpen = ref(false)

const optionValueToObjectMap = computed(() => {
  const optionMap = {}
  for (const option of props.options) {
    optionMap[option.value] = option
  }
  return optionMap
})

const selectedCount = computed(() => model.value.length)
const selectedNames = computed(() => model.value.map((optionValue) => optionValueToObjectMap.value[optionValue].name))
const isSelectAllEnabled = computed(() => selectedCount.value < props.options.length)
const isClearSelectionEnabled = computed(() => selectedCount.value > 0)

function closeDropdown() {
  isDropdownOpen.value = false
}

function toggleDropdown() {
  isDropdownOpen.value = !isDropdownOpen.value
}

function toggleOption(optionValue) {
  if (!model.value.includes(optionValue)) {
    model.value = [...model.value, optionValue]
  } else {
    model.value = model.value.filter(value => value !== optionValue)
  }
}

function selectAll() {
  model.value = [...props.options.map(option => option.value)]
}

function clearSelection() {
  model.value = []
}

const isSelected = (option) => model.value.includes(option.value)
</script>
