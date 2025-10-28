<template>
  <div class="max-w-lg mx-auto py-20">
    <div class="wrapper flex items-center gap-2">
      <div class="rounded-full avatar-wrapper">
        <img class="avatar rounded-full" :src="props.image" alt="Student avatar">
      </div>
      <div>
        <label class="label" for="hours-old">{{ props.label }} is</label>
        <div class="flex items-center">
          <input
            class="input"
            v-model="props.value"
            name="hours-old"
            id="hours-old"
            type="number"
          />
          <div class="caption">{{props.caption}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = {
  label: 'Samuel',
  value: null,
  image: '/student.jpg',
  caption: 'hours old'
  // placeholder optional
}
</script>

<style scoped>
.avatar-wrapper {
  border: 1px solid transparent;
  margin-right: 18px;
  transition: border-color 0.15s ease-in-out;
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
  letter-spacing: 0%;
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
  padding: 11px 16.5px 11px 8px;
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
  margin-right: 12px;
  caret-color: #906FEE;
  outline: none;
}


</style>
