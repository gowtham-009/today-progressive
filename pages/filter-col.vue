  <template>
    <div>
      <div class="grid grid-cols-1 p-2">
         <div class=" p-1 justify-between w-full">
            <p class="text-gray-500 font-medium text-md pl-1">Get Particular coloum</p>
        <div class="contain w-full flex gap-3">
        <select v-model="selectedColumn" class="block text-md w-full px-5 py-2 border rounded-md bg-white focus:outline-none focus:border-white">
       
        <option class="text-black-500 font-medium text-md" value="id">Customer ID</option>
        <option class="text-black-500 font-medium text-md" value="name">Users</option>
        <option class="text-black-500 font-medium text-md" value="email">Email</option>
        <option class="text-black-500 font-medium text-md" value="date">Joined</option>
        <option class="text-black-500 font-medium text-md" value="status">Status</option>
        <option class="text-black-500 font-medium text-md" value="purchased">Purchased</option>
        </select>
            <nuxt-link to="/"  class="bg-white p-2 mb-2 md:mb-0 bg-blue-600 rounded">
                <i class="fa-solid fa-rotate-left text-red-500"></i>
            </nuxt-link>
            </div>
         </div>
      </div>
      <div class="container w-full p-2 bg-white">
        
          <div class="sub-container w-full px-5 py-2" style="background-color: #EFF4FA;">
              <div class="mini-container w-full p-2 bg-white">
                <div class="overflow-x-auto">
  <table class="min-w-full divide-y divide-gray-200">
   
    <tbody class="bg-white divide-y divide-gray-200">
      <tr v-for="user in sortedUsers":key="user.id">
       <p class="text-black-500 font-medium text-md"> {{getColumnValue(user, selectedColumn)}}</p>
      </tr>
    </tbody>
  </table>
 
</div>
              </div>
          </div>
      </div>
    </div>
</template>

  <script>
  import users from '~/data/user.js'
  export default {
    data() {
      return {
        selectedColumn: 'id',
        users:users
      };
    },
    computed: {
      sortedUsers() {
        // Return a sorted copy of users array based on selected column
        return this.users.slice().sort((a, b) => {
          if (this.selectedColumn === 'id' || this.selectedColumn === 'purchased') {
            return a[this.selectedColumn] - b[this.selectedColumn];
          } else if (this.selectedColumn === 'name' || this.selectedColumn === 'email' || this.selectedColumn === 'status') {
            return a[this.selectedColumn].localeCompare(b[this.selectedColumn]);
          } else if (this.selectedColumn === 'date') {
            // Assuming 'joined' is a date field, you can compare it directly
            return new Date(a.joined) - new Date(b.joined);
          }
          return 0;
        });
      }
    },
    methods: {
      getColumnValue(user, column){
        // Return the value of the selected column for the given user
        return user[column];
      }
    }
  };
  </script>
  