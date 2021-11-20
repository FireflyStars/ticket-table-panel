<template>
  <div class="ticket-container mx-auto p-4 lg:p-8 rounded-3xl shadow-lg border-gray-50">
    <div class="ticket-header">
      <h2 class="mt-2 mb-4 font-bold lg:text-4xl text-base">My Ticket</h2>
      <p class="text-justify w-full lg:w-5/6">Ticket manager allows you to create, edit, view and delete inventory tickets all in one place. you also can create return tickets or checkout inventory on Job Orders.</p>
    </div>
    <div class="ticket-body">
      <nav class="flex border-b-4 border-gray-100 flex-wrap justify-between flex-col-reverse lg:flex-row mt-5 md:mt-0">
        <div class="tab-group grid grid-cols-3 lg:grid-cols-6 place-items-center lg:place-items-start w-full lg:w-3/6 mt-0 mt-5 md:mt-0">
          <a href="javascript:;" class="text-center border-b-4 border-white self-end text-blue-500 mx-0 my-0 py-1" 
            :class="selectedTab == 1 ? 'border-blue-400': ''"
            @click="selectTab(1)">Tickets</a>
          <a href="javascript:;" class="text-center border-b-4 border-white self-end text-blue-500 mx-0 my-0 py-1"
            :class="selectedTab == 2 ? 'border-blue-400': ''"
            @click="selectTab(2)">Returns</a>
          <a href="javascript:;" class="text-center border-b-4 border-white self-end text-blue-500 mx-0 my-0 py-1"
            :class="selectedTab == 3 ? 'border-blue-400': ''"
            @click="selectTab(3)">Jobs</a>
        </div>
        <div class="btn-group lg:w-3/6 w-full grid grid-cols-3 place-items-center lg:place-items-end lg:text-right">
          <a href="javascript:;" 
            class="mx-0 my-2 py-1 px-2 text-sm text-center lg:text-base hover:text-white hover:bg-blue-400 border-2 border-blue-400 rounded duration-100"
            :class="selectedButton == 1 ? 'text-white bg-blue-400': 'text-blue-400'"
            @click="selectButton(1)">New Ticket</a>
          <a href="javascript:;" 
            class="text-sm lg:text-base lg:px-3 text-center hover:text-white hover:bg-blue-400 mx-0 my-2 py-1 px-2 border-2 border-blue-400 rounded duration-100"
            :class="selectedButton == 2 ? 'text-white bg-blue-400': 'text-blue-400'"
            @click="selectButton(2)">Return Ticket</a>
          <a href="javascript:;" 
            class="text-sm lg:text-base text-center text-blue-400 hover:text-white hover:bg-blue-400 mx-0 my-2 py-1 px-2 border-2 border-blue-400 rounded duration-100"
            :class="selectedButton == 3 ? 'text-white bg-blue-400': 'text-blue-400'"
            @click="selectButton(3)">New J.O Ticket</a>
        </div>
      </nav>
      <div class="control-row flex justify-between p-4">
        <div class="from-group flex items-center">
          <div class="custom-control custom-switch">
            <input type="checkbox" class="custom-control-input" name="history" id="history"/>
            <label class="custom-control-label text-gray-400" for="history">History</label>
          </div>
        </div>
        <div class="form-group">
          <select name="count" 
                  v-model="form.perPage" 
                  @change="changePerPageCount" 
                  class="form-select cursor-pointer text-gray-400 py-1 pl-2 pr-10 rounded-sm border-2 border-gray-400 outline-none text-center">
                  id="page_count" 
            <option value="-1" selected>Select</option>
            <option>5</option>
            <option>10</option>
          </select>
        </div>
      </div>
      <div class="ticket-table-panel rounded-xl p-1 sm:bg-white lg:bg-gray-50 lg:border lg:shadow-md">
        <table class="table-fixed w-full">
          <thead>
            <tr class="sm:bg-white lg:bg-gray-50 rounded border-b-2 border-gray-400 rounded-xl">
              <th class="w-4/12 md:w-3/12 lg:w-1/6 whitespace-nowrap text-left uppercase p-2 text-xs lg:text-base">ticket number</th>
              <th class="w-4/12 md:w-3/12 lg:w-1/6 whitespace-nowrap text-center uppercase p-1 lg:p-2 text-xs lg:text-base">warehouse clerk</th>
              <th class="w-4/12 md:w-3/12 lg:w-2/6 whitespace-nowrap text-center uppercase p-1 lg:p-2 text-xs lg:text-base hidden md:hidden lg:table-cell">location </th>
              <th class="w-3/12 md:w-2/12 lg:w-1/6 whitespace-nowrap text-center uppercase p-1 lg:p-2 text-xs lg:text-base">status</th>
              <th class="w-1/12 md:w-1/12 lg:w-1/6"></th>
            </tr>
          </thead>
          <tbody>
            <tr class="bg-gray-50 hover:bg-white" v-for="(item, index) in currentPageTicket" :key="index">
              <td class="px-1 py-4 lg:p-2">
                <div class="table-item text-gray-400">
                  <p class="flex items-center text-xs lg:text-sm">#{{ item.id }}
                    <svg xmlns="http://www.w3.org/2000/svg" class="mx-3 h-4 w-4 text-blue-600 transform motion-safe:hover:scale-150 duration-100 cursor-pointer" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                  </p>
                  <p class="text-xs whitespace-nowrap">{{ item.date_1 }}</p>
                </div>
              </td>
              <td class="py-4 text-center">
                <div class="table-item text-gray-400">
                  <p class="text-xs lg:text-sm whitespace-nowrap">{{ item.warehouse_clerk }}</p>
                  <p class="text-xs lg:text-sm whitespace-nowrap">{{ item.date_2 }}</p>
                </div>
              </td>
              <td class="py-4 text-center hidden md:hidden lg:table-cell">
                <div class="table-item text-gray-400">
                  <p class="text-xs lg:text-sm">{{ item.location }}</p>
                </div>
              </td>
              <td class="py-4 text-center">
                <span class="block w-3/4 lg:w-3/4  mx-auto py-1 text-white rounded-2xl text-xs lg:text-sm" :class="item.status == 1 ? 'bg-green-500' : 'bg-red-500'">{{ item.status == 1 ? 'Complete' : 'Pending' }}</span>
              </td>
              <td class="py-4 text-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-red-400 cursor-pointer mx-auto hover:text-red-500 transform motion-safe:hover:scale-110 duration-100" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="ticket-table-footer flex justify-between rounded-b-xl px-2 py-2 md:py-4 bg-white border-gray-400">
          <div class="ticket-nav-info">
            <span class="font-bold text-xs md:text-base">Showing {{ currentPage }} to {{ totalPage }} of {{ allTickets.length }} results</span>
          </div>
          <div class="ticket-nav-buttons flex">
            <a href="javascript:;" 
                class="previous-btn rounded-l-xl border border-gray-400 p-1 duration-200"
                @click="previousPage">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
              </svg>
            </a>
            <a href="javascript:;" 
                class="next-btn rounded-r-xl border border-gray-400 p-1 duration-200"
                @click="nextPage">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>              
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Ticket',
  components:{

  },
  data(){
    return{
      selectedTab: 1, // 1=> Tickets, 2=> Returns, 3=> Jobs
      selectedButton: 1, // 1=> New Ticket, 2=> Return Ticket, 3=> New J.O Ticket
      form: {
        perPage: -1 // 
      },
      perPage: 5,
      currentPage: 1,
      totalPage: 0,
      currentTickets: [],
      allTickets: [
        {
          id: 1234,
          date_1: "2021-09-03 06:58:00",
          warehouse_clerk	: "Dustin Guedy",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 0,
        },
        {
          id: 1235,
          date_1: "2021-04-04 12:23:00",
          warehouse_clerk	: "Tim Bankey",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 1,
        },
        {
          id: 1236,
          date_1: "2021-09-03 06:58:00",
          warehouse_clerk	: "Dustin Guedy",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 0,
        },
        {
          id: 1237,
          date_1: "2021-04-04 12:23:00",
          warehouse_clerk	: "Tim Bankey",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 1,
        },
        {
          id: 1238,
          date_1: "2021-09-03 06:58:00",
          warehouse_clerk	: "Dustin Guedy",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 0,
        },
        {
          id: 1239,
          date_1: "2021-04-04 12:23:00",
          warehouse_clerk	: "Tim Bankey",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 0,
        },
        {
          id: 1240,
          date_1: "2021-09-03 06:58:00",
          warehouse_clerk	: "Dustin Guedy",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 0,
        },
        {
          id: 1241,
          date_1: "2021-04-04 12:23:00",
          warehouse_clerk	: "Tim Bankey",
          date_2: "2021-09-03 07:20:20",
          location: "Pencacola FL",
          status: 0,
        },
      ]
    }
  },
  mounted(){
    this.totalPage = Math.ceil( this.allTickets.length / this.perPage );
  },
  computed:{
    currentPageTicket(){
      if( this.currentPage*this.perPage > this.allTickets.length ){
        return  this.allTickets.slice( (this.currentPage - 1)*this.perPage, this.allTickets.length );
      }else{
        return  this.allTickets.slice( (this.currentPage - 1)*this.perPage, this.currentPage*this.perPage );
      }
    },
  },
  methods: {
    selectTab(value){
      this.selectedTab = value;
    },
    selectButton(value){
      this.selectedButton = value;
    },
    previousPage(){
      if( ! (this.currentPage == 1) ){
        this.currentPage -= 1;
      }
      console.log(this.currentPage);
    },
    nextPage(){
      if( ! (this.currentPage == this.totalPage) ){
        this.currentPage += 1;
      }
    },
    changePerPageCount(){
      this.perPage = this.form.perPage;
    }
  }
}
</script>
<style scoped>
.ticket-container{
    min-width: 960px;
    max-width: 960px;
}
@media only screen and (max-width: 960px){
  .ticket-container{
    min-width: 485px;
    max-width: 485px;
  }

}
.custom-switch {
    padding-left: 2.25rem;
}
.custom-control {
    position: relative;
    z-index: 1;
    display: block;
    min-height: 1.5rem;
    padding-left: 1.5rem;
}
.custom-control-input {
    position: absolute;
    left: 0;
    z-index: -1;
    width: 1rem;
    height: 1.25rem;
    opacity: 0;
}
.custom-control-label {
    position: relative;
    margin-bottom: 0;
    vertical-align: top;
    cursor: pointer;
}
.custom-switch .custom-control-label::before {
    left: -2.25rem;
    width: 1.75rem;
    pointer-events: all;
    border-radius: 0.5rem;
}
.custom-control-label::before, .custom-file-label, .custom-select {
    transition: background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}
