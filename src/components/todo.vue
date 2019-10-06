<template>
    <div class="col-sm-6">
        <div class="list-todo">
            <div 
                v-if="todo.length"
            >
                <div 
                    class="item"
                    v-for="(item, index) in todo"
                    v-bind:key="index"
                >
                    <div class="form-check form-check-inline">
                        <input class="inp-cbx d-none" :id="'cbx' + index" type="checkbox"/>
                        <label class="cbx" :for="'cbx' + index">
                            <span>
                                <svg width="12px" height="10px" viewbox="0 0 12 10">
                                    <polyline points="1.5 6 4.5 9 10.5 1"></polyline>
                                </svg>
                            </span>
                            <span>{{ item.title }}</span>
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
        }
    },
    data() {
        return {}
    },
}
</script>
