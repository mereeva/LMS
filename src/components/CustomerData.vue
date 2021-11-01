<template>
    <div class="container customer-data">
      <table class="table table-striped table-bordered">
         <thead>
            <tr>
              <th>Name </th> 
              <th>Username </th> 
              <th>Email </th> 
              <th>Address </th> 
              <th>Phone </th> 
              <th>Website </th> 
              <th>Company </th> 
              <th>Action </th> 
           </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" v-bind:key="user.id"> 
            <td>{{ user.name | uppercase}} </td> 
            <td>{{ user.username }} </td> 
            <td>{{ user.email}} </td> 
            <td>{{ userAddress(user) }} </td> 
            <td>{{ user.phone | sanitizePhone}} </td> 
            <td>{{ user.website}}</td> 
            <td>{{ userCompany(user) }}</td> 
            <td>
              <i @click="onDelete(user.id)" class="fas fa-times"></i> 
              &nbsp;
              <i @click="showModal(user)" class="fas fa-eye"></i>
            </td> 
            
          </tr>
        </tbody>
      </table>

      <UserModal :userData=modalData
      v-show="isModalVisible"
      @close="closeModal"

    />
   
    </div>
</template>


<script>
import UserModal from "../components/UserModal.vue";


export default {
  name: "CustomerData",
  components: {
    UserModal,
  },
   data() {
    return {
      users: [],
      isModalVisible: false,
      modalData: Object
    };
  },
  filters: {
    uppercase: function(value) {
      return value.toUpperCase();
    },
    sanitizePhone: function(value){
      return value.substring(value.indexOf('x') + 1);
    },
  },
  methods: {
    async getData() {
      try {
      const response = await fetch("https://jsonplaceholder.typicode.com/users")
        // JSON responses are automatically parsed.
        const data = await response.json();
        return data
      } catch (error) {
        console.log(error);
      }
    },
    userAddress: function(user){
      // console.log(user.address.street);
      // return user.address.street;
      return user.address.street + ', ' + user.address.suite+' - '+ user.address.city;
    },
    userCompany: function(user){
      return user.company.name;
    },
    async onDelete(id){
       if(confirm("Are you sure?")){
        const res = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method:'DELETE'
        })
        res.status===200 ? (this.users = this.users.filter((user) => user.id !== id)) : alert('Error Deleting User')
      }
      // this.users = this.users.filter((user) => user.id !== id);
    },
    showModal(data) {
      this.modalData = data;  
      console.log(this.modalData);  
      this.isModalVisible = true;

    },
    closeModal() {
      this.isModalVisible = false;
    }
  },
  async created() {
    this.users = await this.getData();
  },
};


</script>
