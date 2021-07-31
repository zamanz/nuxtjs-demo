<template>
    <span class="text-danger">
        <small v-for="(error,index) in errors" :key="index" class="form-text text-danger">{{ error[field] }}</small>
    </span>
</template>
<script>
export default {
    props:{
        errors:{
            default: null,
            type: Array
        },
        field:{
            default: null,
            type: String
        }
    },
}
</script>