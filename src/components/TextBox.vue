<template>
    <input ref="textbox" type="text" :placeholder="placeholder">
</template>

<script>
export default {
    name: 'TextBox',
    props: [
        'placeholder'
    ],
    mounted() {
        const textbox = this.$refs.textbox;

        textbox.addEventListener('keyup', (event) => {
            if (event.key == 'Enter') {
                this.$emit('submitted', textbox.value);
            }
        });
    }
}
</script>

<style scoped>
    input {
        background-color: rgb(40, 40, 40);
        border: none;
        border-radius: 3px;
        width: 50%;
        height: 35px;
        text-align: center;
        color: rgb(255, 255, 255);
        font-size: 17px;
        font-family: 'Ubuntu', sans-serif;
    }
</style>