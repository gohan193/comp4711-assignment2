<template>
    <b-row>
        <b-col cols="12">
        
		<b-link href="sign-in">Sign-In</b-link>&nbsp;&nbsp;
		<b-link href="sign-in">Exercise Library</b-link>
		
		<h2>
                My Routines
                <b-link href="#/add-board">(New Routine)</b-link>
            </h2>
            <b-table striped hover :items="boards" :fields="fields">
                <template slot="actions" scope="row">
                    <b-btn size="sm" @click.stop="details(row.item)">Details</b-btn>
                </template>
            </b-table>
        </b-col>
    </b-row>
</template>

<script>

    import firebase from '../Firebase'
    import router from '../router'

    export default {
        name: 'RoutineList',
        data () {
            return {
                fields: {
                    title: { label: 'Title', sortable: true, 'class': 'text-left' },
                    actions: { label: 'Action', 'class': 'text-center' }
                },
                boards: [],
                errors: [],
                ref: firebase.firestore().collection('boards'),
            }
        },
        created () {
            this.ref.onSnapshot((querySnapshot) => {
                this.boards = [];
                querySnapshot.forEach((doc) => {
                    this.boards.push({
                        key: doc.id,
                        title: doc.data().title
                    });
                });
            });
        },
        methods: {
            details (board) {
                router.push({ name: 'ShowBoard', params: { id: board.key }})
            }
        }
    }
</script>

<style>
    .table {
        width: 96%;
        margin: 0 auto;
    }
</style>