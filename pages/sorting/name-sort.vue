<template>
    <div>
      <div class="grid grid-cols p-2">
        <div class=" p-1 flex justify-between w-full">
          <div class="mb-1 flex gap-5 justify-start items-center pn">
            <input type="checkbox">
            <label for="checkbox" class="text-gray-500 text-lg font-medium">Information</label>
          </div>
          <div class="mb-1 pn">
            <p class="text-gray-500 text-lg font-medium px-4 py-3">Project Name</p>
          </div>
          <div class="mb-1 flex md:flex-row md:items-center gap-2">
            <div class="relative">
              <input type="text" class="w-full md:w-auto px-4 py-3 focus:outline-none focus:border-blue-500"
                v-model="searchQuery" placeholder="Search user name...">
            </div>
           
            <nuxt-link to="/filter-col" class="bg-white p-2 mb-2 md:mb-0 md:mr-2">
              <i class="fa-solid fa-bars text-gray-400"></i>
            </nuxt-link>
            <nuxt-link to="/datetable" class="bg-white p-2 mb-2 md:mb-0">
              <i class="fa-solid fa-table text-gray-400"></i>
            </nuxt-link>
            <button class="bg-blue-700 p-2 text-white text-md rounded mb-2 md:mb-0 md:mr-2">
              <i class="fa-solid fa-file p-1"></i>
              <span class="hidden md:inline">Team files</span>
            </button>
            <button @click="exportDataToCsv" class="bg-red-700 p-2 text-white text-md rounded mb-2 md:mb-0 md:mr-2">
              <i class="fa-solid fa-file-export p-1"></i>
              <span class="hidden md:inline">Export CSV</span>
            </button>
          </div>
        </div>
      </div>
      <div class="container w-full p-2 bg-white">
  
        <div class="sub-container w-full px-5 py-2" style="background-color: #EFF4FA;">
          <div class="mini-container w-full p-2 bg-white">
            <div class="overflow-x-auto resizable-box" :style="{ height: boxHeight }">
              <table class="min-w-full divide-y divide-gray-200 box" >
                <thead>
                  <tr>
                    <th class="px-6 py-8 text-left text-md font-medium text-gray-600 uppercase  tracking-widergap-1">
                      <nuxt-link to="/">Customer ID</nuxt-link>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider ">
                      <nuxt-link to="/sorting/name-sort">Users
                        <button @click="toggleSortOrder" class="p-0 text-red-500" style="margin-top: -9%;">
                          <i class="fa-solid fa-chevron-up "></i>
                          <i class="fa-solid fa-chevron-down "></i>
                        </button>
                      </nuxt-link>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider"><nuxt-link to="/sorting/date-sort">Joined</nuxt-link></th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider"><nuxt-link>Status</nuxt-link></th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider"><nuxt-link>Purchased</nuxt-link>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider"><nuxt-link>Profile</nuxt-link>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider"><nuxt-link>Action</nuxt-link></th>
                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="user in allcomputfunction" :key="user.id">
                    <td class="px-6 py-4 whitespace-nowrap">
                      <h4 class="text-black-500 font-medium">#{{ user.id }}</h4>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <h4 class="text-black-500 font-medium">{{ user.name }}</h4>
                      <p class="text-gray-400">{{ user.email }}</p>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <h4 class="text-black-500 font-medium">{{ user.date }}</h4>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <p class="p-2 bg-green-400 rounded-md text-white flex items-center justify-center">{{ user.status }}
                      </p>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <h4 class="text-black-500 font-medium">{{ user.purchased }}</h4>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap"><nuxt-link class="p-2 bg-blue-400 rounded-md text-white flex items-center justify-center">{{ user.profile}}</nuxt-link></td>
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
  import users from '~/data/user.js';
  export default {
    data() {
      return {
        searchQuery: '',
        users: users,
        sortOrder: 'ascending',
        isExpanded: false,
       
     
  
      }
    },
    computed: {
      combinedSortedUsers() {
        // Check if searchQuery is provided
        if (this.searchQuery) {
          return this.users.filter(user => {
            // Filter users based on searchQuery
            return  user.name.toLowerCase().includes(this.searchQuery.toLowerCase()) || user.email.toLowerCase().includes(this.searchQuery.toLowerCase()) || user.id === parseInt(this.searchQuery) ||  user.name.charAt(0).toLowerCase() === this.searchQuery.toLowerCase();
          }).sort((a, b) => {
            // Sort the filtered users by user.id
            if (this.sortOrder === 'ascending') {
                return a.name.localeCompare(b.name);
            } else {
                return b.name.localeCompare(a.name);
            }
          });
        } else {
          // If searchQuery is not provided, return sorted users based on user.id
          const sortedUsers = [...this.users];
          sortedUsers.sort((a, b) => {
            if (this.sortOrder === 'ascending') {
                return a.name.localeCompare(b.name);
            } else {
                return b.name.localeCompare(a.name);
            }
          });
          return sortedUsers;
        }
  
      },
      allcomputfunction() {
        return [...this.combinedSortedUsers];
      },
    
      boxHeight() {
        return this.isExpanded ? 'auto' : '500px';
      },
      buttonText() {
        return this.isExpanded ? 'Less' : 'More';
      },
    
  
  
  
  
  
    },
  
  
    methods: {
      toggleSortOrder() {
        // Toggle the sort order between ascending and descending
        this.sortOrder = this.sortOrder === 'ascending' ? 'descending' : 'ascending';
      },
      exportDataToCsv() {
     const csv=users;
        const csvContent = this.convertToCsv(csv);
        const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8' });
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.setAttribute('download', 'export_data.csv');
        link.click();
      },
      convertToCsv(data) {
        const headers = Object.keys(data[0]);
        const rows = data.map(obj => headers.map(header => obj[header]));
        const headerRow = headers.join(',');
        const csvRows = [headerRow, ...rows.map(row => row.join(','))];
        return csvRows.join('\n');
      },
      toggleBoxHeight() {
        this.isExpanded = !this.isExpanded;
      }, 
    },
  
  
  
  }
  
  </script>
  
  <style>
  @import url('~/assets/css/components.css');
  .mini-container {
    position: relative;
  }
  
  .resizable-box {
    overflow: hidden;
    transition: height 0.5s ease;
    overflow-x: visible;
  }
  
  </style>