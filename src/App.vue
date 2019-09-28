<template>
    <div id="app">
        <div class="container">
            <Header/>
        <div id="main">
            <div class="row">
                <div class="col-sm-12">
                    <div
                        class="btn-add pull-right mb-3"
                        v-if="!isAdd"
                    >
                        <button type="button" class="btn btn-outline-primary" @click="showFormAdd">
                            <i class="fa fa-plus" aria-hidden="true"></i>
                        </button>
                    </div>
                    <div
                        class="form-add"
                        v-else
                    >
                        <div class="form-group">
                            <label for="add">Thêm mới</label>
                            <div class="input-group">
                                <input type="text"
                                    class="form-control" name="add" id="add" aria-describedby="helpId" placeholder="Nội dung cần làm">
                                <div class="input-group-append">
                                    <span class="input-group-text btn" title="Hủy" @click="showFormAdd">
                                        <i class="fa fa-close text-danger" aria-hidden="true"></i>
                                    </span>
                                </div>
                                <div class="input-group-append">
                                    <span class="input-group-text btn" title="Lưu">
                                        <i class="fa fa-check text-primary" aria-hidden="true"></i>
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <Todo :todo="todo" />
                <Done :done="done"/>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import Header from './components/header';
import Todo from './components/todo';
import Done from './components/done';
import data from '../data.json';

export default {
    name: 'App',
    components: {
        HelloWorld,
        Header,
        Todo,
        Done
    },
    data() {
        return {
            data: data.todos,
            todo: {},
            done: {},
            isAdd: false,
        }
    },
    created () {
        this.todo = this.data.filter(item => (item.status == 0));
        this.done = this.data.filter(item => (item.status == 1));
    },

    methods: {
        showFormAdd () {
            this.isAdd = !this.isAdd;
        },
    },
}

// const uuidv1 = require('uuid/v1');
// uuidv1();
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /* text-align: center; */
    color: #2c3e50;
}

.form-add .input-group-text {
    cursor: pointer;
}

.form-add {
    transition: .5s;
    animation: ease-in;
}
</style>
