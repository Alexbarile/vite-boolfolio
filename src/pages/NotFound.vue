<script>
export default {
    name: 'NotFound'
}
</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col text-center">
                <h2>Pagina non trovata</h2>
                <router-link :to="{name: 'post_list'}" class="btn btn-primary">
                    Torna ai Post
                </router-link>
            </div>
        </div>
    </div>
</template>
<style lang="scss">
    
</style>