<template>
    <div class="appLoading_container">
        <div class="spinner-border" role="status">
            <span class="sr-only">Loading...</span>
        </div>
        <p class="py-2 text-info">{{text}}</p>
    </div>
</template>

<script>
export default {
    data () {
        return {
            text: "検索中..."
        }
    }
}
</script>

<style scoped>
.appLoading_container {
    display: flex;
    flex-flow: column;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.8);
    z-index: 9999;
}
</style>