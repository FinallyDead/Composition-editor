<template>
<div>
<network class="wrapper" ref="network"
      :nodes="nodes"
      :edges="edges"
      :options="options"
      >
    </network>
<br/><button @click="addNewTask">Add new Tasks.</button><br/>
<br/><button @click="deleteNode">Delete selected node.</button><br/>
<br/><button @click="createBond">Create a bond.</button><br/>
<p><button @click="editNode" class="editSelectedNode">Edit selected node.</button></p>
<div class="Form" ref="Form">
<form>
<br/>
<label>id: </label>
<input value="" ref="idInp" class="inputs">
<p>
    <label>type: </label>
    <input value="" ref="typeInp" class="inputs">
</p>
<p>
    <label>service: </label>
    <input value="" ref="serviceInp" class="inputs">
</p>
<p>
    <label>callback: </label>
    <input value="" ref="callbackInp" class="inputs">
</p>
<p>
    <label>inputs:</label><br/><br/>
        <label>name: </label>
        <input value="" ref="inputsNameInp" class="inputs"><br/><br/>
        <label>value: </label>
        <input value="" ref="inputsValueInp" class="inputs"><br/><br/>
        <label>type: </label>
        <input value="" ref="inputsTypeInp" class="inputs">
</p>
<p>
    <label>outputs:</label><br/><br/>
        <label>name: </label>
        <input value="" ref="outputsNameInp" class="inputs"><br/><br/>
        <label>value: </label>
        <input value="" ref="outputsValueInp" class="inputs"><br/><br/>
        <label>type: </label>
        <input value="" ref="outputsTypeInp" class="inputs">
</p>
<br/>
</form>
</div>
<p><button @click="confirmChanges" class="cnfChg" ref="cnfChg">Confirm changes.</button></p>
</div>
</template>

