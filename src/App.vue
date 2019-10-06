<template>
    <div id="app">
        <div class="container">
            <Header/>
            <div id="main">
                <div
                    class="row"
                    v-if="!isLoading"
                >
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
                                        class="form-control" name="add" id="add" v-model="newItem.title" placeholder="Nội dung cần làm">
                                    <div class="input-group-append">
                                        <button class="input-group-text btn" title="Hủy" @click="showFormAdd">
                                            <i class="fa fa-close text-danger" aria-hidden="true"></i>
                                        </button>
                                    </div>
                                    <div class="input-group-append">
                                        <button class="input-group-text" title="Lưu" @click="saveData">
                                            <i class="fa fa-check text-primary" aria-hidden="true"></i>
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <Todo :todo="todo" />
                    <Done :done="done"/>
                </div>
                <div
                    v-else
                    class="loading"
                >
                    <img src="/static/spinner.gif" alt="">
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
import firebase from 'firebase';

// Your web app's Firebase configuration
var firebaseConfig = {
    apiKey: "AIzaSyBCv2kErheXWRSca_Al1beDfueWJxSEquQ",
    authDomain: "todolistwithapp.firebaseapp.com",
    databaseURL: "https://todolistwithapp.firebaseio.com",
    projectId: "todolistwithapp",
    storageBucket: "todolistwithapp.appspot.com",
    messagingSenderId: "87023604166",
    appId: "1:87023604166:web:6f53beedd41b56ecefa2c7",
    measurementId: "G-MZVTXB51S7"
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);
firebase.analytics();
let db = firebase.database();
let todosRef = db.ref('todos');

const uuidv1 = require('uuid/v1');

export default {
    name: 'App',
    components: {
        HelloWorld,
        Header,
        Todo,
        Done
    },

    data () {
        return {
            todo: [],
            done: [],
            isAdd: false,
            title: null,
            data: [],
            newItem: {
                id: '',
                title: '',
                content: '',
                status: 0
            },
            isLoading: true,
        }
    },

    created () {
        this.getData();
    },

    watch: {
        data () {
            // this.todo = this.data.filter(item => (item.status == 0));
            // this.done = this.data.filter(item => (item.status == 1));
        }
    },

    methods: {
        getData () {
            this.isLoading = true;
            todosRef.once('value').then((snap) => {
                for (const key in snap.val()) {
                    this.data.push(snap.val()[key]);
                }
            }).then(() => {
                this.todo = this.data.filter(item => (item.status == 0));
                this.done = this.data.filter(item => (item.status == 1));
            }).then(() => this.isLoading = false);
        },

        showFormAdd () {
            this.isAdd = !this.isAdd;
        },

        saveData () {
            this.newItem.id = uuidv1();
            this.todo.push(this.newItem);
            todosRef.push(this.newItem);
        }
    },
}

// uuidv1();
</script>

<style>

</style>
