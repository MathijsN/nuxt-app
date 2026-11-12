<script setup>
const route = useRoute()
const { data } = await useFetch('/api/ticker/?id=' + route.params.id)
const { data: allData } = await useFetch('/api/tickers/?limit=15')

const coin = data.value[0]
</script>
<template>
    <dl>
        <dt>Rank</dt>
        <dd>{{ coin.rank }}</dd>
        <dt>Symbol</dt>
        <dd>{{ coin.symbol }}</dd>
        <dt>Name</dt>
        <dd>{{ coin.name }}</dd>
        <dt>Price (USD)</dt>
        <dd>{{ coin.price_usd }}</dd>
        <dt>% change in 24h</dt>
        <dd>{{ coin.percent_change_24h }}</dd>
    </dl>

    <ul class="crypto-list">
        <li v-for="currency in allData.data" :key="allData.data.id">
            <NuxtLink :to="'/currency/' + currency.id">
                <div>
                    <h2>{{ currency.symbol }}</h2>
                    <p class="sub-text">{{ currency.name }} - rank: {{ currency.rank }}</p>
                </div>
                <p class="price">{{ Number(currency.price_usd).toFixed(2) }}
                    <span class="price-change">
                        <svg v-if="currency.percent_change_24h >= 0" width="11" height="9" viewBox="0 0 11 9"
                            fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path d="M5.19531 0L10.3915 9L-0.000840187 9L5.19531 0Z" fill="#72D673" />
                        </svg>
                        <svg v-if="currency.percent_change_24h < 0" width="11" height="9" viewBox="0 0 11 9" fill="none"
                            xmlns="http://www.w3.org/2000/svg">
                            <path d="M5.19531 9L-0.00084002 2.51244e-08L10.3915 -8.834e-07L5.19531 9Z" fill="#F70000" />
                        </svg>
                        {{ currency.percent_change_24h }} %
                    </span>
                </p>
            </NuxtLink>
        </li>
    </ul>
</template>
<style scoped>
ul.crypto-list {
    display: flex;
    gap: var(--spacing-s);
    overflow-x: scroll;
    margin-block: var(--spacing-xl);

    li {
        background-color: var(--color-background-secondary);
        list-style: none;
        padding: var(--spacing-m);
        transition: all var(--animation-duration) ease;
        height: 200px;
        aspect-ratio: 1;
        scrollbar-width: none;

        &:hover {
            background-color: var(--color-secondary);
            transition: all var(--animation-duration) ease;

            h2,
            .price,
            .price-change {
                color: var(--color-text-secondary);
            }
        }

        a {
            width: 100%;
        }
    }
}
</style>