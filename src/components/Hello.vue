<template>
  		<section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input class="new-todo" placeholder="What needs to be done?" autofocus @keyup.13="add()" v-model="newTask">
			</header>
			<!-- This section should be hidden by default and shown when there are todos -->
			<section class="main">
				<input class="toggle-all" type="checkbox" @change="toggleAll()" v-model="all">
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<!-- These are here just to show the structure of the list items -->
					<!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
					<li v-for="(item,index) in myTasks" :class="{completed:item.isCompleted,editing:index == myIndex}">
						<div class="view">
							<input class="toggle" type="checkbox" v-model="item.isCompleted">
							<label @dblclick="edit(index)">{{item.name}}</label>
							<button class="destroy" @click="del(index)"></button>
						</div>
						<input class="edit" v-model="item.name" @keyup.13="save()">
					</li>
					<!--<li>
						<div class="view">
							<input class="toggle" type="checkbox">
							<label>Buy a unicorn</label>
							<button class="destroy"></button>
						</div>
						<input class="edit" value="Rule the web">
					</li>-->
				</ul>
			</section>
			<!-- This footer should hidden by default and shown when there are todos -->
			<footer class="footer">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count"><strong>{{count}}</strong> item left</span>
				<!-- Remove this if you don't implement routing -->
				<ul class="filters">
					<li>
						<a :class="{selected:myShow == 1}"  href="#/" >All</a>
					</li>
					<li>
						<a  :class="{selected:myShow == 2}" href="#/active" >Active</a>
					</li>
					<li>
						<a :class="{selected:myShow == 3}" href="#/completed" >Completed</a>
					</li>
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed" @click="clearCompleted()">Clear completed</button>
			</footer>
		</section>
</template>

<script>
export default {
  name: 'hello',
    data () {
      return {
        tasks:[
          {name:'吃饭',isCompleted:true},
          {name:'睡觉',isCompleted:false},
          {name:'打豆豆',isCompleted:true}
        ],
        newTask:'',
        all:false,
        myIndex:-1
      }
    },
    computed:{
		myShow:function(){
			var status = this.$route.params.status;
			console.log(status);
			switch(status){
				case undefined:
					return 1;
					break;
				case 'active':
					return 2;
					break;
				case 'completed':
					return 3;
					break;
			}
		},
		count:function(){
			return this.tasks.filter(function(item){return item.isCompleted == false}).length;
		},
		myTasks:function(){
			if(this.myShow === 1){//全选 
				return this.tasks;
			}else if(this.myShow === 2){//只显示未完成
				return this.tasks.filter(function(item){return item.isCompleted == false});
			}else{//只显示已完成
				return this.tasks.filter(function(item){return item.isCompleted == true});
			}
		}
	},
	watch:{
		'$route':'changeStatus'
	},
	methods:{
		del:function(index){
			// console.log(index);
			this.tasks.splice(index,1);
		},
		add:function(){
			if(this.newTask== '')return;
			this.tasks.push({name:this.newTask,isCompleted:false});
			this.newTask = '';
		},
		toggleAll:function(){
			// console.log(111);
			// for(var i=0;i<this.tasks.length;i++){
			// 	this.tasks[i].isCompleted = this.all;
			// }
			var that = this;
			//映射
			this.tasks = this.tasks.map(function(item){
				item.isCompleted = that.all;
				return item;
			});
		},
		clearCompleted:function(){
			// map reduce filter
			// [2,3,4]
			this.tasks = this.tasks.filter(function(item){return item.isCompleted == false});
		},
		changeStatus:function(){
			// console.log(this.$route.params.status);
			var status = this.$route.params.status;
			switch(status){
				case undefined:
					this.myShow = 1;
					break;
				case 'active':
					this.myShow = 2;
					break;
				case 'completed':
					this.myShow = 3;
					break;
			}
		},
		// //约定：如果是1 --> 全部 
		// myAll:function(){
		// 	this.myShow = 1;
		// },
		// myActive:function(){	
		// 	this.myShow = 2;
		// },
		// myCompleted:function(){
		// 	this.myShow = 3;
		// },
		edit:function(index){
			this.myIndex = index;
		},
		save:function(){
			this.myIndex = -1;
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
