<script lang="ts">
    const api: { key: string; base: string } = {
        key: "291cecb64261d7788117b97270bf97e8",
        base: "https://api.openweathermap.org/data/2.5/",
    };

    let search: string = "";
    let weather: any = {};
    let weather2: any = {};
    let forecast: any[] = [];

    const searchPressed = async (): Promise<void> => {
        const trimmedSearch: string = search.trim();

        if (trimmedSearch !== "") {
            if (!isNaN(Number(trimmedSearch))) {
                const response2: Response = await fetch(
                    `${api.base}weather?zip=${trimmedSearch}&appid=${api.key}&units=metric`,
                );
                weather2 = await response2.json();
                weather = {};

                const forecastResponse: Response = await fetch(
                    `${api.base}forecast?q=${weather2.name}&appid=${api.key}&units=metric`,
                );
                const forecastData: any = await forecastResponse.json();
                forecast = forecastData.list;

                const uvResponse: Response = await fetch(
                    `${api.base}uvi?lat=${weather2.coord.lat}&lon=${weather2.coord.lon}&appid=${api.key}`,
                );
                const uvData: any = await uvResponse.json();
                weather2.uvi = uvData.value;
            } else {
                const response: Response = await fetch(
                    `${api.base}weather?q=${trimmedSearch}&units=metric&APPID=${api.key}`,
                );
                weather = await response.json();
                weather2 = {};

                const forecastResponse: Response = await fetch(
                    `${api.base}forecast?q=${trimmedSearch}&appid=${api.key}&units=metric`,
                );
                const forecastData: any = await forecastResponse.json();
                forecast = forecastData.list;

                const uvResponse: Response = await fetch(
                    `${api.base}uvi?lat=${weather.coord.lat}&lon=${weather.coord.lon}&appid=${api.key}`,
                );
                const uvData: any = await uvResponse.json();
                weather.uvi = uvData.value;
            }
        }
    };
</script>

<div
    class="flex lg:flex-row flex-col justify-center items-center lg:items-start gap-12"
>
    <div
        class="border-2 bg-gradient-to-r from-cyan-700 to-blue-500 border-sky-600 backdrop-blur-3xl w-[450px] rounded-lg shadow-2xl h-[580px]"
    >
        <div class="m-3 my-6 flex justify-center items-center">
            <form>
                <div class="flex justify-center items-center">
                    <div class="input-container">
                        <input
                            placeholder="Enter city/town or ZIP Code..."
                            bind:value={search}
                            class="input p-3 w-[350px] text-white rounded-xl bg-gradient-to-r from-cyan-300 to-blue-500 border-sky-600"
                            name="text"
                            type="text"
                        />
                    </div>
                    <button
                        class="px-3 py-3 hover:scale-105 transition bg-gradient-to-l from-cyan-300 to-blue-500 border-sky-600 text-white rounded-lg"
                        on:click|preventDefault={searchPressed}
                        ><img
                            class="w-6 text-white"
                            src="https://i.ibb.co/p282Mcr/image-removebg-preview-60.png"
                            alt="search"
                        /></button
                    >
                </div>
            </form>
        </div>
        {#if weather.main !== undefined}
            <div class=" flex justify-center my-3 items-center">
                <img
                    class="w-[250px]"
                    src="https://i.ibb.co/YQBktzz/image-removebg-preview-44.png"
                    alt="weather images"
                />
            </div>
            <h1 class="text-4xl text-white mb-2 text-center font-bold">
                {weather.main.temp}°C
            </h1>
            <p class="text-white text-5xl mb-2 text-center">
                {weather.name}, {weather.sys.country}
            </p>
            <p class="text-white text-xl text-center">
                {weather.weather[0].description}
            </p>
            <div class="flex items-center justify-evenly gap-6 mt-8">
                <div class="">
                    <img
                        class="w-10 border-2 ml-4 rounded-full"
                        src="https://i.ibb.co/RgzvyxT/image.png"
                        alt=""
                    />
                    <p class="text-white font-bold">
                        ws : {weather.wind.speed} m/s
                    </p>
                </div>

                <div>
                    <img
                        class="w-10 border-2 rounded-full"
                        src="https://i.ibb.co/wzjt6vG/image-removebg-preview-53.png"
                        alt=""
                    />
                    <p class="text-white font-bold">
                        h: {weather.main.humidity}%
                    </p>
                </div>

                <div>
                    <img
                        class="w-10 border-2 rounded-full"
                        src="https://i.ibb.co/g4mYZwk/image-removebg-preview-55.png"
                        alt=""
                    />
                    <p class="text-white font-bold text-center">
                        uv: {weather.uvi}
                    </p>
                </div>
            </div>
        {/if}

        <!-- Zip code data heeree  -->

        {#if weather2.main !== undefined}
            <div class=" flex justify-center my-3 items-center">
                <img
                    class="w-[250px]"
                    src="https://i.ibb.co/wzjt6vG/image-removebg-preview-53.png"
                    alt="weather images"
                />
            </div>
            <h1 class="text-4xl text-white mb-2 text-center font-bold">
                {weather2.main.temp}°C
            </h1>
            <p class="text-white text-5xl mb-2 text-center">
                {weather2.name}, {weather2.sys.country}
            </p>
            <p class="text-white text-xl text-center">
                {weather2.weather[0].description}
            </p>
            <div class="flex items-center justify-evenly gap-6 mt-8">
                <div class="">
                    <img
                        class="w-10 border-2 ml-4 rounded-full"
                        src="https://i.ibb.co/RgzvyxT/image.png"
                        alt=""
                    />
                    <p class="text-white font-bold">
                        ws : {weather2.wind.speed} m/s
                    </p>
                </div>

                <div>
                    <img
                        class="w-10 border-2 rounded-full"
                        src="https://i.ibb.co/wzjt6vG/image-removebg-preview-53.png"
                        alt=""
                    />
                    <p class="text-white font-bold">
                        h: {weather2.main.humidity}%
                    </p>
                </div>

                <div>
                    <img
                        class="w-10 border-2 rounded-full"
                        src="https://i.ibb.co/g4mYZwk/image-removebg-preview-55.png"
                        alt=""
                    />
                    <p class="text-white font-bold text-center">
                        uv: {weather2.uvi}
                    </p>
                </div>
            </div>
        {/if}
        {#if !weather.main && !weather2.main}
            <div>
                <img
                    src="https://i.ibb.co/VThS50F/image-removebg-preview-66.png"
                    alt=""
                    class=" "
                />
                <h1
                    class="text-center flex justify-center items-center text-lg font-bold text-white bg-clip-text"
                >
                    Please enter a valid city name or ZIP code.
                </h1>
            </div>
        {/if}
    </div>

    <div>
        {#if forecast.length > 0}
            <div
                class="grid md:grid-cols-3 grid-cols-2 p-4 lg:p-0 lg:grid-cols-5 gap-4"
            >
                {#each forecast as day}
                    <div
                        class="flex flex-col items-center justify-center bg-blue-400 p-4 rounded-lg"
                    >
                        <p class="text-white font-semibold">
                            {new Date(day.dt * 1000).toLocaleDateString(
                                "en-US",
                                {
                                    weekday: "short",
                                },
                            )}
                        </p>
                        <img
                            class="w-16 h-16"
                            src={`http://openweathermap.org/img/w/${day.weather[0].icon}.png`}
                            alt="weather icon"
                        />
                        <p class="text-white">{day.weather[0].description}</p>
                        <p class="text-white">Temp: {day.main.temp}°C</p>
                    </div>
                {/each}
            </div>
        {/if}
    </div>
</div>
