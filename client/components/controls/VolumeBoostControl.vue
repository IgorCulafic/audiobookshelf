<template>
  <div class="relative" v-click-outside="clickOutside">
    <button :aria-label="$strings.LabelVolumeBoost" :aria-expanded="showMenu" class="flex items-center text-gray-300 hover:text-white" @mousedown.prevent @mouseup.prevent @click.stop="showMenu = !showMenu">
      <span class="material-symbols text-2xl" :class="{ fill: value }">campaign</span>
      <span v-if="value" class="text-xs font-semibold pl-0.5">+{{ value }}</span>
    </button>
    <div v-show="showMenu" class="absolute bottom-full left-1/2 -translate-x-1/2 mb-3 z-20 w-36 bg-bg border-black-200 border shadow-xl rounded-lg overflow-hidden">
      <ul role="listbox" :aria-label="$strings.LabelVolumeBoost">
        <li v-for="boost in boosts" :key="boost" role="option" :aria-selected="boost === value" class="py-1 text-sm text-center cursor-pointer" :class="boost === value ? 'bg-black-100' : 'hover:bg-black/10'" @click="$emit('input', boost)">{{ boost ? `+${boost} dB` : $strings.LabelOff }}</li>
      </ul>
      <div class="flex items-center justify-between p-1 border-t border-black-200">
        <ui-icon-btn :aria-label="$strings.ButtonDecrease" :disabled="value <= 0" icon="remove" :size="7" @click="set(value - 1)" />
        <p class="text-sm" aria-live="polite">{{ value ? `+${value} dB` : $strings.LabelOff }}</p>
        <ui-icon-btn :aria-label="$strings.ButtonIncrease" :disabled="value >= maxBoost" icon="add" :size="7" @click="set(value + 1)" />
      </div>
      <p class="px-2 py-1 text-xxs text-center text-gray-300 border-t border-black-200">{{ $strings.MessageVolumeBoostSavedForBook }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      showMenu: false,
      maxBoost: 18
    }
  },
  computed: {
    boosts() {
      return [0, 3, 6, 9, 12, 15, 18]
    }
  },
  methods: {
    set(boost) {
      this.$emit('input', Math.min(Math.max(boost, 0), this.maxBoost))
    },
    clickOutside() {
      this.showMenu = false
    }
  }
}
</script>
