<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Http</h1>
                <hr>
                <div class="form-group">
                    <label>Username</label>
                    <input type="text" class="form-control" v-model="user.username">
                </div>
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" class="form-control" v-model="user.email">
                </div>
                <label>Select a Node to Submit</label>
                <select class="form-control" v-model="postNode">
                    <option v-for="n in nodes">{{ n }}</option>
                </select>
                <br>
                <button class="btn btn-primary" @click="submit">Submit</button>
                <hr>
                <label>Select a Node to Retrieve</label>
                <select class="form-control" v-model="getNode">
                    <option v-for="n in nodes">{{ n }}</option>
                </select>
                <br>
                <button class="btn btn-primary" @click="fetchData">Get Data</button>
                <br><br>
                <ul class="list-group">
                    <li class="list-group-item" v-for="u in users">{{ u.username }} - {{ u.email }}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                user: {
                    username: '',
                    email: ''
                },
                users: [],
                resource: {},
                nodes: ['data', 'alternative'],
                postNode: 'data',
                getNode:'data'
            };
        },
        methods: {
            submit() {
                // this.$http.post('data.json', this.user)
                //         .then(response => {
                //             console.log(response);
                //         }, error => {
                //             console.log(error);
                //         });
                //this.resource.save({}, this.user);
                if (this.postNode == 'data') {
                    this.resource.saveData(this.user);
                }
                
                if (this.postNode == 'alternative') {
                    this.resource.saveAlt(this.user);
                }
            },
            fetchData() {
                // this.$http.get('data.json')
                //         .then(response => {
                //             return response.json();
                //         })
                //         .then(data => {
                //             const resultArray = [];
                //             for (let key in data) {
                //                 resultArray.push(data[key]);
                //             }
                //             this.users = resultArray;
                //         });

                this.resource.getData({node: this.getNode})
                        .then(response => {
                            return response.json();
                        })
                        .then(data => {
                            const resultArray = [];
                            for (let key in data) {
                                resultArray.push(data[key]);
                            }
                            this.users = resultArray;
                        });
            }
        },
        created() {
            const customActions = {
                saveData: {method: 'POST', url: 'data.json'},
                saveAlt: {method: 'POST', url: 'alternative.json'},
                getData: {method: 'GET'}
            };
            this.resource = this.$resource('{node}.json', {}, customActions);
        }
    }
</script>

<style>
</style>
