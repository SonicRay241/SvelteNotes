<script lang="ts">
    export let title: string
    export let thumbname: string
    export let color: string = "fff"

    const hexRe = new RegExp(/^([0-9, a-f]{1,6})$/g)
    const rgbRe = new RegExp(/^([0-9]{1,3},[\s\S]{0,1}[0-9]{1,3},[\s\S]{0,1}[0-9]{1,3})$/g)

    if (hexRe.test(color)) {
        color = `#${color}`
    } else if (rgbRe.test(color)) {
        color = `rgb(${color})`
    } else {
        color = "#fff"
    }
</script>
<div class="wrapper">
    <div class="card">
        <h2 style="color: black;"><strong>{thumbname}</strong></h2>
        <div class="color" style="background-color: {color};"></div>
    </div>
    <h3>{title}</h3>
</div>


<style>
    .card {
        width: 100%;
        height: 12rem;
        padding: 1rem;
        background-color: white;
    }
    .color {
        width: 100%;
        height: 1rem;
    }
    .wrapper {
        margin: 0;
        padding: 0;
        width: 8rem;
        height: 13rem;
        margin-bottom: 8rem;
    }
</style>