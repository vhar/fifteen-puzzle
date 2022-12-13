<template lang="">
    <div class="button">
        <div>
            <slot></slot>
        </div>
    </div>
</template>
<script>
export default {
    name: 'kh-button',
}
</script>
<style scoped>
.button {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-content: center;
    background: linear-gradient(to bottom left, rgb(255,195,55), rgb(255,128,50));
    width: 100%;
    height: auto;
    border-radius: 6px;
    cursor: pointer;
    padding: 6px;
}
.button div {
    height: 100%;
    width: 100%;
    text-align: center;
    background: linear-gradient(to top right, rgb(255,195,55), rgb(255,128,50));
    border-radius: 12px;
    color: #b34c01;
}
</style>