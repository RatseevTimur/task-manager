<template>

    <ul>
        <li v-for="task in sortingTask()"
            :key="task.id"
            :class="{completed: task.completed, editing: inputId===task.id }" >

            <div class="row">
                <div class="task col s3 card-panel teal lighten-4">
                    <input type="date" class="datepicker card-panel teal lighten-4"
                    id="datePickerId" :min="date()">
                    
                </div>
                
                <div class="task col s9 card-panel teal lighten-4">
                    <form action="#"
                    class="left-align">
                        <p>
                        <label>
                            <input type="checkbox"
                            @click="sortingTask()"
                            v-model="task.completed"/>
                            <span>

                                <label class="todo" v-bind:class="{ active: task.completed }"
                                 @dblclick="editorTask(task);taskFocus(task)">
                                    {{task.text}}
                                </label>

                            </span>
                        </label>
                        </p>
                    </form>    

                     <a class="del waves-effect waves-teal btn-flat" @click="removeTask(task)">Удалить</a> 

                </div>

                <input
                    class="edit"
                    v-if="inputId===task.id"
                    :ref="'taskInput' + task.id" 
                    v-model="task.text"
                    @blur="enterEditor(task);resetInput()"
                    @keyup.enter="enterEditor(task);resetInput()"
                    @keyup.esc="exitEditor(task);resetInput()"
                />
            </div>
        </li>
    </ul>
 
</template>

<script>
export default {
    data(){
        return{
            inputId: null,
            beforeEditCache: null
        }
    },
    props: {
        tasks: {default: []},
        removeTask:{
            type: Function
        },
        enterEditor: {
            type: Function
        },
        exitEditor: {
            type: Function
        },
        editorTask: {
            type: Function
        },
        sortingTask: {
            type: Function
        }
    },
    name: 'TaskList',
    methods: {
        taskFocus: function (task) {
            this.inputId = task.id;
            this.$nextTick(() => {
                this.$refs['taskInput' + task.id][0].focus();
            });
        },
        resetInput: function(){
            this.inputId=null
        },
        date(){
            return (new Date().toISOString().split("T")[0]);
        }
    }
}
</script>