.custom-control-label::before {
    position: absolute;
    top: 0.25rem;
    left: -1.5rem;
    display: block;
    width: 1rem;
    height: 1rem;
    pointer-events: none;
    content: "";
    background-color: #f0f0f0;
    border: #f0f0f0 solid 1px;
}
.custom-switch .custom-control-label::after {
    top: calc(0.25rem + 2px);
    left: calc(-2.25rem + 2px);
    width: calc(1rem - 4px);
    height: calc(1rem - 4px);
    background-color: #4eb3e8;
    border-radius: 0.5rem;
    transition: background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out,-webkit-transform .15s ease-in-out;
    transition: transform .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
    transition: transform .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out,-webkit-transform .15s ease-in-out;
}
.custom-control-label::after {
    position: absolute;
    top: 0.25rem;
    left: -1.5rem;
    display: block;
    width: 1rem;
    height: 1rem;
    content: "";
    background: no-repeat 50%/50% 50%;
}
.custom-control-input:checked~.custom-control-label::before {
    color: #fff;
    border-color: #4eb3e8;
    background-color: #4eb3e8;
}
.custom-switch .custom-control-input:checked~.custom-control-label::after {
    background-color: #fff;
    -webkit-transform: translateX(0.75rem);
    transform: translateX(0.75rem);
}
</style>