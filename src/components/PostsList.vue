
<template> 
<div>   
    
<!--**************************************** Overflow scrolling **************************************************-->
    <div id="pepe" class="relative shadow-md sm:rounded-lg flex justify-start hover:justify-between mt-10 ml-2 mr-4 ">
    <div class="pepe1 overflow-scroll fixed left-0 mx-8 mb-10 ">           
    <table id="tabla1" class="table-fixed border-collapse border border-separate border-slate-400 ">
        <thead class="sticky top-0 text-xs text-gray-700 uppercase bg-gray-300 dark:bg-black-700 dark:text-gray-400 ">
            <tr >                
                <th scope="col" class="py-3 px-6 border border-slate-300 w-0.5">ID</th>
                <th scope="col" class="py-3 px-6 border border-slate-300 text-center "> FROM</th>
                <th scope="col" class="py-3 px-6 border border-slate-300 text-center "> TO</th>
                <th scope="col" class="py-3 px-6 border border-slate-300 text-center "> DATE</th>
                <th scope="col" class="py-3 px-6 border border-slate-300 text-center ">SUBJECT</th>
                <!-- <th scope="col" class="py-3 px-6 border border-slate-300">BODY</th>                 -->
            </tr>
        </thead>
        <tbody>
            <tr v-for="correo in pageOfItems" :key="correo.ID" v-on:click="select($event,correos)" v-bind:id="correo.ID" class="emails border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">               
                <td class="py-4 px-6 text-sm font-medium  border border-slate-300 ">{{ correo.ID }}</td>
                <td class="py-4 px-6 text-sm font-medium  border border-slate-300 text-ellipsis overflow-hidden">{{ correo.from }}</td>
                <td class="py-4 px-6 text-sm font-small   border border-slate-300 text-ellipsis overflow-hidden">{{ correo.to }}</td>
                <td class="py-4 px-6 text-sm font-medium  border border-slate-300 ">{{ correo.Date }}</td>
                <td class="py-4 px-6 text-sm font-medium  border border-slate-300 text-ellipsis overflow-hidden">{{ correo.subject }}</td>
                <!-- <td class="py-4 px-6 text-sm font-medium text-center border border-slate-300">{{ correo.Body }}</td>               -->
            </tr>            
        </tbody>         
    </table>    
    
  </div>

  
 
  <div class="pepe2 fixed right-0 overflow-scroll mx-8 mb-10 ">
    <table id="tabla2" class="table-fixed border-collapse border border-separate border-slate-400">
        <thead class="sticky top-0 text-xs text-gray-700 uppercase bg-gray-300 dark:bg-gray-700 dark:text-gray-400 ">
            <tr >                                
                <th class="py-6 px-6 border border-slate-300 text-center "><h3 class="tTO text-left"></h3><h3 class="tFrom text-left"></h3><h3 class="tDate text-left"></h3><h3 class="tSubject text-left"></h3></th>                  
                              
            </tr>
        </thead>
        <tbody>
            <tr class="border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">                               
                <td class="text-justify py-4 px-6 text-sm font-medium text-center border border-slate-300"><p class=" tBody text-ellipsis overflow-hidden"></p></td>              
            </tr>            
        </tbody>
    </table>
  </div>
  
      
</div>


<!-- <div class="center">
      <vs-table v-model="selected"  @selected="handleSelected" @dblSelection="doubleSelection" :data="correos">
        <template slot="header"> <h3>Enron Emails</h3></template>
        <template slot="thead">
          <vs-tr>
            <vs-th>ID</vs-th>
            <vs-th>FROM</vs-th>
            <vs-th>TO</vs-th>
            <vs-th>DATE</vs-th>
            <vs-th>SUBJECT</vs-th>
          </vs-tr>
        </template>
        <template slot-scope="{data}">
          <vs-tr :data="tr" :key="indextr" v-for="(tr,indextr) in data">
            <vs-td :data="data[indextr].ID">{{ data[indextr].ID }}</vs-td>
            <vs-td :data="data[indextr].from">{{ data[indextr].from }}</vs-td>
            <vs-td :data="data[indextr].to">{{ data[indextr].to }}</vs-td>
            <vs-td :data="data[indextr].Date">{{ data[indextr].Date }}</vs-td>
            <vs-td :data="data[indextr].subject">{{ data[indextr].subject }}</vs-td>
          </vs-tr>
        </template>
      </vs-table>

    </div> -->


    <!-- <div class="center">
      <vs-table v-model="selected">
        <template #thead>
          <vs-tr>
            <vs-th>
              Name
            </vs-th>
            <vs-th>
              Email
            </vs-th>
            <vs-th>
              Id
            </vs-th>
          </vs-tr>
        </template>
        <template #tbody>
          <vs-tr
            :key="i"
            v-for="(tr, i) in correos"
            :data="tr"
            :is-selected="selected == tr"
          >
            <vs-td>
              {{ tr.ID }}
            </vs-td>
            <vs-td>
            {{ tr.Date }}
            </vs-td>
            <vs-td>
            {{ tr.subject }}
            </vs-td>
          </vs-tr>
        </template>
      </vs-table>
      </div> -->

     <!-- Paginación -->
      <div class="fixed bottom-3 left-12">        
              <jw-pagination :pageSize=10 :items="correos" @changePage="onChangePage" ></jw-pagination>       
      </div>

     
</div>
</template>

<script>

export default {
  name: "PostsList",
   data(){
    return {
      correos:[], 
      pageOfItems: [],          
    }
   },
  
  created(){
    fetch("http://localhost:3000/Enron-email")
    .then((response) => response.json())
    //.then(json => console.log(json))
    .then((data) => (this.correos = data))
  },

methods:{
  onChangePage(pageOfItems){
      this.pageOfItems = pageOfItems
    },
  select(e, correos){            
        let id = e.currentTarget.id 
        let flag = false
        correos.forEach(email => {
          if(flag){
            return
          }
          if(email.ID == id){            
            this.$el.querySelector(".tBody").innerHTML = email.Body
            this.$el.querySelector(".tTO").innerHTML = "TO : " + email.to
            this.$el.querySelector(".tFrom").innerHTML = "FROM : " + email.from
            this.$el.querySelector(".tDate").innerHTML = "DATE : " + email.Date
            this.$el.querySelector(".tSubject").innerHTML = "SUBJECT : " + email.subject
            flag = true
            console.log("Finalizó el forEach")
            return
          }
        })
             
        },
    // handleSelected(tr) {
    //   // this.$vs.notify({
    //   //   title:`Selected ${tr.username}`,
    //   //   text:`Email: ${tr.email}`
    //   // })
    //   console.log(tr)
    // },
    doubleSelection(tr) {
      this.$vs.notify({
        title:`Double Selection ${tr.username}`,
        text:`Email: ${tr.email}`,
        color: 'success'
      })
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
 /* #pepe{
  border: solid 2px blue;
  display: flex;
  margin: 50px;
 } */
 .pepe1{
  /* border: solid 2px green; */
  height: 86%;
  width: 50%;
 }
 .pepe2{
  /* border: solid 2px red; */
  height: 86%;
  width: 45%;
 }
 #tabla1{
  width: 100%;
 }
 #tabla2{
  width: 100%;
 }

</style>
