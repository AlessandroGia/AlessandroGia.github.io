<script>
    import { fly, fade } from "svelte/transition";
    import { onMount } from "svelte";

    const birthday = '2001-09-24'

    let splash = true;
    let welcome = false;

    let global_background_color = 'transparent'

    let stain_x = 0;
    let stain_y = 0;
    let stain_r = 100;

    let start_left;
    let start_right;
    let opacities = 1

    let transf 
    
    let divs_an = []

    let div_tech;
    let div_tech_height;
    
    let y;

    let max_y;
    let max_x;

    setTimeout(() => {
        welcome = true
            stain_x = 100;
            stain_y = 100;
            stain_r = 0;
        setTimeout(() => {
            splash = false

        }, 2599)
    }, 500)

    function isMorning() {
        const now = new Date();
        const currentHour = now.getHours();

        if (currentHour >= 6 && currentHour < 18) 
            return true;
        return false;
    }

    let interactive_div = []
    let width_interactive_div = [0, 0, 0, 0, 0, 0, 0, 0]

    let item_animations = [0,0,0,0,0,0,0,0]

    onMount(() => {
        max_y = window.innerHeight
        max_x = window.innerWidth
        start_left = -max_x/3
        start_right = -start_left
        transf = [start_left, start_right, start_left, start_right, start_left, start_right, start_left, start_right]
        opacities = [1,1,1,1,1,1,1,1]
        
        window.addEventListener('resize', () => {
            max_y = window.innerHeight
            max_x = window.innerWidth
        })

        window.addEventListener('scroll', () => {
            if (div_tech !== undefined && div_tech !== null)
                div_tech_height = div_tech.getBoundingClientRect().height;
            if (y > max_y / 3) {
                global_background_color = "rgba(255, 0, 0, 0.1)"
            } else {
                global_background_color = 'transparent'
            }
            divs_an.forEach((div, i) => {
                if (div !== undefined && div !== null) {
                    let start
                    let rect = div.getBoundingClientRect()
                    let end_an = (max_y/2) - (rect.height/2)

                    let start_an = end_an + rect.height

                    if (i % 2 == 0)
                        start = start_left
                    else 
                        start = start_right 

                    let inc = start - ((start_an - rect.top) * (start/rect.height))
                    if (y - rect.height > 0 && rect.top <= start_an && rect.top > end_an ) {
                        width_interactive_div[i] = (start_an - rect.top) * (rect.width/rect.height)
                        opacities[i] = (start_an - rect.top) * (1/rect.height)
                        transf[i] = inc
                        if (rect.top - rect.width/3 > end_an)
                            item_animations[i] = 0
                    } else {
                        if (rect.top <= end_an) {
                            transf[i] = 0
                            item_animations[i] = 1
                        }
                    }
                }    
            });

        })
    })
    




    
</script>




