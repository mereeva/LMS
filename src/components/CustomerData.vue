<template>
    <div class="container customer-data">
        <div class="search-wrapper">
           Search <input v-model="searchQuery" placeholder="Name/UserName/Street Address">
        </div>
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
          <tr v-for="user in resultQuery" v-bind:key="user.id"> 
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
      modalData: Object,
      searchQuery: null
    };
  },
  computed: {
    resultQuery() {
      //Search user by name, username or Address
      if (this.searchQuery) {
        return this.users.filter(item => {
          return this.searchQuery
            .toLowerCase()
            .split(" ")
            .every(v => item.name.toLowerCase().includes(v) || item.username.toLowerCase().includes(v) || item.address.street.toLowerCase().includes(v));
        });
      } else {
        return this.users;
      }
    }
  },
  filters: {
    uppercase: function(value) {
      return value.toUpperCase();
    },
    sanitizePhone: function(phone){
      //Sanitize if it contains extra info
      if(phone.includes("x")){
        return phone.substring(0, phone.indexOf('x'));
      }
      else{
        return phone;
      }
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

<style>
.search-wrapper{
  font-size: 24px;
  top: 8px;
  padding: 10px;
}
 .search-wrapper label {
    position: absolute;
    font-size: 12px;
    color: #000;
    top: 8px;
    left: 12px;
    z-index: -1;
    transition: .15s all ease-in-out;
  }
 .search-wrapper input {
    padding: 4px 12px;
    color: rgba(0,0,0,.70);
    border: 1px solid rgba(0,0,0,.12);
    transition: .15s all ease-in-out;
    background: white;
    width: 500px;
  }
</style>

