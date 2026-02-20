<script setup lang="ts">
import { ref } from 'vue'
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'

const logs = ref<string[]>([])
const inputFocused = ref(false)

function activeElementLabel() {
  const active = document.activeElement
  if (!(active instanceof HTMLElement))
    return 'none'

  const id = active.id ? `#${active.id}` : ''
  return `${active.tagName.toLowerCase()}${id}`
}

function log(message: string) {
  logs.value = [`${new Date().toISOString()} ${message}`, ...logs.value].slice(0, 18)
}

function onInputFocus() {
  inputFocused.value = true
  log(`input focus (active=${activeElementLabel()})`)
}

function onInputBlur() {
  inputFocused.value = false
  log(`input blur (active=${activeElementLabel()})`)
}

function onOutsideClick() {
  log(`outside click (active=${activeElementLabel()})`)
}
</script>

<template>
  <main class="page">
    <h1>Headless UI Vue Popover Focus Restore Repro</h1>
    <ol class="steps">
      <li>Click the input to open the popover.</li>
      <li>Click anywhere outside the popover (the gray box is just an obvious target).</li>
      <li>Notice focus returns to the input.</li>
    </ol>

    <Popover v-slot="{ open }" as="div" class="popover-root">
      <PopoverButton as="label" class="trigger">
        <span>Date input trigger</span>
        <input
          id="repro-input"
          type="text"
          placeholder="Type here"
          @focus="onInputFocus"
          @blur="onInputBlur"
        >
      </PopoverButton>

      <PopoverPanel class="panel">
        <p>Popover content (stay open while typing in input).</p>
        <button type="button" @click="log(`inside panel click (open=${open})`)">
          Inside action
        </button>
      </PopoverPanel>
    </Popover>

    <div class="outside" @click="onOutsideClick">
      Suggested outside-click target
    </div>

    <p class="status">
      Active element is input: <strong>{{ inputFocused ? 'yes' : 'no' }}</strong>
    </p>

    <h2>Event log</h2>
    <ul class="logs">
      <li v-for="entry in logs" :key="entry">
        {{ entry }}
      </li>
    </ul>
  </main>
</template>
