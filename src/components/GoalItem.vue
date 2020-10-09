<template>
<div> 
    <li class="list-group-item d-flex justify-content-between align-items-center">
        {{title}} 
        <span> 
            <a v-on:click="emitCurrUpdate(id, false)" style="font-size: 1.5rem; cursor: pointer;">-</a> 
            <span class="badge badge-success badge-pill" style="width: 50px; margin-left: 10px; margin-right: 10px;">{{curr}}/{{outOf}}</span>
            <a v-on:click="emitCurrUpdate(id, true)" style="font-size: 1.5rem; cursor: pointer;">+</a>
        </span>
    </li>
</div>
</template>

<script>
export default {
    name: 'GoalItem',
    props: ['title', 'curr', 'outOf', 'id'],
    data(){
        return {
        }
    },
    methods: {
        emitCurrUpdate(goalId, isIncreased){
            this.$emit('update-curr', goalId, isIncreased);    
        }
    }
    
}
</script>

<style>
</style>
