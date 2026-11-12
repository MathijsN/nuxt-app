<template>
    <header>
        <nav>
            <ul>
                <li>
                    <NuxtLink to="/">Home <span aria-hidden="true">Home</span></NuxtLink>
                </li>
                <li>
                    <NuxtLink to="https://mathijs-portfolio.netlify.app/">Portfolio <span
                            aria-hidden="true">Portfolio</span></NuxtLink>
                </li>
            </ul>
        </nav>
    </header>
    <main>
        <slot />
    </main>
    <footer>
    </footer>
</template>
<style scoped>
nav {
    display: flex;
    justify-content: end;
    padding: var(--spacing-s);

    ul {
        gap: var(--spacing-m);
        display: flex;

        li {
            list-style: none;
            overflow-y: hidden;
            height: 2.2rem;

            &:hover {
                a {
                    transform: translateY(-1.6rem);
                    transition: all var(--animation-duration) ease;
                    background-color: var(--color-secondary);
                    color: var(--color-text-secondary);
                }
            }

            a {
                text-transform: lowercase;
                display: flex;
                gap: var(--spacing-s);
                flex-direction: column;
                padding: var(--spacing-s);
                transition: all var(--animation-duration) ease;

                span {
                    /* transform: translateY(0.5rem); */
                }
            }
        }
    }
}
</style>