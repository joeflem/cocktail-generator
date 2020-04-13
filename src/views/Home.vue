<template>
   <div>
        <Generator/>
   </div>
</template>

<script>
    import Generator from "../components/Generator";
    export default {
        name: 'Home',
        components: {
            Generator
        },
        data: function () {
            return {
              started: false,
            };
        }
    }
</script>
