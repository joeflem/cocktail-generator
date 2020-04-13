<template>
    <div class="generator">
      <slot v-if="!started">

      </slot>
    </div>
</template>

<script>

    export default {
        name: 'Home',
        data: function () {
            return {
              started: false,
            };
        }
    }
</script>
