<template>
    <div class>
    <div class="Sim-Todo-Part">
        <div v-if="todos.length==1" class="todo-list">
        <div v-for="todo in todos" :key="todo.id" >
            <span :class='{"todo-brown":todo.color=="brown","todo-gray":todo.color=="gray","todo-green":todo.color=="green"}'>
                {{todo.title}}
            </span>
            </div>
            <span class="empty"></span>
            <span class="empty"></span>
            </div>
        <div v-if="todos.length==2" class="todo-list">
        <div v-for="todo in todos" :key="todo.id" >
            <span :class='{"todo-brown":todo.color=="brown","todo-gray":todo.color=="gray","todo-green":todo.color=="green"}'>{{todo.title}}</span>
            </div>
            <span class="empty"></span>
            </div>
        <div v-if="todos.length>=3" class="todo-list">
        <div v-for="todo in todos.slice(0,2)" :key="todo.id" >
            <span :class='{"todo-brown":todo.color=="brown","todo-gray":todo.color=="gray","todo-green":todo.color=="green"}'>{{todo.title}}</span>
            </div>
            <span class="more">...</span>
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
                    <div class="todo-details">
                    <span>{{ todo.title }}</span>
                    <span>{{ todo.startHour }}:{{ todo.startMin }} - {{ todo.endHour }}:{{ todo.endMin }}</span>
                    </div>
                </div>
            <button class="add-more" @click.stop="openEditor">
                添加日程
            </button>
        </div>
        <div class="editor" v-if="editActive">
            <div class="escape" @click.stop="closeEditor">
                X
            </div>
            <form>
                <div>
                    待办事项
                    <input type="text" placeholder="e.g 听音乐" v-model="newTitle" @click.stop>
                </div>
                <div>
                    备注
                    <input type="text" placeholder="(选填)" v-model="newNote" @click.stop>
                </div>
                <div>
                    <span>颜色</span>
                    <span class="spholder">{{ newColor }}</span>
                    <select v-model="newColor" @click.stop>
                        <option>brown</option>
                        <option>green</option>
                        <option>grey</option>
                    </select>
                </div>
                <div>
                    <span>起始时间</span>
                    <span class="spholder">{{ startTime }}</span>
                    <select v-model="Start.newStartHour" @click.stop>
                        <option v-for="hour in hours" :key="hour">{{ hour }}</option>
                    </select>
                    <select v-model="Start.newStartMin" @click.stop>
                        <option v-for="min in minutes" :key="min">{{ min }}</option>
                    </select>
                </div>
                <div>
                    <span>结束时间</span>
                    <span class="spholder">{{ endTime }}</span>
                    <select v-model="End.newEndHour" @click.stop>
                        <option v-for="hour in hours" :key="hour">{{ hour }}</option>
                    </select>
                    <select v-model="End.newEndMin" @click.stop>
                        <option v-for="min in minutes" :key="min">{{ min }}</option>
                    </select>
                </div>
                <button @click.prevent="submitTodo" @keydown.Enter="submitTodo">提交</button>
                </form>
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
            newTitle:'',
            newNote:'',
            Start:{
                newStartHour: null,
                newStartMin: null, 
            },
            End:{
                newEndHour: null,
                newEndMin: null, 
            },
            newColor:'brown',
        }
    },
    computed:{
        theDate(){
            return this.currentMonth.clone().date(this.date).format("dddd,MMMM Do");
        },
        hours(){
            let hours=[];
            for(let i=0;i<24;i++){
                hours.push(i);
            }
            return hours;
        },
        minutes(){
            let mins=[];
            for(let i=0;i<60;i++){
                mins.push(i);
            }
            return mins;
        },
        startTime(){
            return this.timeFormatter(this.Start.newStartHour, this.Start.newStartMin);
        },
        endTime(){
            return this.timeFormatter(this.End.newEndHour, this.End.newEndMin);
        },
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
        openEditor(){
            this.setEdit(true);
        },
        handleKey(event){
            if(event.key === 'Escape'&&!this.editActive)this.closeShower();
            else if(event.key === 'Escape'&&this.editActive) this.closeEditor();
        },
        timeFormatter(hour,min){
            if(hour&&min)return `${hour.toString().padStart(2,'0')}:${min.toString().padStart(2,'0')}`;
            else if(hour)return `${hour.toString().padStart(2,'0')}:00`
            else if(min)return `00:${min.toString().padStart(2,'0')}`
            else return '00:00';
        },
        submitTodo() {
            this.todos.push({
            id: this.newId++, // 建议添加唯一ID
            title: this.newTitle,
            note: this.newNote,
            color: this.newColor,
            startHour: this.Start.newStartHour, 
            startMin: this.Start.newStartMin,
            endHour: this.End.newEndHour,
            endMin: this.End.newEndMin,
            });
            this.newTitle = '';
            this.newNote = '';
            this.newColor ='brown';
            this.Start.newStartHour = null;
            this.Start.newStartMin = null;
            this.End.newEndHour = null;
            this.End.newEndMin = null;

            this.closeEditor();
        },
        checkTodo(){
            if(this.End.newEndHour<this.Start.newStartHour){

            }
            else if(this.End.newEndMin<this.Start.newStartMin){

            }
            if(title==''){

            }
        }
    },
}
</script>

