<template>
    <div><label><input type="checkbox" :checked="!anyRemaining" @change="allChecked"> Check All</label></div>
</template>


<script>
export default {
    name: 'todo-check-all',
    /* props: {
        anyRemaining:{
            type: Boolean,
            required: true,
        }
    }, */
    computed:{
        anyRemaining(){
            return this.$store.getters.anyRemaining
        }
    },
    methods: {
        allChecked(){
            this.$store.dispatch('checkAll', event.target.checked)
        }
    }
}
</script>
