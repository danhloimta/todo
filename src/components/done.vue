<template>
    <div class="col-sm-6">
        <div class="list-done">
            <div
                v-if="done.length"
            >
                <div 
                    class="item"
                    v-for="(task, index) in done"
                    :key="index"
                >
                    <div class="form-check form-check-inline">
                        <input class="inp-cbx d-none" :id="'cbx' + task.id" type="checkbox" checked/>
                        <label class="cbx" :for="'cbx' + task.id">
                            <span>
                                <svg width="12px" height="10px" viewbox="0 0 12 10">
                                    <polyline points="1.5 6 4.5 9 10.5 1"></polyline>
                                </svg>
                            </span>
                            <span class="done">{{ task.title }}</span>
                        </label>
                    </div>
                </div>
            </div>
            <div
                v-else
            >
                <span>No task done!</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Done',
    props: {
        done: {
            type: Array,
            default: [],
        }
    },
    data() {
        return {}
    },
}
</script>

<style>
    .done {
        text-decoration: line-through;
    }
</style>