<script>
export default{
 data: () => ({
      nodes: [

      ],
      edges: [

      ],
      options: {
         nodes: {
          borderWidth: 4
         },
         edges: {
          color: '#9CEE90'
        }
      },
      currentIndex: 0,
      previewIndex: 0,
      editingIndex: 0,
      oldValue: 0,
      selected: false
  }),
  methods: {
    deleteNode(){
         this.nodes.splice(this.currentIndex, 1);
        },
    createBond(){
        this.edges.push({from:this.nodes[this.previewIndex].id, to:this.nodes[this.currentIndex].id});
    },
    editNode(){
    if (this.selected){
        this.editingIndex=this.currentIndex;
        this.$refs.cnfChg.style.display="block";
        this.$refs.Form.style.display="block";
        this.$refs.idInp.value=this.nodes[this.editingIndex].id;
        this.$refs.typeInp.value=this.nodes[this.editingIndex].type;
        this.$refs.serviceInp.value=this.nodes[this.editingIndex].service;
        this.$refs.callbackInp.value=this.nodes[this.editingIndex].callback;
        this.$refs.inputsNameInp.value=this.nodes[this.editingIndex].inputs[0].name;
            var editStrEdgBond = this.nodes[this.editingIndex].inputs[0].value.slice(5);
                 var editPos = 0;
                 editPos = editStrEdgBond.indexOf("::");
                 editStrEdgBond = editStrEdgBond.slice(0,editPos);
        this.$refs.inputsValueInp.value=editStrEdgBond;
        this.oldValue=editStrEdgBond;
        this.$refs.inputsTypeInp.value=this.nodes[this.editingIndex].inputs[0].type;
        this.$refs.outputsNameInp.value=this.nodes[this.editingIndex].outputs[0].name;
        this.$refs.outputsValueInp.value=this.nodes[this.editingIndex].outputs[0].value;
        this.$refs.outputsTypeInp.value=this.nodes[this.editingIndex].outputs[0].type;
        }
    },
    confirmChanges(){
        this.$refs.cnfChg.style.display="none";
        this.$refs.Form.style.display="none";
        var strEdgBond = 0;
        strEdgBond = this.$refs.inputsValueInp.value;
        this.nodes[this.editingIndex].inputs[0].value="ref::"+strEdgBond+"::out";
        for (var i=0;i<this.edges.length;i++){
            if (this.edges[i].from==this.nodes[this.editingIndex].id && this.edges[i].to==this.oldValue){
                this.edges.splice(i,1);
            }
        }
        this.edges.push({from: this.nodes[this.editingIndex].id, to:strEdgBond});
        this.nodes[this.editingIndex].type=this.$refs.typeInp.value;
        this.nodes[this.editingIndex].service=this.$refs.serviceInp.value;
        this.nodes[this.editingIndex].callback=this.$refs.callbackInp.value;
        this.nodes[this.editingIndex].inputs[0].name=this.$refs.inputsNameInp.value;
        this.nodes[this.editingIndex].inputs[0].type=this.$refs.inputsTypeInp.value;
        this.nodes[this.editingIndex].outputs[0].name=this.$refs.outputsNameInp.value;
        this.nodes[this.editingIndex].outputs[0].value=this.$refs.outputsValueInp.value;
        this.nodes[this.editingIndex].outputs[0].type=this.$refs.outputsTypeInp.value;
    },
    addNewTask(){
       this.nodes.splice(0,this.nodes.length);
       this.edges.splice(0,this.edges.length);
       var Tasks = [
                {id:"0",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                {id:"1",  type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                {id:"2",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                {id:"3",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]}
              ];
       for (let i=0;i<Tasks.length;i++){
            this.nodes.push(Tasks[i]);
            this.nodes[i].label="Task "+(i+1);
       }
       for (let i=0;i<this.nodes.length;i++){
            var strEdgBond = " ";
            if (this.nodes[i].inputs[0].value.indexOf("ref::")==0){
                    strEdgBond = this.nodes[i].inputs[0].value.slice(5);
                    var pos = 0;
                    pos = strEdgBond.indexOf("::");
                    strEdgBond = strEdgBond.slice(0,pos);
                    this.edges.push({from: this.nodes[i].id, to:strEdgBond});
            }
       }
       for (let i=0;i<this.nodes.length;i++){
                       var currentId=this.nodes[i].id;
                       if (i!=0){
                           this.nodes[i].id=Math.ceil(Math.random() * (2500 - 1) + 1);
                       }
                       else{
                           this.nodes[0].id=Math.ceil(Math.random() * (2500 - 1) + 1);
                       }
                       var newId=this.nodes[i].id;
                       for (let j=0;j<this.edges.length;j++){
                           if (this.edges[j].from==currentId){
                               this.edges[j].from=newId;
                           }
                           if (this.edges[j].to==currentId){
                               this.edges[j].to=newId;
                           }
                       }
                       for (let f=0;f<this.nodes.length;f++){
                           if (this.nodes[f].inputs[0].value!=""){
                               var strEdgBond2 = this.nodes[f].inputs[0].value.slice(5);
                               var pos2 = 0;
                               pos2 = strEdgBond2.indexOf("::");
                               strEdgBond2 = strEdgBond2.slice(0,pos2);
                               if (strEdgBond2==currentId){
                                     strEdgBond2=newId;
                               }
                               this.nodes[f].inputs[0].value="ref::"+strEdgBond2+"::out";
                            }
                       }
                  }
    }
  },
  mounted(){
  var vm = this;
  function onLoadCallback(callback){
  callback();
  }
  onLoadCallback(function(){
  var Tasks = [
                    {id:"0",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                    {id:"1",  type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                    {id:"2",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                    {id:"3",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]}
                  ];
           for (let i=0;i<Tasks.length;i++){
                vm.nodes.push(Tasks[i]);
                vm.nodes[i].label="Task "+(i+1);
           }
           for (let i=0;i<vm.nodes.length;i++){
                var strEdgBond = " ";
                if (vm.nodes[i].inputs[0].value.indexOf("ref::")==0){
                        strEdgBond = vm.nodes[i].inputs[0].value.slice(5);
                        var pos = 0;
                        pos = strEdgBond.indexOf("::");
                        strEdgBond = strEdgBond.slice(0,pos);
                        vm.edges.push({from: vm.nodes[i].id, to:strEdgBond});
                }
           }
           for (let i=0;i<vm.nodes.length;i++){
                var currentId=vm.nodes[i].id;
                if (i!=0){
                    vm.nodes[i].id=Math.ceil(Math.random() * (2500 - 1) + 1);
                }
                else{
                    vm.nodes[0].id=Math.ceil(Math.random() * (2500 - 1) + 1);
                }
                var newId=vm.nodes[i].id;
                for (let j=0;j<vm.edges.length;j++){
                    if (vm.edges[j].from==currentId){
                        vm.edges[j].from=newId;
                    }
                    if (vm.edges[j].to==currentId){
                        vm.edges[j].to=newId;
                    }
                }
                for (let f=0;f<vm.nodes.length;f++){
                    if (vm.nodes[f].inputs[0].value!=""){
                        var strEdgBond2 = vm.nodes[f].inputs[0].value.slice(5);
                        var pos2 = 0;
                        pos2 = strEdgBond2.indexOf("::");
                        strEdgBond2 = strEdgBond2.slice(0,pos2);
                        if (strEdgBond2==currentId){
                            strEdgBond2=newId;
                        }
                        vm.nodes[f].inputs[0].value="ref::"+strEdgBond2+"::out";
                        }
                }
           }
  });
  document.onclick = function(e){
      if ( e.target.className == 'editSelectedNode') {
          vm.selected=false;
      }
  }
  this.$refs.network.network.on('click', function (properties){
    vm.selected=true;
    vm.previewIndex=vm.currentIndex;
    for (let i=0; i<vm.nodes.length; i++){
            if (properties.nodes[0]==vm.nodes[i].id){
                vm.currentIndex=i;
            }
        }
    console.log(properties);
    });
   }
  }
</script>

<style>
.wrapper{
  min-height: 75vh;
  min-width: 120vh;
  border: 1px solid black;
  background-color: #565656;
  padding: 10px;
  height: 75vh;
  width: auto;
}
.Form{
display: none;
margin-left: auto;
margin-right: auto;
background-color: #424242;
color: white;
}
.inputs{
background-color: #576981;
color: white;
}
.cnfChg{
display: none;
margin-left: auto;
margin-right: auto;
}
</style>