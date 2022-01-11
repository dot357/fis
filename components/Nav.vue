<template>
    <nav>
        <nuxt-link to="/"><div class="logo">freeinvoice.software</div></nuxt-link>
        <ul>
           
            <li>Road Map</li>
            <nuxt-link to="/builder"><li>Invoicer</li></nuxt-link>
            <li>User stuff</li>
            <a href="https://www.buymeacoffee.com/dot357" target="_blank"><li class="buyMe">Buy me a coffee</li></a>
        </ul>
    </nav>
</template>

<script>
export default {
    name : 'Nav'
}
</script>


<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@400;700&display=swap');
nav{
    background: var(--base-color);
    color: white;
    height: 60px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    padding: 0 4rem;
}
nav a{
    color: inherit;
    text-decoration: none;
}
nav .logo{
    font-weight: 500;
    font-size: 1.2rem;
    letter-spacing: 1px;
    font-weight: 600;
}

nav ul{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    gap: 25px;
    list-style: none;
}

nav ul li{
    position: relative;
    cursor: pointer;
}

nav ul li:not(.buyMe)::before{
    content: '';
    display: block;
    width: 0%;
    height: 1px;
   background: #FFDD00;
   border-radius: 999px;
    position: absolute;
    top: 99%;
}

nav ul li:hover:not(.buyMe)::before{
    transition: 0.4s cubic-bezier(0.075, 0.82, 0.165, 1);
    width: 100%;
    
}

nav .buyMe{
    background: #FFDD00;
    color: black;
    font-weight: 700;
    font-size: 1.3rem;
    padding: 6px 12px;
    border-radius: 4px;
    font-family: 'Amatic SC', cursive;
    box-shadow: 0px 0px 25px -5px #FFDD00;
}

nav .buyMe:hover {
    transition: all 0.3s ease;
    transform: scale(0.96);
    box-shadow: 0px 0px 25px -5px #ffdd0000;
}
</style>