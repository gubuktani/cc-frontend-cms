<template>
  <!-- Main -->
  <main class="main-dashboard">
    <!-- Content -->
    <section
      class="flex flex-row justify-between gap-6 flex-grow max-md:max-w-none"
    >
      <div class="flex flex-col gap-8 max-sm:gap-6 w-full">
        <!-- Headline -->
        <div class="flex justify-between">
          <h1 class="font-bold text-2xl text-dark self-center py-1">
            User List
          </h1>
          <button
            type="button"
            class="btn btn-primary p-3 self-center flex flex-row gap-1.5 items-center max-sm:p-[10px]"
            @click="showModal(null)"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="2"
              stroke="currentColor"
              class="w-5 h-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 4.5v15m7.5-7.5h-15"
              />
            </svg>
            <p class="text-sm">Add User</p>
          </button>
        </div>

        <!-- Search -->
        <div class="flex justify-between">
          <div class="relative w-full">
            <div
              class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
            >
              <svg
                class="w-5 h-5 text-gray-500"
                aria-hidden="true"
                fill="currentColor"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                  clip-rule="evenodd"
                ></path>
              </svg>
            </div>
            <input
              type="text"
              id="table-search-users"
              class="block p-2 pl-10 text-sm text-gray-900 w-full input-field"
              placeholder="Search for users"
              v-model="query.search"
              v-on:keyup.enter="$fetch()"
            />
          </div>
        </div>

        <!-- User List -->
        <div class="flex flex-col gap-4 w-full">
          <!-- Show Content -->
          <div class="relative overflow-x-auto h-[500px]">
            <table class="w-full text-sm text-left text-gray-500">
              <thead
                class="text-xs text-gray-700 uppercase bg-gray-100 sticky inset-0"
              >
                <tr>
                  <th scope="col" class="px-6 py-3">Name</th>
                  <th scope="col" class="px-6 py-3 max-sm:hidden">Email</th>
                  <th scope="col" class="px-6 py-3 max-sm:hidden">City</th>
                  <th scope="col" class="px-6 py-3"></th>
                </tr>
              </thead>
              <tbody class="overflow-y-auto">
                <tr
                  class="bg-white border-b hover:bg-gray-50"
                  v-for="(item, index) in listUsers"
                  :key="item.id"
                >
                  <th
                    scope="row"
                    class="flex items-center max-sm:items-start px-6 py-4 text-gray-900 whitespace-nowrap"
                  >
                    <img
                      class="w-10 h-10 rounded-full max-sm:mt-[0.75px]"
                      :src="
                        item.avatar
                          ? imgUrl + item.avatar
                          : 'assets/images/user.png'
                      "
                      :alt="item.username + 'image'"
                    />
                    <div class="pl-3 max-sm:pl-4">
                      <div class="flex gap-0.5 items-center">
                        <div class="text-base font-semibold">
                          {{ item.name }}
                        </div>

                        <!-- Admin Icon -->
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20"
                          fill="currentColor"
                          class="w-5 h-5 fill-yellow-500"
                          v-if="item.role == 'admin'"
                        >
                          <path
                            fill-rule="evenodd"
                            d="M8 7a5 5 0 113.61 4.804l-1.903 1.903A1 1 0 019 14H8v1a1 1 0 01-1 1H6v1a1 1 0 01-1 1H3a1 1 0 01-1-1v-2a1 1 0 01.293-.707L8.196 8.39A5.002 5.002 0 018 7zm5-3a.75.75 0 000 1.5A1.5 1.5 0 0114.5 7 .75.75 0 0016 7a3 3 0 00-3-3z"
                            clip-rule="evenodd"
                          />
                        </svg>

                        <!-- Premium Icon -->
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20"
                          fill="currentColor"
                          class="w-5 h-5 fill-primary-1"
                          v-if="item.type == 'premium'"
                        >
                          <path
                            fill-rule="evenodd"
                            d="M16.403 12.652a3 3 0 000-5.304 3 3 0 00-3.75-3.751 3 3 0 00-5.305 0 3 3 0 00-3.751 3.75 3 3 0 000 5.305 3 3 0 003.75 3.751 3 3 0 005.305 0 3 3 0 003.751-3.75zm-2.546-4.46a.75.75 0 00-1.214-.883l-3.483 4.79-1.88-1.88a.75.75 0 10-1.06 1.061l2.5 2.5a.75.75 0 001.137-.089l4-5.5z"
                            clip-rule="evenodd"
                          />
                        </svg>

                        <!-- Disable Icon -->
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20"
                          fill="currentColor"
                          class="w-5 h-5 fill-red-500"
                          v-if="item.disabled_at"
                        >
                          <path
                            fill-rule="evenodd"
                            d="M8.485 2.495c.673-1.167 2.357-1.167 3.03 0l6.28 10.875c.673 1.167-.17 2.625-1.516 2.625H3.72c-1.347 0-2.189-1.458-1.515-2.625L8.485 2.495zM10 5a.75.75 0 01.75.75v3.5a.75.75 0 01-1.5 0v-3.5A.75.75 0 0110 5zm0 9a1 1 0 100-2 1 1 0 000 2z"
                            clip-rule="evenodd"
                          />
                        </svg>

                        <!-- Logged User -->
                        <div
                          class="font-normal text-gray-500"
                          v-if="item.id == $auth.user.id"
                        >
                          ( You )
                        </div>
                      </div>
                      <div class="font-normal text-gray-500">
                        {{ item.username }}
                      </div>
                      <div class="flex gap-1.5 items-center mt-2 sm:hidden">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20"
                          fill="currentColor"
                          class="w-5 h-5 fill-gray-400"
                        >
                          <path
                            d="M3 4a2 2 0 00-2 2v1.161l8.441 4.221a1.25 1.25 0 001.118 0L19 7.162V6a2 2 0 00-2-2H3z"
                          />
                          <path
                            d="M19 8.839l-7.77 3.885a2.75 2.75 0 01-2.46 0L1 8.839V14a2 2 0 002 2h14a2 2 0 002-2V8.839z"
                          />
                        </svg>
                        <div class="font-normal text-gray-500">
                          {{ item.email }}
                        </div>
                      </div>
                      <div class="flex gap-1.5 mt-1 sm:hidden">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20"
                          fill="currentColor"
                          class="w-5 h-5 fill-gray-400"
                        >
                          <path
                            fill-rule="evenodd"
                            d="M9.69 18.933l.003.001C9.89 19.02 10 19 10 19s.11.02.308-.066l.002-.001.006-.003.018-.008a5.741 5.741 0 00.281-.14c.186-.096.446-.24.757-.433.62-.384 1.445-.966 2.274-1.765C15.302 14.988 17 12.493 17 9A7 7 0 103 9c0 3.492 1.698 5.988 3.355 7.584a13.731 13.731 0 002.273 1.765 11.842 11.842 0 00.976.544l.062.029.018.008.006.003zM10 11.25a2.25 2.25 0 100-4.5 2.25 2.25 0 000 4.5z"
                            clip-rule="evenodd"
                          />
                        </svg>
                        <div class="font-normal text-gray-500">
                          {{ item.city ? item.city : '-' }}
                        </div>
                      </div>
                    </div>
                  </th>
                  <td class="px-6 py-4 max-sm:hidden">
                    {{ item.email }}
                  </td>
                  <td class="px-6 py-4 max-sm:hidden">
                    {{ item.city ? item.city : '-' }}
                  </td>
                  <td class="px-6 py-4 w-4">
                    <!-- Modal toggle -->
                    <div
                      class="font-semibold text-primary-1 cursor-pointer"
                      @click="showModal(item.id)"
                    >
                      Edit
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Pagination -->
          <nav
            class="flex items-center justify-between pt-4"
            aria-label="Table navigation"
          >
            <span class="text-sm font-normal text-gray-500"
              >Showing
              <span class="font-semibold text-gray-900"
                >{{ users.from ? users.from : 0 }} -
                {{ users.to ? users.to : 0 }}</span
              >
              of
              <span class="font-semibold text-gray-900">{{
                users.total
              }}</span></span
            >
            <ul class="inline-flex items-center -space-x-px">
              <li class="cursor-pointer" v-for="page in listPages">
                <!-- Inactive -->
                <div
                  class="px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700"
                  v-if="
                    !page.active &&
                    page.label != '&laquo; Previous' &&
                    page.label != 'Next &raquo;'
                  "
                  @click="changePage(page.label)"
                >
                  {{ page.label }}
                </div>

                <!-- Active -->
                <div
                  aria-current="page"
                  class="z-10 px-3 py-2 leading-tight text-white border border-primary-1 bg-primary-1 hover:bg-primary-2 hover:text-white"
                  v-if="page.active"
                >
                  {{ page.label }}
                </div>

                <!-- Previous -->
                <div
                  class="px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700 rounded-l-md"
                  v-if="!page.active && page.label == '&laquo; Previous'"
                  @click="previousPage()"
                >
                  &laquo;
                  <span class="max-sm:hidden"> Previous</span>
                </div>

                <!-- Next -->
                <div
                  class="px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700 rounded-r-md"
                  v-if="!page.active && page.label == 'Next &raquo;'"
                  @click="nextPage()"
                >
                  <span class="max-sm:hidden">Next </span>
                  &raquo;
                </div>
              </li>
            </ul>
          </nav>
        </div>
      </div>
    </section>

    <!-- Modal -->
    <div
      class="fixed inset-0 flex justify-center items-center bg-black bg-opacity-50 z-[999]"
      v-if="modal"
      @click="closeModal"
    >
      <div
        class="w-[450px] max-md:w-[90%] card p-4 bg-white overflow-y-auto shadow-lg h-[90%]"
        @click.stop
      >
        <FormUser class="" :user_id="user_id" @close-form="closeModal" />
      </div>
    </div>
  </main>
