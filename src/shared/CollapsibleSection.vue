<template>
    <div>
        <div class="header">
            <div v-if="open" @click="open = !open">&#x25B2; Collapse</div>
            <div v-if="!open" @click="open = !open">&#x25BC; Expand</div>
        </div>
        <slot v-if="open">
            <div>Default</div>
        </slot>
    </div>
</template>

<script>
export default {
  name: 'CollapsibleSection',
  data() {
    return {
      open: true,
    };
  },
};
</script>

<style scoped>
.header {
    background-color: #bbb;
    padding: 3px;
    cursor: pointer;
}
span {
    width: 100%;
}
</style>
