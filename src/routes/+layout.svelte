<script>
    import Home from "../Component/Home/Home.svelte";
    import "../app.css";
</script>

<div class="app">
    <div>
        <h1
            class="lg:text-6xl md:text-5xl text-4xl my-12 font-bold text-center text-sky-400 font-sans"
        >
            Welcome To Our Weather Webside
        </h1>
    </div>
    <main>
        <Home />
    </main>
</div>

<slot />
