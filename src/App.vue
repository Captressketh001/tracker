<template>
<div>
  <header></header>
  <main>
    <div class="todo-body">
      <div class="todo-header">
        <h1>Ex-track</h1>
        <img src="/images/icon-sun.svg" alt="">
      </div>
      <div class="todo-input">
        <input type="text" placeholder="Enter expense title..." v-model="expense"  required>
        <input type="text" placeholder="Enter expense Amount..." v-model="amount"  required>
        <button class="btn" @click="addExpense()">Add Expense</button>
      </div>

      <div class="todo-tasks" v-if="fetchExpense ==''" style="height:80px">
        <p style="text-align:center" >Your Expense List is empty</p>
      </div>
      <div class="todo-container" v-else>
        <div class="todo-tasks" v-for="(expenses, ix) in fetchExpense" :key="ix">
          <input type="checkbox" name="" id="" v-model="expenses.completed" title="Mark Expense as completed">
          <p v-if="expenses.completed"><s>{{expenses.expense}}</s></p>
          <p v-else>{{expenses.expense}}</p>
          <p v-if="expenses.completed"><s>${{expenses.amount}}</s></p>
          <p v-else>${{expenses.amount}}</p>
          <img src="/images/icon-cross.svg" alt="" @click="removeExpense(expenses.id)" title="Remove Expense" style="cursor:pointer">
        </div>
         <div class="todo-state">
          <p>{{this.$store.state.todo.length}} left</p>
          <div class="active">
            <p @click="addToFilter('all')" style="cursor:pointer">All</p>
          <p @click="addToFilter(true)" style="cursor:pointer">Completed</p>
          </div>
        </div> 
      </div>
    </div>
  </main>
</div>
  
</template>

<script>

import { v4 } from 'uuid';
export default {
  name: 'App',
  data (){
    return{
        expense:'',
        amount:'',
        filtered:''
    }
  },
  computed:{
    fetchExpense(){
      let x  = this.$store.state.todo
      if (this.filtered == '' || this.filtered  == 'all'){
        return x
      } 
      else{
      return x.filter(list => {
				if (list.completed == this.filtered) {
					return list.completed == true;
				}
      
			});
      }
    }
  },
  methods:{
    addExpense(){
      let data = {
        id: v4(),
        expense:this.expense,
        amount: this.amount,
        completed:false
      }
      this.$store.dispatch('pushTodo', data)
      this.amount = ''
      this.expense = ''
    },
    addToFilter(data){
         this.filtered = data
         console.log('this', this.filtered)
    },
    removeExpense(id){
      this.$store.dispatch('doDelete', id)
    },
    clearComplete(){
      for(let x  in this.$store.state.todo){
        console.log('x', this.$store.state.todo[x].completed)
        if(this.$store.state.todo[x].completed){
          this.$store.state.todo.splice(x);
        }
      }
    }
  }
}
</script>

<style scoped>

</style>
