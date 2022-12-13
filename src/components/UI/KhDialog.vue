<template>
    <div class="dialog" v-if="show" @click="hideDialog">
        <div class="dialog-content">
            <slot></slot>
            <kh-button @click="hideDialog" >Ok</kh-button>
        </div>
    </div>
</template>
<script>
export default {
    name: 'kh-dialog',
    props: {
        show: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        hideDialog() {
            this.$emit('update:show', false);
        }
    }
}
</script>
<style scoped>
.dialog {
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, .5);
    position: fixed; 
    display: flex;
    z-index: 10;
    user-select: none;
    padding: 5px;
}
.dialog-content {
    margin: auto;
    background: linear-gradient(to top right, rgb(255,195,55), rgb(255,128,50));
    border-radius: 6px;
    width: 100%;
    color: #b34c01;
    padding: 12px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 12px;
}
</style>