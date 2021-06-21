<template>
  <div id="ListStudent" class="row">
    <notifications group="foo"/>
    <div class="student-list col-md-12">
      <div class="col-md-10" style="text-align: left; margin: auto">
        <div class="col-md-6" style="float:left;">
          <h3>DANH SÁCH</h3>
        </div>
        <div class="col-md-3" style="float:left;">
          <input type="text" v-model="inputSearch">
          <button @click="search" class="btn btn-dark">Search</button>
        </div>
        <div class="col-md-3" style="float:left;">
          <button class="btn btn-success" @click="open(true)" style="float: right">Create Student</button>
        </div>
      </div>
      <table class="table table-hover table-bordered col-md-10" id="example" style="margin: auto">
        <thead>
        <tr>
          <th width="400">Name</th>
          <th>Email</th>
          <th>Status</th>
          <th width="300">Action</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.fullName }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.status }}</td>
          <td>
            <button @click="openDel(user)" style="color: firebrick" class="action" title="Delete"><i
                class="fa fa-trash"></i></button>
            <button @click="open(false, user)" style="color: burlywood" class="action" title="Edit"><i
                class="fa fa-edit"></i>
            </button>
            <button @click="openView(user)" style="color: #4CAF50" class="action" title="View"><i class="fa fa-eye"></i>
            </button>
          </td>
        </tr>
        </tbody>
      </table>
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
            <input class="form-control" v-model="fullName">
          </div>
          <div class="form-group">
            <label class="label-text">Email</label>
            <input class="form-control" v-model="email">
          </div>
          <div class="form-group">
            <input type="button" @click="close" value="Close" class="btn btn-info col-md-3" style="margin-right: 5px">
            <input v-if="this.isCreate" value="Create" type="submit" class="btn btn-success col-md-3">
            <input v-if="!this.isCreate" value="Update" type="submit" class="btn btn-success col-md-3">
          </div>
        </div>
      </form>
    </vue-modal-2>
    <vue-modal-2 name="modal-2" @on-close="closeDel"
                 :headerOptions="{
                    title: 'Delete Student: ' + this.fullName,
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
      <div>
        <input type="button" @click="closeDel" value="Close" class="btn btn-info col-md-3" style="margin-right: 5px">
        <input @click="this.delete" value="Delete" type="submit" class="btn btn-danger col-md-3">
      </div>
    </vue-modal-2>
    <vue-modal-2 name="modal-3" @on-close="closeView"
                 :headerOptions="{
                    title: 'Student infomation',
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
      <div class="dialog" style="text-align: left">
        <div class="form-group">
          <label>Name: {{ this.fullName }}</label>
          <label></label>
        </div>
        <div class="form-group">
          <label>Email: {{ this.email }}</label>
          <label></label>
        </div>
      </div>
      <div>
        <input type="button" @click="closeView" value="Close" class="btn btn-info col-md-3" style="margin-right: 5px">
      </div>
    </vue-modal-2>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "list-student",
  components: {},
  data() {
    return {
      users: [],
      message: 'this is text',
      isConfirmationDialogVisible: false,
      isCreate: false,
      id: '',
      email: "",
      fullName: "",
      createTime: 0,
      status: 1,
      account: {},
      masks: [],
      inputSearch: ""

    }
  },
  created() {
    console.log('created');
    this.getAllStudents();
  },
  computed: {},
  methods: {
    handleAddEdit(type, user) {
      console.log('handleAddEdit');
      if (type) {
        this.isCreate = true;
        this.fullName = '';
        this.email = '';
        this.id = '';
      } else {
        this.isCreate = false;
        this.fullName = user.fullName;
        this.email = user.email;
        this.id = user.id;
        this.createTime = user.createTime;
      }
    },
    open(type, user) {
      console.log('open');
      this.handleAddEdit(type, user);
      this.$vm2.open('modal-1')
    },
    close() {
      this.$vm2.close('modal-1')
    },
    openDel(user) {
      this.fullName = user.fullName;
      this.id = user.id;
      this.$vm2.open('modal-2')
    },
    closeDel() {
      this.$vm2.close('modal-2')
    },
    openView(user) {
      this.fullName = user.fullName;
      this.id = user.id;
      this.email = user.email;
      this.$vm2.open('modal-3')
    },
    closeView() {
      this.$vm2.close('modal-3')
    },
    async getAllStudents() {
      await axios.get('http://localhost:8081/nxp/v1/student/getAll').then(res => {
        // await axios.get('http://192.168.100.50:8082/mfscontroller/mntConfig/getAll').then(res =>{
        console.log(res.data);
        this.users = res.data;
      });
    },
    create() {
      const row = {};
      row.fullName = this.fullName;
      row.email = this.email;
      row.status = this.status;
      row.account = this.account;
      row.masks = this.masks;
      console.log(row);
      console.log(this.isCreate);
      if (this.isCreate) {
        axios.post('http://localhost:8081/nxp/v1/student/create', row)
            .then(res => {
              // this.searchModalVisible = false;
              console.log('data: ' + res.data);
              this.$notify({
                group: 'foo',
                title: 'Alert',
                text: 'Success',
                duration: 3000,
              });
              this.close();
              this.getAllStudents();
            })
            .catch(err => {
              console.log(err.response);
              this.$notify({
                group: 'foo',
                title: 'Alert',
                text: 'Not success',
                duration: 3000,

              });
            });
      } else {
        axios.put('http://localhost:8081/nxp/v1/student/update/' + this.id, row)
            .then(res => {
              // this.searchModalVisible = false;
              console.log('data: ' + res.data);
              this.$notify({
                group: 'foo',
                title: 'Alert',
                text: 'Success',
                duration: 3000,
              });
              this.close();
              this.getAllStudents();
            })
            .catch(err => {
              console.log(err.response);
              this.$notify({
                group: 'foo',
                title: 'Alert',
                text: 'Not success',
                duration: 3000,

              });
            });
      }
    },
    delete() {
      axios.delete('http://localhost:8081/nxp/v1/student/delete/' + this.id)
          .then(res => {
            // this.searchModalVisible = false;
            console.log('data: ' + res.data);
            this.users = res.data;
            console.log('users: ' + this.users);
            this.$notify({
              group: 'foo',
              title: 'Alert',
              text: 'Success',
              duration: 3000,
            });
            this.getAllStudents();
          })
          .catch(err => {
            console.log(err.response);
            this.$notify({
              group: 'foo',
              title: 'Alert',
              text: 'Not success',
              duration: 3000,

            });
          });
    },
    search() {
      axios.get('http://localhost:8081/nxp/v1/student/search/' + this.inputSearch + '/1/1/5')
          .then(res => {
            // this.searchModalVisible = false;
            console.log('data: ' + res.data);
            this.$notify({
              group: 'foo',
              title: 'Alert',
              text: 'Success',
              duration: 3000,
            });
            this.getAllStudents();
          })
          .catch(err => {
            console.log(err.response);
            this.$notify({
              group: 'foo',
              title: 'Alert',
              text: 'Not success',
              duration: 3000,

            });
          });
    }
  },
}
</script>

<style scoped>
.student-list {
  padding: 15px;
  background-color: #F2F2F2;
}

table {
  border: 1px solid #a21318;
}

thead {
  background-color: #B9F6CA;
}

tbody {

}

.action {
  margin-left: 5px;
  border-radius: 10%;
  padding: 0px 10px;
}

.action:hover {
  background-color: #03a9f4;
  color: papayawhip;
}

.dialog {
  max-width: 600px;
  padding: 10px;
}

.modal-dialog label {
  left: 0;
}
</style>