<template>
<div class="container">

    <div class="goalsContainer">
        <input v-on:change="updateDate()" class="form-control mb-5" v-model="monthYearInput" type="month" name="monthYear">
        
        <form v-on:submit.prevent="addNewGoal()" class="form-inline">
            <div class="form-group" style="margin-left: 10px;">
                <label>New Goal Name</label>
                <input v-model="newGoalTitle" class="form-control ml-2" type="text" name="newgoalname" >
            </div>
            <div class="form-group" style="margin-left: 10px;">
                <label>New Goal Total</label>
                <input v-model="newGoalOutOf" class="form-control ml-2" type="number" name="newgoaloutof">
            </div>
            <div style="text-align: center; margin-left: 10px;">
                <button class="btn btn-success">Create Goal</button>
            </div>
        </form> 
        <br /> 
        
        <ul class="list-group">
            <!-- When I saw v-for it blew my mind!!!!! honestly one of the coolest things :) -->
            <GoalItem
                v-for="goal in this.goals" 
                v-bind:key="goal.id"
                v-bind:title="goal.title"
                v-bind:curr="goal.curr"
                v-bind:outOf="goal.outOf"
                v-bind:id="goal.id"
                v-on:update-curr="updateGoalsCurr"
            ></GoalItem>
        </ul>
    </div>
</div>
</template>

<script>
import GoalItem from './GoalItem.vue'

export default {
    name: 'GoalsPage',
    components: {
        GoalItem    
    },
    props: ['goals', 'initMonth'],
    data(){
        return {
            monthYearInput: '',
            newGoalTitle: '',
            newGoalOutOf: ''
        }
    },
    created(){
        //These lines control the default date that is loaded
        var date = new Date();
        if(date.getMonth() + 1 < 10){
            this.monthYearInput = date.getFullYear() + "-0" + (date.getMonth() + 1);
        }else {
            this.monthYearInput= date.getFullYear() + "-" + (date.getMonth() + 1);
        }

    
    },
    methods: {
        updateDate(){
            this.$emit('updated-date', this.monthYearInput);    
        },

        addNewGoal(){
            this.$emit('create-new-goal', this.newGoalTitle, this.newGoalOutOf);    
        },

        updateGoalsCurr(goalId, isIncreased){
            this.$emit('update-curr', goalId, isIncreased);    
        }
    }
    
}
</script>

<style>
    .goalsContainer {
        background-color: #FFF;
        border-radius: 10px;
        padding: 20px;
        margin-top: 10%;
    }
</style>
