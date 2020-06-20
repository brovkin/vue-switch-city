<template>
    <div>
        <div class="switch__time" v-if="checkbox">New-York: {{new_york}}</div>
        <div class="switch__time" v-else >Moscow: {{moscow}}</div>
        <label class="switch">
            <input class="switch__checkbox" type="checkbox" v-model="checkbox">
            <div class="switch__title">{{getCity}}</div>
        </label>
    </div>
</template>

<script>
    export default {
        name: 'SwitchCities',
        props: ['moscow', 'new_york'],
        data() {
            return {
                checkbox: false
            }
        },
        computed: {
            getCity() {
                return this.checkbox ? 'New York' :'Moscow';
            }
        },
        mounted() {

        }
    }
</script>

<style scoped>
    .switch {
        background-color: #2c3e50;
        box-shadow: 0 0 10px 0 rgba(0,0,0, 0.3);
        width: 200px;
        border-radius: 30px;
        padding: 10px;
        display: block;
        cursor: pointer;
        margin: 0 auto;
    }

    .switch__time {
        margin: 1rem 0;
    }

    .switch__checkbox {
        display: none;
    }

    .switch__title {
        padding: 10px;
        border-radius: 30px;
        font-weight: bold;
        color: #000;
        background-color: #ccc;
        width: 100px;
        height: auto;
        transition: .3s all ease-in-out;
        text-align: center;
    }

    .switch__checkbox:checked ~ .switch__title {
        color: #fff;
        background-color: #2196F3;
        transform: translateX(80px);
    }
</style>
