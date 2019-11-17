<template>
    <div class="metric">
        <h1>{{nameCoin}}: {{ metricRender }}</h1>
    </div>
</template>

<script>

    import axios from 'axios'

    export default {
        name: "Metric",
        data() {
            return {
                metricRender: '',
                nameCoin:'',
                arr:[],
            }
        },
        methods: {
            async getAxios() {
                const respons = await axios.get('https://api.coincap.io/v2/assets');
                if (respons.data && respons.status === 200) {
                    console.log('respons - ', respons.data);
                    this.metricRender = respons.data.data[0].priceUsd;
                    this.nameCoin = respons.data.data[0].name;

                } else {
                    throw new Error('Сервер не доступен')
                }
            },
            intervalAxios: function () {
                setInterval(() => {
                    this.getAxios();
                }, 1000);
            },
        },
        computed: {

        },
        mounted() {
            this.getAxios();
            //Запустили intervalAxios
            this.intervalAxios();
        },
    }
</script>

<style scoped>

</style>