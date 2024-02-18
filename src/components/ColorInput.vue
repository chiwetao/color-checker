<template>
    <div class="color-input">
        <label>{{ label }}</label>
        <input type="color" v-bind:value="color" v-on:input="updateColor" />
    </div>
</template>

<script setup>
const props = defineProps({
    label: String,
    color: String,
});

const emits = defineEmits(['update:color']);

const updateColor = (event) => {
    emits('update:color', event.target.value);
};
</script>

<style scoped>
input {
  width: 100%;
  height: 150px;
}
.color-input {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: left;
}

label {
  margin-bottom: 5px;
  align-self: flex-start;
}

</style>
