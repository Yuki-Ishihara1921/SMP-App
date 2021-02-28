<template>
    <button
        class="btn btn-secondary btn-sm rounded-circle ml-auto my-auto"
        @click="isModalOpen = true"
    >
        ？
    </button>
    <div v-if="isModalOpen" class="modal__container">
        <div class="m-3">
            <div class="modal__content">
                <section>
                    <h5 class="text-info font-weight-bold">
                        2~4ヶ所の中間地点を表示
                    </h5>
                    <div class="modal__content--image">
                        <img class="w-100" src="../assets/exampleMap.png" alt="サンプル画像">
                    </div>
                    <p class="my-2">
                        <span class="text-danger h6">
                            <img src="../assets/redMarker.png" alt="赤マーカー">
                            中間地点
                        </span>
                        <span class="text-primary h6">
                            <img src="../assets/blueMarker.png" alt="青マーカー">
                            検索地点
                        </span>
                    </p>
                    <p class="py-3 h6 border-top">
                        ※ 実際の中間距離ではなく、地図上での中間地点を表示します。
                    </p>
                </section>
                <button
                    class="btn btn-primary btn-sm w-25"
                    @click="isModalOpen = false"
                >
                    OK
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            isModalOpen: false
        }
    }
}
</script>

<style scoped>
.modal__container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    text-align: center;
}

.modal__content {
    max-width: 500px;
    margin: auto;
    padding: 1rem;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 0.3rem;
    background-color: #fff;
    background-clip: padding-box;
}

.modal__content--image {
    max-width: 300px;
    margin: 1rem auto;
}
</style>