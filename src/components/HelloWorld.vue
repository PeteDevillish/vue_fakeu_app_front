<template>
  <div class="hello">

    <div class="user">
        <img v-bind:src=user.image>
      <div>
        <div class="a">{{user.uid}}</div>
        <span class="a">{{user.firstname}}</span>
        <span class="a">{{user.lastname}}</span>
        <div class="a">{{ user.city }}</div>
        <div class="a">{{ user.address }}</div>
      </div>
      <User>

      </User>


    </div>
    <input type="text" v-model="search" placeholder="search by last name">
    <input type="text" v-model="search2" placeholder="search by last name">
    <div align="center">
      <table class="table">
        <thead>
        <tr>
          <th v-on:click="sort('firstname')">First Name</th>
          <th v-on:click="sort('lastname')">Last Name</th>
          <th v-on:click="sort('city')">City</th>
          <th v-on:click="sort('address')">Address</th>
          <th v-on:click="sort('image')">Image</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="user in filteredUsers" @click="getUser(user)">
          <td>{{ user.firstname }}</td>
          <td>{{ user.lastname }}</td>
          <td>{{ user.city }}</td>
          <td>{{ user.address }}</td>
          <td>{{ user.image }}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import User from "./User";

  export default {
    name: 'HelloWorld',
    components: {User},
    data() {
      return {
        users: [],
        currentSort: 'name',
        currentSortDir: 'asc',
        search: '',
        search2: '',
        user: '',
      }
    },
    mounted: function () {
      axios.get('http://localhost:3000/fake')
        .then((res) => {
          console.log(res.data);
          this.users = res.data;
        })
        .catch((er) => {
          console.log(er);
        });
    },
    methods: {
      sort: function (s) {
        //if s == current sort, reverse
        if (s === this.currentSort) {
          this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
        }
        this.currentSort = s;
      },
      getUser: function (element) {
        this.user = element;
      }
    },
    computed: {
      sortedUsers: function () {
        return this.users.sort((a, b) => {
          let modifier = 1;
          if (this.currentSortDir === 'desc') modifier = -1;
          if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
          if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
          return 0;
        });
      },
      filteredUsers: function () {
        return this.sortedUsers.filter((user) => {
          return user.firstname.match(this.search)
          && user.city.match(this.search2)
        });
      }
    }


  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  table, td, th {
    border: 1px solid #ddd;
    text-align: left;
  }

  table {
    border-collapse: collapse;
    width: 100%;
  }

  th:hover {
    background-color: #a6b4c4;
    cursor: pointer;
  }

  th, td {
    padding: 15px;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  tr:hover {
    background-color: #2b3035;
    color: antiquewhite;
  }

  .user {
    background-color: #2b3035;
    color: antiquewhite;
    width: 50%;
    margin: 0 auto;
    padding: 30px;
    padding-bottom: 50px;
  }
  img{
    float: left;
  }
  .a{
    padding: 10px;
  }

  input{
    margin: 20px;
  }

</style>
