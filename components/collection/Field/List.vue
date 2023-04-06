<template>
  <!-- start of showing custom in the table -->
    <div class="overflow-auto">
       <table class="shadow-2xl border-2 border-gray-800 w-full">
        <thead class="bg-gray-800 text-white text-left">
            <tr>
                <th class="py-3">Name</th>
                <th class="py-3">Type</th>
                <th class="py-3">Description</th>
                <th class="py-3">placeholder</th>
                <th class="py-3">Edit Action</th>
                <th class="py-3">Delete Action</th>
            </tr>
        </thead>
        <tbody class="divide-y  divide-gray-800">
            <tr v-for="(field,index) in customFieldUrlData" :key="field" class="text-left cursor-pointer">
              <td class="py-3 pr-6 whitespace-nowrap">
                <textarea v-if="editFieldIndex===index" v-model="editFieldForm.name" class="focus:border-purple-500" cols="10" rows="1">{{field.name}}</textarea>
                <p v-else>{{field.name}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <textarea v-if="editFieldIndex===index" v-model="editFieldForm.type" class="focus:border-purple-500" cols="10" rows="1">{{field.type}}</textarea>
                <p v-else>{{field.type}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <textarea v-if="editFieldIndex===index" v-model="editFieldForm.description" class="focus:border-purple-500" cols="10" rows="1">{{field.type_data.description}}</textarea>
                <p v-else>{{field.type_data.description}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <textarea v-if="editFieldIndex===index" v-model="editFieldForm.placeholder" class="focus:border-purple-500" cols="10" rows="1">{{field.type_data.placeholder}}</textarea>
                <p v-else>{{field.type_data.placeholder}}</p>
              </td>         
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionFieldEdit @edit="editField(field,index)"/>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionFieldDelete @delete="deleteField(field)"/>
              </td>
            </tr>
        </tbody>
       </table>
          
    </div>
     <!-- End of showing custom in the table -->
</template>
<script setup lang="ts">
//Defining the schema of Fieldschema
interface FieldSchema{
  project_id:string,
  name:string,
  entity:string,
  type_data:object,
  type:string,
  role_id:string,
  uid:string
}
// Getting the props of customFieldurlData
let props = defineProps<{ customFieldUrlData:FieldSchema }>()

const editFieldIndex=ref("0")
//Declaring in the empty form and use it in v-model
const editFieldForm=reactive({
  name:'',
  type:'',
  description:'',
  placeholder:''

})
const emits = defineEmits(['edit','delete']);
//Emitting the delete and pass it in the main
const deleteField=(field:any)=>{
    emits('delete',field)
  
}
//Emitting the edit and pass it in the main
const editField=(field:any,index:any)=>{
  editFieldIndex.value=index
  emits('edit',field,editFieldForm)
}
</script>