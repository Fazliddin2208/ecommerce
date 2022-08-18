<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Rahmat</h1>

                <p>Sizning buyurtmangiz 48 soat ichida yetkazib beriladi!</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = 'Success | CompBuy'
    },
}
</script>