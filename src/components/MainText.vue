<template>
    <div :class="`font-weight-medium text-h4 ${color}`">
              <div class="mr-2">{{text}}</div>
          </div>
</template>

<script>
export default {
    props:{
        text: {
            type: String,
            default: '-'
        },
        color: {
            type: String,
        }
    }
}
</script>