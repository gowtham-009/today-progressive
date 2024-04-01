<template>
    <div>
       <div class="layouts w-full p-5 ">
            <div class="contains w-full p-2">
                <Header/>
                <slot/>
            </div>
       </div>
    
    </div>
</template>

<script>



</script>

<style>
@media(max-width: 900px){
.layouts{
    padding: 0%;
}
}
</style>