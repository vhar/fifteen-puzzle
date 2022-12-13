<template>
    <div class="knuckle">
        <div class="knuckle-num">
            <slot></slot>
        </div>
    </div>
</template>
<script>
export default {
    name: 'kh-knuckle'
}
</script>
<style scoped>
@font-face {
    font-family: 'Oswald';
    src: url('~@/fonts/Oswald-Bold.woff2') format('woff2');
    font-weight: bold;
    font-style: normal;
    font-display: swap;
}

.knuckle {
    position: relative;
    width: 76px;
    height: 76px;
    border-radius: 6px;
    background: linear-gradient(to bottom left, rgb(255,195,55), rgb(255,128,50));
    cursor: pointer;
    user-select: none;
}
.knuckle-num {
    position: absolute;
    top: 6px;
    left: 6px;
    width: 64px;
    height: 64px;
    border-radius: 50%;
    background: linear-gradient(to top right, rgb(255,195,55), rgb(255,128,50));
    text-align: center;
    font: bold 48px/64px Oswald;
    color: #b34c01;
    text-shadow: 2px 2px 2px rgba(255,255,255,0.3);
}
.knuckle.blank,
.knuckle.blank .knuckle-num {
    background: none;
    cursor: default;
}
</style>