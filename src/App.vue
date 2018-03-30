<template>
    <div id="app">
        <Notewriter />
    </div>
</template>

<script>
require('./assets/roolith.min.css');
import Notewriter from './components/Notewriter.vue'

export default {
    name: 'app',
    components: {
        Notewriter
    }
}
</script>

<style>

</style>
