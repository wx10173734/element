<template>
  <div>
    <Table/>
   <!--<Form></Form>-->
  </div>
</template>

<script>
  import Table from './views/table'
import Form from './views/form'
export default {
  name: 'App',
  components:{
    // Form
    Table
  },
  data(){
    return{

    }
  }
}
</script>

<style>

</style>
