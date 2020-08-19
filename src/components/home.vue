<template>
  <div class="container" id="home">
    <div class="row">
       <div class="col-md">
   <h1> TODOLIST </h1>
    </div>
    </div>
    <br>
  <div class="row">
    <div class="col-md">
        <form class="form-inline"> 
          <input  class="form-control" v-model="list">
    <button type="button" class="btn btn-primary" @click="list_match"  style="margin-right:5px">ADD</button>
        </form>
    </div>
  </div>
  <br>
  <div class="row">
     <div class="col-md">
        <div v-for="(data , index) in list_name" :key="data">
         <input type="checkbox" v-model="data.value" @click="check(index)">
           {{ data.name }}
      </div>
     </div>
  </div>
  <br>
  <div class="row">
        <div class="col-md-1">
            <button type="button" class="btn btn-success" v-b-modal.modal-1 @click="edit">Edit</button>
        </div>
         <div class="col-md-1">
            <button type="button" class="btn btn-danger" v-b-modal.modal-2 @click="Delete_data">Delete</button>
        </div>    


  </div>

<div>
  <b-modal id="modal-1" title="Edit">
    <div v-for="(data) in list_edit" :key="data">
      <input  class="form-control" v-model="data.name">
      <br>
    </div>
     <template v-slot:modal-footer="{cancel }">
      <b-button variant="primary" @click="edit_data">
        Submit
      </b-button> 
      <b-button variant="secondary" @click="cancel()">
        Cancel
      </b-button>
    </template>
  </b-modal>
</div>

<div>
  <b-modal id="modal-2" title="Delete">
     <div v-for="(data , index) in list_name" :key="data">
         <input type="checkbox"  @click="check_delete(index)">
           {{ data.name }}
      </div>
     <template v-slot:modal-footer="{cancel}">
      <b-button variant="danger" @click="Delete_list">
        Delete
      </b-button> 
      <b-button variant="secondary" @click="cancel()">
        Cancel
      </b-button>
    </template>
  </b-modal>
</div>

  </div>
</template>  

<script>

export default {
    name : "home",
    data(){
      return{
        list : "",
        list_name: [],
        list_edit: [],
        list_delete: []
        }
    },
    methods:{
      list_match: function(){ 
        if(this.list == ""){
          alert("Error")
        }
        else{
        //this.$cookies.remove("cookies_list")
        if(this.$cookies.get("cookies_list") == undefined){
          this.list_name = [{name : this.list , value : false}]
          this.$cookies.set("cookies_list",JSON.stringify(this.list_name))
         // console.log(JSON.parse(this.$cookies.get("cookies_list")))
        } 
        else{
          this.list_name = JSON.parse(this.$cookies.get("cookies_list"))
           var add_list = {name : this.list , value : false }
           this.list_name[this.list_name.length] = add_list
           this.$cookies.set("cookies_list",JSON.stringify(this.list_name))
        }
        this.list = ""
      }
      },
      check : function(index){
        if(this.list_name[index].value == false){
          this.list_name[index].value = true
        }
        else{
          this.list_name[index].value = false
        }
         this.$cookies.set("cookies_list",JSON.stringify(this.list_name))
      },
      Get_Start: function(){
        if(this.$cookies.get("cookies_list") != undefined){
        this.list_name = JSON.parse(this.$cookies.get("cookies_list"))
        }
        console.log("Get Start")
      },

      edit : function(){
        this.list_edit = JSON.parse(this.$cookies.get("cookies_list"))
      },
      edit_data : function(){
        this.$cookies.set("cookies_list",JSON.stringify(this.list_edit))
        this.list_name = this.list_edit
        this.$root.$emit('bv::hide::modal', 'modal-1', '#btnShow')  
      },
      Delete_data : function(){
        this.list_delete = []
      },
      check_delete : function(index){
        var check_dupi = false
        for(var i = 0 ; i <= this.list_delete.length ; i++){
          if(index == this.list_delete[i]){
            check_dupi = true 
            this.list_delete.splice(i,1)
          }
        }
        if(check_dupi == false){
        this.list_delete.push(index)
        }
      },
      Delete_list : function(){
        this.list_delete.sort(function(a, b){return b - a});
        for(var j = 0 ; j < this.list_delete.length ; j++){
          this.list_name.splice( this.list_delete[j],1)
        }
         this.$cookies.set("cookies_list",JSON.stringify(this.list_name))
         this.$root.$emit('bv::hide::modal', 'modal-2', '#btnShow')
      }
    },
     beforeMount(){
    this.Get_Start()
 },
}
</script>