{#if splash}
    <div id="splash" out:fade>
        <div class="stain" style="width: {stain_x}vw; height: {stain_y}vh; border-radius: {stain_r}%"></div>
        {#if welcome}
            <span in:fade>
                {#if isMorning()}
                    Good Morning!
                {:else}
                    Good Evening!
                {/if} 
            </span>
            <span id="p_splash" in:fly={{y: -200, duration: 1000, delay: 500}}>
                Welcome to my portfolio.
            </span>
        {/if}
    </div>
{:else}

    <div id="div_container" style="background-color: {global_background_color};" in:fly={{y: 200, duration: 2000}}>
        <div id="top"  style="height: {max_y}px" >
            {#if y < max_y / 3}

                <div id="segment_1" in:fade={{delay:500}} out:fade></div>
                <div id="segment_2" in:fly={{y: 200, duration: 1000}} out:fly={{y: -200, duration: 1000}} style="width: {max_x}px; height: {max_y}px; left: {max_x * 0.2}px; top: -{max_y/2}px; transform: rotate({40 + y/5}deg);"></div>
                <div id="segment_3" in:fly={{y: -200, duration: 1000}} out:fly={{y: 200, duration: 1000}} style="width: {max_x}px; height: {max_y}px; right: {max_x * 0.2}px; bottom: -{max_y/2}px; transform: rotate({60 + y/5}deg);"></div>

                <div id="about_me" in:fly={{x: 500, duration: 1000, delay:500}} out:fade={{delay:500}}>
                    <span in:fade={{duration: 1000, delay: 1000}}>
                        I'm Alessandro Giacento, a {new Date().getFullYear() - new Date(birthday).getFullYear()}-years-old Computer Science student at The University of Pesche.
                    </span>
                </div>
            {/if}
        </div>


        
        {#if y > max_y / 3}
            <div id="div-tech" in:fly={{y: -200, duration: 1000}} out:fade>Technology</div>
            <div bind:this={div_tech} id="middle" style="height: {max_y * 2}px" in:fade={{duration: 1000}} out:fade>
                <div bind:this={divs_an[0]} class="item-left" style="transform: translateX({transf[0]}px); opacity:{opacities[0]};">
                    <div id="animated-div-tech">
                        {#if item_animations[0]}
                            <div id="animated-div-img-tech" out:fade={{duration: 200}}><img src="svg/code-slash-outline.svg" style="height: 4vw;" alt="Languages"></div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 200}} out:fade={{duration: 200}}>JavaScript/TypeScript</div>
                            <div class="animated-div-item-tech" in:fly={{y: -400, duration: 500, delay: 400}} out:fade={{duration: 200}}>HTML/CSS</div>
                            <div class="animated-div-item-tech" in:fly={{y: -600, duration: 500, delay: 600}} out:fade={{duration: 200}}>Python</div>
                            <div class="animated-div-item-tech" in:fly={{y: -800, duration: 500, delay: 800}} out:fade={{duration: 200}}>C/C++</div>
                            <div class="animated-div-item-tech" in:fly={{y: -1000, duration: 500, delay: 1000}} out:fade={{duration: 200}}>Java</div>
                        {:else}
                            <div id="animated-div-img-tech" in:fade={{delay: 500, duration: 200}}> <img src="svg/code-slash-outline.svg" style="height: 10vw;" alt="Languages"> </div>
                        {/if}
                    </div>
                </div>
                <div bind:this={divs_an[1]} class="item-right" style="transform: translateX({transf[1]}px); opacity:{opacities[1]}">
                    <div id="animated-div-tech">
                        {#if item_animations[1]}
                            <div id="animated-div-img-tech" out:fade={{duration: 200}} > <img src="svg/layers-outline.svg" style="height: 4vw;" alt="Frameworks"> </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 200}} out:fade={{duration: 200}}>
                                Node.js
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -400, duration: 500, delay: 400}} out:fade={{duration: 200}}>
                                React
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -600, duration: 500, delay: 600}} out:fade={{duration: 200}}>
                                Angular
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -800, duration: 500, delay: 800}} out:fade={{duration: 200}}>
                                Svelte
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -1000, duration: 500, delay: 1000}} out:fade={{duration: 200}}>
                                Flask
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -1200, duration: 500, delay: 1200}} out:fade={{duration: 200}}> 
                                FastApi
                            </div>
                        {:else}
                            <div id="animated-div-img-tech" in:fade={{delay: 500, duration: 200}}> <img src="svg/layers-outline.svg" style="height: 10vw;"  alt="Frameworks"> </div>
                        {/if}
                    </div>
                </div>
                <div bind:this={divs_an[2]} class="item-left" style="transform: translateX({transf[2]}px); opacity:{opacities[2]}">
                    <div id="animated-div-tech">
                        {#if item_animations[2]}
                            <div id="animated-div-img-tech" out:fade={{duration: 200}} > <img src="svg/server-outline.svg" style="height: 4vw;" alt="Databases"> </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 200}} out:fade={{duration: 200}}>
                                PostgreSQL
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 400}} out:fade={{duration: 200}}>
                                Mysql
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 600}} out:fade={{duration: 200}}>
                                MongoDB
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 800}} out:fade={{duration: 200}}>
                                Redis
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 1000}} out:fade={{duration: 200}}>
                                MariaDB
                            </div>
                        {:else}
                            <div id="animated-div-img-tech" in:fade={{delay: 500, duration: 200}}> <img src="svg/server-outline.svg" style="height: 10vw;" alt="Databases"> </div>
                        {/if}
                    </div>
                </div>
                <div bind:this={divs_an[3]} class="item-right" style="transform: translateX({transf[3]}px); opacity:{opacities[3]}">
                    <div id="animated-div-tech">
                        {#if item_animations[2]}
                            <div id="animated-div-img-tech" out:fade={{duration: 200}} > <img src="svg/ellipsis-horizontal-outline.svg" style="height: 4vw;" alt="Other"> </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 200}} out:fade={{duration: 200}}>
                                GitHub
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 400}} out:fade={{duration: 200}}>
                                Docker
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 600}} out:fade={{duration: 200}}>
                                Make
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 800}} out:fade={{duration: 200}}>
                                Npm
                            </div>
                            <div class="animated-div-item-tech" in:fly={{y: -200, duration: 500, delay: 1000}} out:fade={{duration: 200}}>
                                Pip
                            </div>
                        {:else}
                            <div id="animated-div-img-tech" in:fade={{delay: 500, duration: 200}}> <img src="svg/ellipsis-horizontal-outline.svg" style="height: 10vw;" alt="Other"> </div>
                        {/if}
                    </div>
                </div>
            </div>
        {:else}
            <div style="height: {max_y * 1.5}px; background-color: transparent">
                
            </div>
        {/if}

        {#if y >  ((max_y / 3) + div_tech_height)}
            <div id="bottom" style="height: {max_y * 0.2}px; width: 100%; background-color: transparent">
                <div in:fly={{duration: 2000, y: -200}} id="bottom">
                    <div class="contact-logo" in:fly={{x: -200, duration: 1000, delay: 500}} out:fade><a href="https://github.com/AlessandroGia" target="_blank"><img src="svg/logo-github.svg" style="height: 6vw;" alt="GitHub"></a></div>
                    <div class="contact-logo" in:fly={{x: 200, duration: 1000, delay: 500}} out:fade><a href="mailto:alessandrogiacento@alegiacento.dev"><img src="svg/mail.svg" style="height: 6vw;" alt="E-Mail"></a></div>
                    <div id="credits">
                        <div id="credit" in:fade={{duration: 500, delay: 600}} out:fade>
                            Icons taken from <a id="link-ionic" href="https://ionicframework.com" target="_blank">Ionic</a>
                        </div>
                    </div>
                    <div id="end-footer">
                        <div id="copyright" in:fly={{y: -200, duration: 500, delay:800}}  out:fade>
                            @ {new Date().getFullYear()} Alessandro Giacento
                        </div>
                        <div id="powered" in:fly={{y: -200, duration: 500, delay:800}} out:fade>
                            Powered by Svelte
                        </div>
                    </div>
                </div>
            </div>
        {:else}
            <div style="height: {max_y * 0.5}px; background-color: transparent">
                
            </div>
        {/if}
        
    </div>
{/if}


<svelte:window bind:scrollY={y} />

<style>


    #link-ionic {
        color: rgba(0, 0, 0, 0.5);
        text-decoration: none;
        transition: color 0.5s;
    }

    #link-ionic:hover {
        color: rgba(0, 0, 150, 0.5);
    }

    #credits {
        display: flex;
        justify-content: center;
        flex: 0 0 calc(100%);
        align-items: center;
        margin-bottom: 3vw;
        margin-top: 1vw;
    }

    #credit {
        text-align: center;
        width: 30vw;
        font-size: 3vw;
        padding-top: 2px;
        color: rgba(0, 0, 0, 0.5);
        border-top: 1px solid rgba(0, 0, 0, 0.1);
    }

    .contact-logo {
        display: flex;
        flex: 0 0 calc(50%);
        align-items: center;
        justify-content: center;
        padding-top: 10px;
    }

    #animated-div-tech {
        display: flex;
        flex-direction: column;
        height: 100%;
        align-items: center;
    }

    .animated-div-item-tech {
        display: flex;
        flex: 1;
        font-size: 3vw;
        justify-content: center;
        align-items: center;
        height: 100%;
        border-top: 1px solid rgba(0, 0, 0, 0.1);
    }

    #animated-div-img-tech {
        display: flex;
        flex: 1;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100%;

    }

    img {
        object-fit: ratio; 
    }

    .item-left {
        flex: 0 0 calc(50% - 20px);
        margin: 10px;
        padding: 10px;
        box-sizing: border-box; 
        background-color: transparent;
        box-shadow: 0 0 100px rgba(240,240,240, 0.1);
        border-radius: 10%;
        color: rgba(0, 0, 0, 0.675);
    }

    .item-right {
        flex: 0 0 calc(50% - 20px);
        margin: 10px;
        padding: 10px;
        box-sizing: border-box; 
        background-color: transparent;
        box-shadow: 0 0 100px rgba(240,240,240, 0.1); /* Aggiungi un'ombra sfocata */
        border-radius: 10%;
        color: rgba(0, 0, 0, 0.675);

    }

    #middle {
        width: 100%;
        background-color: rgba(0, 255, 255, 0.1);
        box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);
        display: flex;
        flex-wrap: wrap;
        overflow: hidden;
        margin-left: 10px;
        margin-right: 10px;
    }

    
    #div-tech {
        font-size: 10vw;
        text-align: center;
        color: gray;
        margin-bottom: 1em;
    }

    #segment_1 {
        position: absolute;
        background-color: green;
        width: 100%;
        height: 100%;
    }

    #segment_2 {
        position: absolute;
        background-color: red;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }

    #segment_3 {
        position: absolute;
        background-color: white;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }


    #top {
        width: 100%;
        display: flex;
        align-items: center;
        position: relative;
        overflow: hidden;
    }

    #about_me {
        position: absolute;
        align-items: center;
        font-size: 1.5em;
        padding: 5vw;
        margin-right: 5vw;
        margin-left: 5vw;
        background-color: #cccccc;
        border-radius: 25px;
        opacity: 0.8;
        transition: color 1s, filter;    
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }

    #bottom {
        display: flex;
        flex-wrap: wrap;
        width: 100%;
        height: 100%;
        background-color: white;
        box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
    }


    .stain {
        overflow: visible;
        position: fixed;
        opacity: 0.2;
        width: 50px;
        height: 50px;
        border-radius: 50%;
        background-color: red;
        transition: width 1.5s, height 1.5s, border-radius 1.5s; 
    }


    #splash {
        width: 100%;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        background-color: #ffffff;
    }

    #p_splash {
        font-size: 2em;
        width: 100%;
        text-align: center;
    }

    #div_container {
        display: flex;
        background-color: transparent;
        flex-direction: column;
        align-items: center; 
        justify-content: center;
        transition: background-color 2s;
    }

    #end-footer {
        display: flex;
        flex-wrap: wrap;
        width: 100%;
        background-color: #fefefe;
        padding-top: 3px;
        padding-bottom: 2px;

    }

    #powered {
        opacity: 0.4;
        color: gray;
        text-align: center;
        bottom: 0;
        font-size: 2.5vw;
        transition: opacity 1s, color 1s;
        flex: 0 0 calc(50%);
        align-self: flex-end;
        justify-content: left;
        
    }

    #powered:hover {
        opacity: 1;
        color: black;
    }

    #copyright {
        opacity: 0.4;
        color: gray;
        text-align: center;
        bottom: 0;
        font-size: 2.5vw;
        transition: opacity 1s, color 1s;
        flex: 0 0 calc(50%);
        align-self: flex-end;
        justify-content: right;
    }

    #copyright:hover {
        opacity: 1;
        color: black;
    }


   

</style>