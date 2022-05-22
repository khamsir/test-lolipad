<template>
    <div>{{title}}: <span class="font-weight-medium">{{value}}</span></div>
</template>

<script>
export default {
    props:{
        title: {
            type: String,
            default: '-'
        },
        value: {
            type: String,
            default: '-'
        },
    }
}
</script>