<template>

    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                  
                    <div class="card-header"> Directory Component </div>

                    <div class="card-body">

                        <div class="form-group">
                            <label> Name </label>
                            <input class="form-control" type="text" v-model="item.name" id="name" placeholder="enter name" >
                        </div>

                        <div class="form-group">
                            <label> Phone </label>
                            <input class="form-control" type="text" v-model="item.tel" id="tel" placeholder="enter phone" >
                        </div>

                        <div class="d-grid" style="margin-top: 5px;">
            <button class="btn btn-success" @click="save">{{ isEditing ? "Update" : "Save" }} </button>
                        </div>

                        <div style="margin-top: 10px;">
                                    <h3 class="text-center" v-if="lists.length > 0">All Numbers</h3>
                                    <ul class="list-group">
                                       
                                        <li
                                            class="list-group-item"
                                            v-for="tel in lists"
                                            :key="tel.id">
                                            <div class="row">
                                                <div class="col-md-8"> {{ tel.name }} - {{ tel.tel }} </div>
                                                <div class="col-md-4"> 
                                                    <button
                                                    class="btn btn-warning btn-sm mr-2"
                                                    @click="editTel(tel)"
                                                    >Edit</button>
                                                    <button
                                                    class="btn btn-danger btn-sm mr-2"
                                                    @click="deleteTel(tel.id)"
                                                    >Delete</button> 
                                                </div>
                                            </div>
                                        </li>
                                   
                                    </ul>
                          </div>
   
             </div> </div> </div> </div> </div> 
</template>


<script>
   
    export default {
        
        name : "Directory",
        
        data() {
            return {
                lists: [],
                item: {
                    name: "",
                    tel : ""
                },
                isEditing: false,
                temp_id: null
            }
        }, // end data

        mounted(){
            this.fetchAll();
        },

        methods : {
          
            fetchAll(){
                try {
                axios.get('/api/tel')
                    .then(res => this.lists = res.data)
                } catch (e) {
                    console.log(e)
                }
            }, // end fetchall

            save() {
                    let method = axios.post
                    let url = "/api/tel"
                    if (this.temp_id) {
                        method = axios.put
                        url = `/api/tel/${this.temp_id}`
                    }
                    try {
                        method(url, this.item)
                            .then(res => {
                                this.fetchAll()
                                this.item = {
                                    name: "",
                                    tel: ""
                                }
                                this.temp_id = null
                                this.isEditing = false
                            })
                    } catch (e) {
                        console.log(e)
                    }
            }, // end save

            editTel(tel) {
                    this.item = {
                        name: tel.name,
                        tel : tel.tel,
                    }
                    this.temp_id = tel.id;
                    this.isEditing = true
            }, // end editTel

            deleteTel(id) {
                    try {
                        axios.delete(`/api/tel/${id}`)
                            .then(res => {
                                this.fetchAll()
                            })
                    } catch (e) {
                        console.log(e)
                    }
            }
        
        } // end methods  
        
    } // end export

</script>


<style scoped>
</style>
