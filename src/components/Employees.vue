<template>
  <div id="employee" class="row">
    <notifications group="foo"/>
    <div class="student-list col-md-12">
      <div class="col-md-10" style="text-align: left; margin: auto">
        <div class="col-md-6" style="float:left;">
          <h3>DANH SÁCH</h3>
        </div>
        <div class="col-md-3" style="float:left;">
          <button class="btn btn-success" @click="open()" style="float: right">Create Employee</button>
        </div>
      </div>
      <table class="table table-hover table-bordered col-md-10" id="example" style="margin: auto">
        <thead>
        <tr>
          <th width="400">NAME</th>
          <th>WAGE</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="emp in employees" :key="emp.id">
          <td>{{ emp.name }}</td>
          <td>{{ emp.wage }}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
  <vue-modal-2 name="modal-1" @on-close="close"
               :headerOptions="{
                    title: this.isCreate ? 'Create Student': 'Edit Student',
                 }"
               :footerOptions="{
                    btn1Style: {
                      display: 'none'
                    },
                    btn2Style: {
                      display: 'none'
                    },
                 }"
  >
    <form v-on:submit.prevent="create">
      <div class="dialog">
        <div class="form-group">
          <label>Name</label>
          <input class="form-control" v-model="name">
        </div>
        <div class="form-group">
          <label class="label-text">Email</label>
          <input class="form-control" v-model="wage">
        </div>
        <div class="form-group">
          <input @click="close" value="Close" class="btn btn-info col-md-3" style="margin-right: 5px">
          <input value="Create" type="submit" class="btn btn-success col-md-3">
        </div>
      </div>
    </form>
  </vue-modal-2>
</template>

<script>
import axios from "axios";
export default {
  name: "Employees"
}
</script>

<style scoped>

</style>