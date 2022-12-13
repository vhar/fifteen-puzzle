<template>
    <div class="timer">{{ time }}</div>
</template>
<script>
export default {
    name: 'kh-timer',
    props: ['time']
}
</script>
<style scoped>
@font-face {
    font-family: 'Oswald';
    src: url('~@/fonts/Oswald-ExtraLight.woff2') format('woff2');
    font-weight: 200;
    font-style: normal;
    font-display: swap;
}
.timer {
    font: 200 18px/24px Oswald;
    color: #ddd;
    text-shadow: 2px 2px 2px rgba(255,255,255,0.3);
}
</style>