<template>
<button :style={background:color} class="btn" @click="onClick()">{{ label }}</button>
</template>

<script>
export default {
    name:'my-button',
    props:{
        label:{
            type:String,
            default:'Close'
        },
        color:{
            default:'#000'
        }
    },
    methods:{
        onClick(){
            this.$emit('btn-click');
        }
    },
}
</script>
<style scoped>

</style>