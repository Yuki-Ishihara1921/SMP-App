<template>
    <header class="appHeader fixed-top border-bottom bg-light">
        <div>
            <span class="text-success font-weight-bold">S</span>earch 
            <span class="text-success font-weight-bold">M</span>eeting 
            <span class="text-success font-weight-bold">P</span>lace
        </div>
        <app-explanation />
    </header>
</template>

<script>
import AppExplanation from './AppExplanation'

export default {
    components: {
        AppExplanation
    }
}
</script>

<style scoped>
.appHeader {
    display: flex;
    padding: 0.25rem 1.5rem;
    font-size: 25px;
}

@media screen and (max-width: 400px) {
    .appHeader {
        font-size: 20px;
    }
}
</style>