</template>

<script>
export default {
  middleware: 'admin',
  layout: 'LayoutDashboard',
  data() {
    return {
      imgUrl: process.env.imgUrl,
      users: [],

      // Options
      query: {
        page: 1,
        search: null,
        limit: 10,
      },

      modal: false,

      // Edit User
      user_id: null,
    }
  },
  async fetch() {
    const query = {}

    for (const [key, value] of Object.entries(this.query)) {
      if (value) {
        query[key] = value
      }
    }

    const resUsers = await this.$axios.get('/api/users', {
      params: query,
    })

    this.users = resUsers.data.result
    this.query.page = this.users.current_page
  },
  computed: {
    // Get user list
    listUsers() {
      return this.users.data
    },

    // Get page list
    listPages() {
      return this.users.links
    },
  },
  methods: {
    // Change Page
    changePage(page) {
      this.query.page = page
      this.$fetch()
    },

    // Previous Page
    previousPage() {
      if (this.query.page != 1) {
        this.query.page = this.query.page - 1
        this.$fetch()
      }
    },

    // Next Page
    nextPage() {
      if (this.query.page != this.users.last_page) {
        this.query.page = this.query.page + 1
        this.$fetch()
      }
    },

    // Open Modal
    showModal(id) {
      this.modal = true
      this.user_id = id
    },

    // Close Modal
    closeModal() {
      this.modal = false
      this.user_id = null
    },
  },
}
</script>
