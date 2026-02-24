<template>




</template>



<style>

.layout {
    display: grid;
    grid-template-columns: 200px 1fr;
}

.layout > * {
    border: solid 1px red;
}

.layout header,
.layout footer {
    grid-column: 1 / -1;
}


</style>


