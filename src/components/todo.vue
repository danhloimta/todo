<template>
    <div class="col-sm-6">
        <div class="list-todo">
            <div 
                v-if="todo.length"
            >
                <div 
                    class="item todo-item"
                    v-for="(task, index) in todo"
                    v-bind:key="index"
                >
                    <div class="form-check form-check-inline">
                        <input class="inp-cbx todo d-none" :id="'cbx' + task.id" type="checkbox"/>
                        <label
                            class="cbx"
                            :for="'cbx' + task.id"
                            @click="doneTask($event, task)"
                        >
                            <span>
                                <svg width="12px" height="10px" viewbox="0 0 12 10">
                                    <polyline points="1.5 6 4.5 9 10.5 1"></polyline>
                                </svg>
                            </span>
                            <span>{{ task.title }}</span>
                        </label>
                    </div>
                </div>
            </div>
            <div
                v-else
            >
                <span>No task!</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Todo',
    components: {},
    props: {
        todo: {
            type: Array,
            default: [],
        },
        done: {
            type: Array,
            default: [],
        },
        data: {
            type: Array,
            default: [],
        },
    },
    data() {
        return {}
    },

    watch: {
        data () {
            console.log(123);
            
            this.$parent.filterData();
        }
    },

    created () {
    },

    methods: {
        doneTask (event, task) {
            let index = this.data.findIndex((item) => {
                return item.id == task.id;
            });

            task.status = 1;
            this.data[index].status = 1;
            this.$parent.filterData();
            this.$parent.updateDataToFireBase(task);

            return;
        },
    },
}
</script>

<style>
    .todo-item {
        transition: .5s;
        /* animation-delay: .5s; */
        transition: ease-in-out;
    }

    .item.todo-item.remove-ele {
        animation: remove 2.5s ease-in-out forwards;
        animation-duration: 2.5s;
    }

    @keyframes remove {
        from {
            position: relative;
            left: 0;
            display: block;
        } to {
            left: 400px;
            opacity: 0;
            display: none;
        }
    }
</style>