<script lang="ts">
    import { useFocus } from "svelte-navigator";

    const registerFocus = useFocus();

    let countries: Country[] = []; // Countries data
    let countriesNumber: number; // Number of countries

    let search: string = ""; // Search for filtering
    let filteredCountries: Country[] = []; // Filter applyed

    $: search, (filteredCountries = filterCountries(search));

    let contriesDisplayed: number = 24; // Number of countries displayed

    function filterCountries(s: string): Country[] {
        return countries.filter((c) => {
            return c.countryName.toLowerCase().includes(s.toLowerCase());
        });
    }

    function shuffle(array: any[]): any[] {
        let currentIndex = array.length,
            randomIndex;
        while (currentIndex != 0) {
            randomIndex = Math.floor(Math.random() * currentIndex);
            currentIndex--;
            [array[currentIndex], array[randomIndex]] = [
                array[randomIndex],
                array[currentIndex],
            ];
        }
        return array;
    }

    fetch("./countries.json")
        .then((r) => r.json())
        .then((d) => {
            countries = d as Country[];
            countriesNumber = d.length;
            filteredCountries = filterCountries("");
        });

    interface Country {
        countryCode: string;
        countryName: string;
        currencyCode: string;
        population: string;
        capital: string;
        continentName: string;
    }
</script>

<div class="quiz fc">
    <div class="input-and-info f">
        <input
            class="search"
            type="text"
            placeholder="Search"
            bind:value={search}
        />
        <span>{filteredCountries.length}/{countriesNumber}</span>
    </div>
    <div class="country-list fc">
        {#each filteredCountries as c, index}
            {#if index < contriesDisplayed}
                <div class="country fc">
                    <span>{c.countryName}</span>
                    <img
                        height="200"
                        src="./flags/{c.countryCode.toLowerCase()}.svg"
                        alt="flag of {c.countryName}"
                    />
                </div>
            {/if}
        {/each}
        {#if contriesDisplayed < filteredCountries.length - 1}
            <button
                class="load-more-button"
                on:click={() => (contriesDisplayed += 24)}>Load more...</button
            >
        {/if}
    </div>
</div>

<style lang="scss">
    .quiz {
        align-items: center;

        .input-and-info {
            width: 100%;
            margin: 6px 0px;
            justify-content: center;
            align-items: center;

            .search {
                width: 300px;
                height: 18px;
                margin: 0px 12px 0px 0px;
            }
        }

        .country-list {
            align-items: center;

            .country {
                justify-content: center;
                align-items: center;
                margin: 0px 0px 12px 0px;
            }

            .load-more-button {
                height: 22px;
                margin: 12px 0px 24px 0px;
            }
        }
    }
</style>
