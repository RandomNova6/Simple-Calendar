<template>
    <div>
    <div class="Sim-Todo-Part">
        <div class="todo-list" :v-if="todos.length()" v-for="todo in todos" :key="todo.id" >
            <span class="todo">{{todo.title}}</span>
            </div>
    </div>
    <div class="Sim-Todo-Details" v-if="todoActive" v-focus tabindex="-1" @keydown="handleKey">
        <div class="shade-left"></div>
        <div class="shade-right"></div>
        <div class="shower" >
            <div class="title">
                {{ theDate }}
            </div>
            <div class="escape" @click.stop="closeShower">
                X
                </div>
            <div class="todo-list-details" v-for="todo in todos" :key="todo.id">

                </div>
            <button class="add-more">
                添加日程
            </button>
        </div>
        <div class="editor" v-if="editActive">
            
        </div>
    </div>
    </div>
</template>

<script>

export default{
    props:{
        currentMonth:{},
        date:{},
    },
    data(){
        return{
            todoActive:false,
            editActive:false,
            todos: [],
            newId:1,
            newTitle:{},
            newNote:{},
            newStartTime:{},
            newEndTime:{},
            newDuration:{},
        }
    },
    computed:{
        theDate(){
            return this.currentMonth.clone().date(this.date).format("dddd,MMMM Do");
        }
    },
    methods:{
        setTodo(state){
            this.todoActive=state;
            if(state) {
             this.$nextTick(() => {
            this.$el.querySelector('.Sim-Todo-Details')?.focus();
            });
        }},
        setEdit(state){
            this.editActive=state;
        },
        closeShower(){
            this.setTodo(false);
        },
        closeEditor(){
            this.setEdit(false);
        },
        handleKey(event){
            if(event.key === 'Escape'&&!this.editActive)this.closeShower();
            else if(event.key === 'Escape'&&this.editActive) this.closeEditor();
        }
    },
}
</script>

<style scoped>
.Sim-Todo-Part{

}
.Sim-Todo-Details{
    position: fixed;
    top: 0;
    left: 35vw;
    width: 30vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000; /* 确保在最上层 */
    background: rgba(68, 159, 212, 0.95); /* 可选背景 */
    border-radius: 7pt 7pt 7pt 7pt;
}
.todo-list{

}
.todo{

}
.shower{

}
.editor{

}
.title {
    position: absolute;
    top: 5%;
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
    width: auto;
}
.escape{
    position: absolute;
    right:10%;
    top:5%;
    width:10pt;
    cursor: pointer;
    background: rgba(209, 119, 45, 0.203);
    border-radius: 2pt 2pt 2pt 2pt;
}
.todo-list-details{

}
.add-more{
    position: absolute;
    bottom: 5%;
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
    width: auto;
}
.shade-left{
    position: fixed;
    top: 0;
    left: 0;
    width: 35vw;
    height: 100vh;
    z-index: 1000; /* 确保在最上层 */
    background:transparent;
}
.shade-right{
    position: fixed;
    top: 0;
    right: 0;
    width: 35vw;
    height: 100vh;
    z-index: 1000; /* 确保在最上层 */
    background:transparent;
}
</style>