<style scoped>
.Sim-Todo{

}
.Sim-Todo-Part {
    position: absolute;
    top: 0; 
    left: 0; 
    width: 100%;
    height: 100%;
    pointer-events: none; 
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
    background: rgba(113, 179, 220, 0.95); /* 可选背景 */
    border-radius: 7pt 7pt 7pt 7pt;
}
.todo-list {
    position: absolute;
    top: 20%;
    left: 5%;
    width: 90%;
    height: 70%;
    overflow-y: auto;
    grid-template-rows: 14%,14%,14%,10%;
    display: grid;
}
.todo-brown{
    display: flex;
    margin-bottom: 1%;
    align-items: center;
    justify-content: center;
    border-radius:4pt 4pt 4pt 4pt ;
    background-color: rgb(171, 154, 108);
}
.todo-green{
    display: flex;
    margin-bottom: 1%;
    align-items: center;
    justify-content: center;
    border-radius:4pt 4pt 4pt 4pt ;
    background-color: rgb(157, 187, 122);
}
.todo-gray{
    display: flex;
    margin-bottom: 1%;
    align-items: center;
    justify-content: center;
    border-radius:4pt 4pt 4pt 4pt ;
    background-color: rgb(169, 168, 168);
}
.empty{

}
.more{
    display: flex;
    margin-bottom: 1%;
    align-items: center;
    justify-content: center;
    border-radius:4pt 4pt 4pt 4pt ;
    background-color: rgb(240, 131, 63);
}
.shower{

}
.editor{
    position: fixed;
    top: 20vh;
    left: 35vw;
    width: 30vw;
    height: 60vh;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1001; /* 确保在最上层 */
    background: rgba(132, 187, 219, 0.95); /* 可选背景 */
    border-radius: 7pt 7pt 7pt 7pt;
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
.todo-details{

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
form{
    grid-template-columns: 25% 25% 25% 25%;
    grid-template-rows: 20% 20% 20% 20% 20%;
}
form div{
    display: flex;
    align-items: center;
    margin: 10px 0;
    padding: 5px;
}
.spholder{
    display: inline-block;
    width: 30%;
    min-width: 100px;
    height:15pt;
    background-color: rgb(255, 255, 255);
    border: 1px #adb5bd;
    border-radius: 4px;
    margin: 0 10px;
}
input{
    margin: 0 10px;
    background-color: rgb(255, 255, 255);
    border: 1px #adb5bd;
    border-radius: 4px;
}
</style>