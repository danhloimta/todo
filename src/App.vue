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
                                <form v-on:submit.prevent="saveData()" id="form">
                                    <div class="input-group">
                                        <input type="text"
                                            class="form-control" name="add" id="add" v-model="newItem.title" placeholder="Nội dung cần làm">
                                        <div class="input-group-append">
                                            <button class="input-group-text btn" title="Hủy" @click="showFormAdd">
                                                <i class="fa fa-close text-danger" aria-hidden="true"></i>
                                            </button>
                                        </div>
                                        <div class="input-group-append">
                                            <button class="input-group-text" title="Lưu" type="submit">
                                                <i class="fa fa-check text-primary" aria-hidden="true"></i>
                                            </button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                    <Todo
                        :todo="todo"
                        :data="data"
                        :done="done"
                    />
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

var toastr = require("toastr");

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
            isAdd: false,
            isLoading: true,
            todo: [],
            done: [],
            data: [],
            newItem: {},
            dataFireBase: [],
        }
    },

    created () {
        this.getData();
    },

    watch: {
        data () {
            this.filterData();
        }
    },

    methods: {
        getData () {
            this.isLoading = true;
            this.data = [];
            todosRef.once('value').then((snap) => {
                this.dataFireBase = snap.val();
                for (const key in snap.val()) {
                    this.data.push(snap.val()[key]);
                }
            }).then(() => {
                this.filterData();
            }).then(() => this.isLoading = false);
        },

        showFormAdd () {
            this.isAdd = !this.isAdd;
        },

        saveData () {
            if (!this.newItem.title.length) {
                return toastr.error('Tiều đề không được để trống', 'Error!');
            }
            this.newItem.id = uuidv1();
            this.newItem.status = 0;
            this.data.push(this.newItem);
            todosRef.push(this.newItem);
            this.newItem = {};

            return toastr.success('Thêm thành công', 'Thành công!');
        },

        filterData () {
            this.todo = this.data.filter(item => (item.status == 0));
            this.done = this.data.filter(item => (item.status == 1));
        },

        updateDataToFireBase (task) {
            let indexTask = null;
            let updates = {};
            for (let index in this.dataFireBase) {
                if (this.dataFireBase[index].id == task.id) {
                    indexTask = index;
                }
            };

            updates['todos/' + indexTask] = task;

            return firebase.database().ref().update(updates);
        }
    },
}
</script>

<style>
    @import 'toastr';
</style>
