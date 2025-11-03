<template>
  <div class="wrapper flex items-center gap-2">
      <div class="rounded-full avatar-wrapper">
        <img class="avatar rounded-full" :src="imageUrl" :alt="imageAlt">
      </div>
      <div>
        <label class="label" for="numeric-input">{{ label }}</label>
        <div class="flex items-center">
          <div
            class="input-wrapper"
            :data-value="inputValue && inputValue.length >= 3 ? inputValue : ''"
          >
            <input
              class="input"
              :value="inputValue"
              name="numeric-input"
              id="numeric-input"
              type="text"
              inputmode="numeric"
              ref="inputEl"
              @input="onInput"
              :placeholder="placeholder"
            />
          </div>
          <div class="caption">{{caption}}</div>
        </div>
      </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
interface Props {
  label: string
  imageUrl: string
  imageAlt?: string 
  caption: string
  placeholder?: string
}

const { imageAlt = 'Avatar'} = defineProps<Props>()

const inputValue = defineModel<string>();
const inputEl = ref<HTMLInputElement | null>(null)

function formatSpaces(digits: string) {
  return digits.replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
}

function findCaretPosByDigitIndex(formatted: string, digitIndex: number) {
  if (digitIndex <= 0) return 0
  let count = 0
  for (let i = 0; i < formatted.length; i++) {
    if (/\d/.test(formatted[i])) {
      count++
      if (count === digitIndex) {
        return i + 1
      }
    }
  }
  return formatted.length
}

function onInput(e: Event) {
  const el = e.target as HTMLInputElement
  const prevValue = el.value
  const prevCaret = el.selectionStart ?? prevValue.length
  const digitsBeforeCaret = prevValue.slice(0, prevCaret).replace(/\D/g, '').length

  const onlyDigits = prevValue.replace(/\D/g, '')
  const formatted = formatSpaces(onlyDigits)

  inputValue.value = formatted

  requestAnimationFrame(() => {
    if (!inputEl.value) return
    inputEl.value.value = formatted
    const newCaret = findCaretPosByDigitIndex(formatted, digitsBeforeCaret)
    inputEl.value.setSelectionRange(newCaret, newCaret)
  })
}

onMounted(() => {
  const initial = inputValue.value == null ? '' : String(inputValue.value)
  const digits = initial.replace(/\D/g, '')
  inputValue.value = formatSpaces(digits)
})
</script>

<style scoped>
.avatar-wrapper {
  border: 1px solid transparent;
  margin-right: 18px;
  transition: border-color 0.15s ease-in-out;
  flex-shrink: 0;
}

.avatar {
  width: 80px;
  height: 80px;
  object-fit: cover;
  object-position: center;
  box-sizing: content-box;
  border: 3px solid transparent;
}

.label {
  display: block;
  font-family: Koulen;
  font-weight: 400;
  font-size: 16px;
  line-height: 15px;
  letter-spacing: .02em;
  color: #1E0E4C;
  margin-bottom: 12px;
  transition: color 0.15s ease-in-out;
}

.caption {
  font-family: Inter;
  font-weight: 400;
  font-size: 18px;
  line-height: 100%;
  color: #1E0E4C;
}

.wrapper:focus-within {
  .label {
    color: #3D06D7;
  }

  .input {
    border: 1.5px solid #906FEE;
    color: #1E0E4C;
  }

  .avatar-wrapper {
    border-color: #3D06D7;
  }
}

.input {
  display: block;
  width: 72px;
  height: 44px;
  padding: 6.5px 15px 11px 8px;
  font-family: Inter;
  font-weight: 500;
  font-size: 18px;
  line-height: 100%;
  background-color: transparent;
  background-clip: padding-box;
  border: 1px solid #CFCADF;
  color: #CFCADF;
  border-radius: 6px;
  transition: border-color 0.15s ease-in-out;
  margin-right: 0;
  caret-color: #906FEE;
  outline: none;
}

.input-wrapper {
  display: inline-grid;
  width: min-content;
  min-width: 72px;
  margin-right: 12px;
  flex: 0 0 auto;
}

.input-wrapper::after {
  content: attr(data-value) ' ';
  visibility: hidden;
  white-space: pre;
  grid-area: 1 / 1;
  font-family: Inter;
  font-weight: 500;
  font-size: 18px;
  letter-spacing: 0;
  padding: 6.5px 15px 11px 8px;
  border: 1px solid transparent;
  box-sizing: border-box;
}

.input-wrapper > .input {
  grid-area: 1 / 1;
  width: 100%;
  min-width: 0;
}

</style>
