<template>
    <div class="form-group col-md-3 col-6">
        <input
            class="form-control form-control-sm"
            type="search"
            autocomplete="off"
            :id="id"
            :placeholder="placeholder"
        />
    </div>
</template>

<script>
export default {
    props: {
        id: String,
        placeholder: String
    }
}
</script>

<style scoped>
input {
    font-size: 16px;
}

@media screen and (max-width: 400px) {
    input {
        font-size: 10px;
    }
}
</style>