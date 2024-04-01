<template>
    <div>
        <div class="grid grid-cols-1 p-2">
            <span class="text-gray-500 font-medium mt-1 text-md ml-1">Fetch a Particular Range of Data</span>
         <div class=" p-1 flex justify-start gap-2 w-full">
            <input type="date" v-model="startDate" class="px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-blue-500">
            <input type="date" v-model="endDate" class="px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-blue-500">
            <nuxt-link to="/"  class="bg-white p-2 mb-2 md:mb-0 bg-blue-600 rounded">
                <i class="fa-solid fa-rotate-left text-red-500"></i>
            </nuxt-link>
        </div>
      </div>
      <div class="container w-full p-2 bg-white">
        
          <div class="sub-container w-full px-5 py-2" style="background-color: #EFF4FA;">
              <div class="mini-container w-full p-2 bg-white">
                <div class="overflow-x-auto resizable-box" :style="{ height: boxHeight }">
  <table class="min-w-full divide-y divide-gray-200">
    <thead >
      <tr>
        <th class="px-6 py-8 text-left text-md font-medium text-gray-600 uppercase  tracking-widergap-1">
           <p>Customer ID</p>   
        </th>
        <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider ">
          <p>Users</p>
        </th>
        <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">Joined</th>
        <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">Status</th>
        <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">Purchased</th>
        <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">Profile</th>
        <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">Action</th>
      </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-200">
      <tr v-for="user in filteredUsers":key="user.id">
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">#{{ user.id }}</h4></td>
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">{{ user.name }}</h4><p class="text-gray-400">{{ user.email }}</p></td>
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">{{ user.date }}</h4></td>
        <td class="px-6 py-4 whitespace-nowrap"><p class="p-2 bg-green-400 rounded-md text-white flex items-center justify-center">{{ user.status }}</p></td>
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">{{ user.purchased }}</h4></td>
        <td class="px-6 py-4 whitespace-nowrap"><nuxt-link   class="p-2 bg-blue-400 rounded-md text-white flex items-center justify-center">{{ user.profile }}</nuxt-link></td>
        <td class="px-6 py-4 whitespace-nowrap flex gap-3 mt-3">
          <i class="fa-solid fa-pen-to-square text-blue-500"></i>
          <i class="fa-solid fa-circle-check text-green-500"></i>
          <i class="fa-solid fa-trash text-red-500"></i>
        </td>
      </tr>
    </tbody>
  </table>
</div>
<div class="more-less w-full flex justify-end">
   <button @click="toggleBoxHeight" class="bg-blue-500 rounded text-white p-2">{{ buttonText }}</button>
  </div>
 </div>
  </div>
      </div>
    </div>
</template>

<script>
import usersdata from '~/data/user.js'; 
export default {
  data() {
    return {
        startDate: '',
      endDate: '',
      users:usersdata,
      isExpanded: false
    }
  },
  computed: {
    filteredUsers() {
      if (!this.startDate || !this.endDate) {
        return this.users;
      }
      const startDate = new Date(this.startDate);
      const endDate = new Date(this.endDate);
      
      return this.users.filter(user => {
        const userDate = new Date(user.date);
        return userDate >= startDate && userDate <= endDate;
      });
    },
    boxHeight() {
      return this.isExpanded ? 'auto' : '500px';
    },
    buttonText() {
      return this.isExpanded ? 'Less' : 'More';
    }
  },
  methods:{
    toggleBoxHeight() {
      this.isExpanded = !this.isExpanded;
    }
  }
}
</script>

<style>
@import url('~/assets/css/components.css');
</style>