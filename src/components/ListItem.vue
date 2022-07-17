<template>
<div  class="listitem" aria-labelledby="dropdownMenuLink">
    <li :style="{'margin-left':`${addMargin}px`}">{{iterateObject.text}}
       <font-awesome-icon v-if="hasChildren" @click="setdropdown=!setdropdown" icon="fa-solid fa-angle-down" /> 
    </li>
    <hr />
    <div v-if="showTheChildren">
        <list-item v-for="(child,index) in iterateObject.childItems" 
        :key="index"
        :search-string="searchString"
        :add-margin="iterateObject.childItems?margin+10:0"
         :iterate-object="child"/>
    </div>
    
</div>


</template>
<script>
export default {
    name:'ListItem',
    props:{
        iterateObject:{
            type:Object,
            required:true,
        },
        addMargin:{
            type:Number,
            default:0,
        },
        searchString:{
            type:String
        }
    },
    computed:{
        hasChildren(){
return this.iterateObject.childItems&&this.iterateObject.childItems.length
        },
    showTheChildren(){
     return this.iterateObject.childItems && (this.searchString.length||this.setdropdown);
    }
    },
    data(){
        return{
          margin:this.addMargin,
          setdropdown:false,
        }
    }
}
</script>
<style scoped>
hr {
  margin:0;
  /* border:0; */
}
li{
 
  margin:2px;
  outline: auto;
  outline-color: #6c757d;
  text-align: start;
}
.listitem{
/* outline: auto; */
}
</style>