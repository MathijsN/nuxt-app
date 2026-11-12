<script setup>
const { data } = await useFetch('/api/tickers/?limit=15')
const isVisible = ref(false)

function balanceToggle() {
    document.getElementById('balance').classList.toggle('visible')
}

</script>
<template>
    <h1>Welcome, Mathijs</h1>
    <div class="balance-wrapper">
        <button @click="isVisible = !isVisible">
            <span>Toggle balance visibility</span>
            <svg v-if="isVisible" width="22" height="16" viewBox="0 0 22 16" fill="none"
                xmlns="http://www.w3.org/2000/svg">
                <path
                    d="M19.8703 6.962C20.3443 7.582 20.3443 8.419 19.8703 9.038C18.3773 10.987 14.7953 15 10.6133 15C6.43134 15 2.84934 10.987 1.35634 9.038C1.12539 8.74127 1 8.37601 1 8C1 7.62399 1.12539 7.25873 1.35634 6.962C2.84934 5.013 6.43134 1 10.6133 1C14.7953 1 18.3773 5.013 19.8703 6.962Z"
                    stroke-width="2" stroke-linecap="round" stroke-linejoin="round" stroke="#FAFCFF" />
                <path
                    d="M13.6133 8C13.6133 9.65685 12.2701 11 10.6133 11C8.95643 11 7.61328 9.65685 7.61328 8C7.61328 6.34315 8.95643 5 10.6133 5C12.2701 5 13.6133 6.34315 13.6133 8Z"
                    stroke-width="2" stroke-linecap="round" stroke-linejoin="round" stroke="#FAFCFF" />
            </svg>

            <svg v-else width="22" height="9" viewBox="0 0 22 9" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                    d="M1 1C1 1 1.68037 1.77757 2.99826 2.64705M2.99826 2.64705C4.15489 3.41013 5.80257 4.24401 7.91232 4.68518M2.99826 2.64705L3 2.645M2.99826 2.64705L1 5M7.91232 4.68518C8.85094 4.88146 9.88101 5 11 5C12.1095 5 13.1315 4.88346 14.0637 4.69017M7.91232 4.68518L7.914 4.68M7.91232 4.68518L7 7.5M14.0637 4.69017C18.5928 3.75103 21 1 21 1M14.0637 4.69017L14.063 4.688M14.0637 4.69017L15 7.5M21 5L19.004 2.648"
                    stroke-width="2" stroke-linecap="round" stroke-linejoin="round" stroke="#FAFCFF" />
            </svg>
        </button>
        <p id="balance" class="balance" :class="{ visible: isVisible }">
            <span class="background"></span>
            $13098,89
        </p>
        <button class="buy">buy</button>
    </div>
    <ul class="crypto-list">
        <li v-for="currency in data.data" :key="data.data.id">
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
.balance-wrapper {
    display: flex;
    gap: var(--spacing-m);
    padding-block: var(--spacing-s);
    width: min-content;
    align-items: center;
    position: relative;

    button {
        background: none;
        border: transparent;
        cursor: pointer;

        &.buy {
            background-color: var(--color-secondary);
            height: min-content;
            padding: var(--spacing-s) var(--spacing-m);
            border-radius: var(--spacing-m);
            font-size: 1rem;
        }

        span {
            border: 0;
            clip-path: inset(50%);
            height: 1px;
            margin: 0;
            overflow: hidden;
            position: absolute;
            white-space: nowrap;
            width: 1px;
        }

        svg {
            transition: all var(--animation-duration) ease;
        }
    }

    .background {
        position: absolute;
        width: 0%;
        height: 100%;
        background-color: var(--color-secondary);
        z-index: -1;
        transition: all var(--animation-duration) ease;
    }

    &:has(p.visible) {
        .background {
            width: 100%;
            transition: all var(--animation-duration) ease;
        }
    }
}

ul.crypto-list {
    display: grid;
    gap: var(--spacing-s);
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    margin-block: var(--spacing-xl);

    li {
        background-color: var(--color-background-secondary);
        list-style: none;
        padding: var(--spacing-m);
        transition: all var(--animation-duration) ease;
        height: 200px;

        &:hover {
            background-color: var(--color-secondary);
            transition: all var(--animation-duration) ease;

            h2,
            .price,
            .price-change {
                color: var(--color-text-secondary);
            }
        }
    }
}

.balance {
    filter: blur(8px);
    width: max-content;
    transition: all var(--animation-duration) ease;

    font-size: calc(1.5rem + var(--fluid) * 1rem);

    &.visible {
        filter: blur(0px);
        color: var(--color-text-secondary);
    }
}

a {
    text-wrap: balance;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;



    h2 {
        font-size: 1rem;
        color: var(--color-secondary);
        text-transform: uppercase;
        font-weight: 100;
    }

    .sub-text {
        font-size: 0.8rem;
        color: var(--color-text-accent);
    }

    .price {
        font-size: 3rem;

        .price-change {
            font-size: 1rem;
        }
    }
}
</style>