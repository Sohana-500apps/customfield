<template>
    <div>
        <!-- start of custom Field -->
       <div class="p-[50px] flex divide-y-2 border-y">
           
        <h1>Fields</h1>
        <!-- start of add button -->
        <div class="ml-[56rem]">
            <button class="flex bg-slate-100" @click="openField">
            <PlusIcon class="h-6 w-6"/>
            <p class="ml-3">Add Custom Field</p>
            </button>
        </div>
        <!-- start of end button -->
        <!-- opening the custom field modal by clicking add custom -->
        <CollectionFieldAdd v-show="isAdd" @cancel="isAdd=false" @save="saveField"/>
         
      </div>
      <div class="text-center" v-if="!isAdd">
        <h1 class="text-3xl">Custom Field List</h1>
         <div>
            <CollectionFieldList :customFieldUrlData="customFieldUrlData" @delete="deleteFieldListener" @edit="editFieldListener"/>
        </div>
      </div>
      <!-- End of custom Field -->
    </div>
</template>
<script setup lang="ts">
//importing the HeroIcon and  useAuthLazyFetch
import { PlusIcon } from "@heroicons/vue/24/outline"
import {useAuthLazyFetch}  from "../../../composables/useAuthLazyFetch"
//Define the schema of FieldUrl
interface FieldUrl{
    customFieldUrl:string,
    url:string
}
// Get the props of customfieldurl and url
const props = withDefaults(defineProps<FieldUrl>(), {
    customFieldUrl: "",
    url:""
})


const isAdd=ref(false)

////opening the  Add model
const openField=()=>{
    isAdd.value=!isAdd.value
}

//Showing the dynamic custom field in the table
const getCustomFieldUrl = async () => {
    try {
    const { data: customFieldData } = await useAuthLazyFetch(props.customFieldUrl, {});
    return customFieldData;
  } catch (error) {
    console.error("Error fetching tag URL", error);
    return null;
  }
}
const customFieldUrlData = await getCustomFieldUrl();

//Deleting the custom field
const deleteFieldListener=(field)=>{

if(confirm('Are you sure to delete this record?')){
    useAuthLazyFetchDelete(`${props.url}/${field.uid}`, {});
    let fieldurlIndex=customFieldUrlData.value.findIndex(item => item.uid === field.uid)
    console.log("fieldurl",fieldurlIndex)
    if(fieldurlIndex!==-1){
        customFieldUrlData.value.splice(fieldurlIndex,1)
    }
}
}

//saving the custom Field

const saveField=(addForm)=>{
useAuthLazyFetchPost(`${props.url}/`, {
    body: {
      project_id:'1',
      name:addForm.name,
      entity:'CONTACTS',
      type_data:{
        is_required:1,
        show_in_filter:1,
        description:addForm.description,
      placeholder:addForm.placeholder,
      },
      type:addForm.type,
      role_id:'1'
    }
  });
}

//Editing the custom Field

const editFieldListener=(field,editFieldForm)=>{
 useAuthLazyFetchPut(`${props.url}/${field.uid}?name=${editFieldForm.name}?description=${editFieldForm.description}?placeholder=${editFieldForm.placeholder}?type=${editFieldForm.type}`, {
    body: {
      name:editFieldForm.name,
      entity:'CONTACTS',
      type_data:{
        is_required:1,
        show_in_filter:1,
        description:editFieldForm.description,
      placeholder:editFieldForm.placeholder,
      },
      type:editFieldForm.type,
      
    },
  });
}
</script>