<template>
    <div class="metric">
<h1>{{assets[0].name}} : {{assets[0].priceUsd}}</h1>
    </div>
</template>

<script>

    import axios from 'axios'

    export default {
        name: "Metric",
        data() {
            return {
                assets: [{
                    name:'',
                    priceUsd:'',
                }],
            }
        },
        async created() {
            const respons =await axios.get('https://api.coincap.io/v2/assets?limit=1');
             if (respons.data && respons.status === 200) {
                 this.setAssets(respons.data.data);
                 this.getPrices();
                 // console.log('respons - ', respons.data.data);
                } else {
                    throw new Error('Сервер не доступен')
                }
        },
        methods: {
            setAssets(data) {
                this.assets = data;
            },
            getPrices() {
                const ws = new WebSocket(`wss://ws.coincap.io/prices?assets=bitcoin`);
                ws.onmessage = (msg) => {
                    this.updatePrices(msg.data);
                }
            },
            updatePrices(data) {
                const parsedPrices = JSON.parse(data);
                for (let item of this.assets) {
                    for (let key in parsedPrices) {
                        item.priceUsd = parsedPrices[key];
                    }
                }
            }
        },
    }
</script>

<style scoped>

</style>