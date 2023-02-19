<script lang="ts">
    let countries: Country[] = []; // Countries data
    let countriesNumber: number; // Number of countries

    let search: string = ""; // Search for filtering
    let filteredCountries: Country[] = []; // Filter applyed

    $: search, (filteredCountries = filterCountries(search));

    let contriesDisplayed: number = 24; // Number of countries displayed

    /**
     * Filter contries regarding to user's search query
     */
    function filterCountries(s: string): Country[] {
        return countries
            .filter((c) => {
                return (
                    c.countryName.toLowerCase().includes(s.toLowerCase()) ||
                    c.continentName.toLowerCase().includes(s.toLowerCase()) ||
                    c.capital.toLowerCase().includes(s.toLowerCase())
                );
            })
            .sort((a: Country, b: Country) => {
                return parseInt(a.population) < parseInt(b.population) ? 1 : -1;
            });
    }

    /**
     * Add K, M or B to shorten integer values
     */
    function numberify(n: string): string {
        const l = n.length;
        if (l <= 3) {
            return n;
        } else if (l <= 6) {
            return n.substring(0, l - 3) + "," + n.charAt(l - 3) + "K";
        } else if (l <= 9) {
            return n.substring(0, l - 6) + "," + n.charAt(l - 6) + "M";
        } else if (l <= 12) {
            return n.substring(0, l - 9) + "," + n.charAt(l - 9) + "B";
        } else {
            return n;
        }
    }

    // When init component, fetch country data
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

<div class="country-container fc">
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
                <div class="country f">
                    <div class="flag f">
                        <img
                            class="flag-svg"
                            src="./flags/{c.countryCode.toLowerCase()}.svg"
                            alt="flag of {c.countryName}"
                        />
                    </div>
                    <div class="country-info fc ">
                        <div class="logo-and-property">
                            <span class="country-name">{c.countryName}</span>
                        </div>
                        {#if c.capital}
                            <div class="logo-and-property" title="Capital">
                                <img
                                    class="icon"
                                    src="./icons/capital.svg"
                                    alt="capital icon"
                                />
                                <span class="country-capital">{c.capital}</span>
                            </div>
                        {/if}
                        {#if c.population}
                            <div class="logo-and-property" title="Population">
                                <img
                                    class="icon"
                                    src="./icons/population.svg"
                                    alt="population icon"
                                />
                                <span class="country-population"
                                    >{numberify(c.population)}</span
                                >
                            </div>
                        {/if}
                        {#if c.continentName}
                            <div class="logo-and-property" title="Continent">
                                <img
                                    class="icon"
                                    src="./icons/earth.svg"
                                    alt="earth icon"
                                />
                                <span class="continent-name"
                                    >{c.continentName}</span
                                >
                            </div>
                        {/if}
                    </div>
                </div>
            {/if}
        {/each}
        {#if contriesDisplayed < filteredCountries.length - 1}
            <button
                class="load-more-button"
                on:click={() => (contriesDisplayed += 24)}>Load more...</button
            >
            <button
                class="load-more-button"
                on:click={() => (contriesDisplayed += countriesNumber)}
                >Load all</button
            >
        {/if}
    </div>
</div>

<style lang="scss">
    .country-container {
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
            width: 100%;
            align-items: center;

            .country {
                width: 100%;
                margin-bottom: 12px;

                .flag {
                    width: 50%;
                    margin-right: 6px;
                    justify-content: flex-end;

                    .flag-svg {
                        height: 120px;
                    }
                }

                .country-info {
                    width: 50%;
                    margin-left: 6px;

                    .logo-and-property {
                        display: flex;
                        align-items: center;

                        .icon {
                            width: 14px;
                            height: 14px;
                            margin-right: 6px;
                        }

                        .country-name {
                            font-size: 16px;
                            margin-bottom: 6px;
                        }

                        .country-capital,
                        .country-population,
                        .continent-name {
                            font-size: 12px;
                            color: #aaa;
                        }
                    }
                }
            }

            .load-more-button {
                height: 22px;
                margin: 12px 0px 24px 0px;
            }
        }
    }

    @media (max-width: 700px) {
        .country-container {
            .country-list {
                .country {
                    .flag {
                        .flag-svg {
                            height: 72px;
                        }
                    }
                }
            }
        }
    }
</style>
