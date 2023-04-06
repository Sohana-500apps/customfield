<template>
    <!-- start of custom field tag -->
    <CollectionFieldMain :customFieldUrl="customFieldUrl" :url="url"/>
     <!-- end of custom field tag -->
</template>
<script setup lang="ts">
//importing the customField url and url
const customFieldUrl="https://v1-orm-lib.mars.hipso.cc/custom-fields/CONTACTS?offset=0&limit=100&sort_column=id&sort_direction=desc"
const url="https://v1-orm-lib.mars.hipso.cc/custom-fields"
</script>