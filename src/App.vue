<template>
<div>

    <div v-bind:class="{loggedin: loggedIn}">
        <LoginPage v-on:login-user="handleLogin" />
   </div>
    <div v-if="loggedIn">
        <GoalsPage v-on:updated-date="handleUpdatedDate" v-on:create-new-goal="handleCreateNewGoal" v-bind:goals="goals" v-bind:initMonth="monthYearInput" v-on:update-curr="handleUpdateCurr"/>
   </div>
</div>
</template>

<script>
import LoginPage  from './components/LoginPage.vue'
import GoalsPage from './components/GoalsPage.vue'

export default {
    name: 'App',
    components: {
       LoginPage,
       GoalsPage,
    },
    data() {
        return {
            loggedIn: false,
            monthYearInput: '',
            userId: 0,
            goals: '',
        }
    },
    methods: {
        getGoals(){
            var url = "https://cors-anywhere.herokuapp.com/http://patrickmalara.com/mongoal/api/getGoals.php"
            var data = "userId="+ this.userId + "&date=" + this.monthYearInput;

            fetch(url, {
                method: 'POST', 
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
                body: data 
            }).then(response => response.json()
            ).then(result => {
                this.goals = result;
                console.log(result)
            });

        },
        
        handleLogin( usernameInput, passwordInput){
            console.log(usernameInput);
            this.loggedIn = true;    
           
            var url = "https://cors-anywhere.herokuapp.com/http://patrickmalara.com/mongoal/api/login.php"
            var data = "Username="+ usernameInput + "&Password="+ passwordInput;

            fetch(url, {
                method: 'POST', 
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
                body: data 
            }).then(response => response.json()
            ).then(result => {
                this.userId = result.id;

                //These lines control the default date that is loaded
                var date = new Date();
                if(date.getMonth() + 1 < 10){
                    this.monthYearInput = date.getFullYear() + "-0" + (date.getMonth() + 1);
                }else {
                    this.monthYearInput = date.getFullYear() + "-" + (date.getMonth() + 1);
                }

                this.getGoals();
                console.log(result.id)
            });

        },
        
        handleUpdatedDate(monthYearInput){
            this.monthYearInput = monthYearInput;   
            this.getGoals();
        },

        handleCreateNewGoal(newGoalTitle, newGoalOutOf){
            console.log(newGoalTitle, newGoalOutOf);
            
            var url = "https://cors-anywhere.herokuapp.com/http://patrickmalara.com/mongoal/api/createGoal.php";
            var data = "NewGoalTitle="+ newGoalTitle + "&NewGoalOutOf="+ newGoalOutOf + "&MonthYear=" + this.monthYearInput + "&UserId=" + this.userId;

            fetch(url, {
                method: 'POST', 
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
                body: data 
            }).then(response => response.text()
            ).then(result => {

                this.getGoals();
                console.log(result);
            });

        },

        handleUpdateCurr(goalId, isIncreased){
            console.log(goalId, isIncreased);
            
            var url = "https://cors-anywhere.herokuapp.com/http://patrickmalara.com/mongoal/api/updateCurr.php";
            var data = "GoalId="+ goalId + "&isIncreased="+ isIncreased;

            fetch(url, {
                method: 'POST', 
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
                body: data 
            }).then(response => response.text()
            ).then(result => {

                this.getGoals();
                console.log(result);
            });
  
        }
        
    }
}
</script>

<style>
    body {
        background-color: #28a745;
    }
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.loggedin {
    display: none;    
}
</style>
