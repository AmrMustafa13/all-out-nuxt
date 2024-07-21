<template>
  <div class="body-wrapper">
    <div class="container-fluid">
      <div class="card card-body" v-if="load">
        <div>
          <h1 style="font-size: 25px; font-weight: 600">Chatbot Details</h1>
          <p style="margin-bottom: 30px">
            You can follow up with chatbots details
          </p>
        </div>
        <!-- start of edit1 -->
        <form class="mt-4 row" novalidate="">
          <div class="mb-3 form-group col-5">
            <label> By Service </label>
            <div class="controls">
              <select
                @change="filter('service', $event)"
                name="select"
                id="select"
                required=""
                class="form-control"
                aria-invalid="false"
              >
                <option value="">All</option>
                <!-- <option
                  :value="item.id"
                  v-for="item in services.content"
                  :key="item.id + 'serv'"
                >
                  {{ item.titleEn }}
                </option> -->
              </select>
              <div class="help-block"></div>
            </div>
          </div>
          <div class="mb-3 form-group col-2">
            <label>
              Display
              <span class="text-danger">*</span>
            </label>
            <div class="controls">
              <select
                name="select"
                id="select"
                required=""
                class="form-control"
                aria-invalid="false"
              >
                <option value="">10</option>
              </select>
              <div class="help-block"></div>
            </div>
          </div>
          <div class="mb-3 form-group col-2">
            <label>
              Order
              <span class="text-danger">*</span>
            </label>
            <div class="controls">
              <select
                name="select"
                id="select"
                required=""
                class="form-control"
                aria-invalid="false"
              >
                <option value="">Date Descending</option>
              </select>
              <div class="help-block"></div>
            </div>
          </div>
          <div class="mb-3 form-group col-2">
            <a
              href="javascript:void(0)"
              class="btn btn-primary d-flex align-items-center px-3"
              id="add-notes"
              style="margin-top: 1.5em"
            >
              <i class="ti ti-search me-0 me-md-1 fs-4"></i>
              <span class="d-none d-md-block font-weight-medium fs-3"
                >Search</span
              >
            </a>
          </div>
        </form>
        <ul
          class="nav nav-pills p-3 mb-3 rounded align-items-center card flex-row"
        >
          <li class="nav-item">
            <a
              href="javascript:void(0)"
              class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
              id="all-category"
              @click="filter('status', 'ALL')"
              :class="{ active: step == 'ALL' }"
            >
              <i class="ti ti-list fill-white me-0 me-md-1"></i>
              <span class="d-none d-md-block font-weight-medium">All</span>
            </a>
          </li>
          <li class="nav-item">
            <a
              href="javascript:void(0)"
              class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
              id="note-business"
              @click="filter('status', 'Paid')"
              :class="{ active: step == 'Paid' }"
            >
              <span class="d-none d-md-block font-weight-medium">PAID</span>
            </a>
          </li>
          <li class="nav-item">
            <a
              href="javascript:void(0)"
              class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
              id="note-social"
              @click="filter('status', 'Pending')"
              :class="{ active: step == 'Pending' }"
            >
              <span class="d-none d-md-block font-weight-medium">PENDING</span>
            </a>
          </li>
          <li class="nav-item">
            <a
              href="javascript:void(0)"
              class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
              id="note-social"
              @click="filter('status', 'Unpaid')"
              :class="{ active: step == 'Unpaid' }"
            >
              <span class="d-none d-md-block font-weight-medium">UNPAID</span>
            </a>
          </li>
          <li class="nav-item">
            <a
              href="javascript:void(0)"
              class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
              id="note-social"
              @click="filter('status', 'Refunded')"
              :class="{ active: step == 'Refunded' }"
            >
              <span class="d-none d-md-block font-weight-medium">REFUNDED</span>
            </a>
          </li>
          <li class="nav-item">
            <a
              href="javascript:void(0)"
              class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
              id="note-social"
              @click="filter('status', 'Canceled')"
              :class="{ active: step == 'Canceled' }"
            >
              <span class="d-none d-md-block font-weight-medium">CANCELED</span>
            </a>
          </li>
          <li
            class="nav-item ms-auto row gap-1"
            style="position: absolute; right: 1em"
          >
            <a
              href="javascript:void(0)"
              class="btn btn-primary d-flex align-items-center px-2 col-5"
              style="width: 5em"
              id="add-vendor"
              @click="showModal('addVendorModal')"
            >
              <i class="ti ti-file-export me-0 me-md-1 fs-4"></i>
              <span class="d-none d-md-block font-weight-medium fs-3">New</span>
            </a>
          </li>
        </ul>

        <!-- end of edit1 -->
        <div>
          <div style="padding: 16px 16px 0px">
            <h3 style="font-size: 20px; font-weight: 600">List</h3>
            <p>You can follow up with</p>
          </div>
          <div class="table-responsive">
            <table
              style="border-collapse: collapse"
              class="table search-table align-middle text-nowrap"
            >
              <thead class="header-item">
                <tr>
                  <th>ID</th>
                  <th>Service</th>
                  <th>Customer</th>
                  <th>N.Reservations</th>
                  <th>N.Users</th>
                  <th>Date</th>
                  <th>Phone</th>
                  <!-- Added -->
                  <th>Status</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <!-- start row -->
                <tr
                  class="search-items"
                  v-for="item in listing"
                  :key="'booking-' + item.id"
                >
                  <td>{{ item.id }}</td>
                  <td>{{ item.serviceName }}</td>
                  <td>{{ item.customerName }}</td>
                  <td>{{ item.noReservations }}</td>
                  <td>{{ item.numOfUsers }}</td>
                  <td>{{ getDate(item.dateAndTime) }}</td>
                  <td>{{ item.phoneNumber }}</td>
                  <!-- Added -->
                  <td>{{ item.status }}</td>

                  <td>
                    <div class="action-btn">
                      <a
                        @click="goto(item.id)"
                        href="javascript:void(0)"
                        class="text-info edit"
                      >
                        <div class="eye-display">
                          <img src="/icons/eye-filled.png" alt="" />
                        </div>
                      </a>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
            <ul class="pagination">
              <li class="page-item" v-if="listing.number > 1">
                <a
                  class="page-link link"
                  @click="pagination($event, listing.number - 1)"
                  href=""
                  aria-label="Previous"
                >
                  <span aria-hidden="true">
                    <i class="ti ti-chevrons-left fs-4"></i>
                  </span>
                </a>
              </li>
              <li
                class="page-item"
                v-for="page in listing.totalPages"
                :class="{ active: page == listing.number }"
                :key="page + 'pag'"
              >
                <a
                  class="page-link link"
                  @click="pagination($event, page)"
                  href=""
                  >{{ page }}</a
                >
              </li>
              <li class="page-item" v-if="!listing.last">
                <a
                  class="page-link link"
                  @click="pagination($event, listing.number + 1)"
                  href=""
                  aria-label="Next"
                >
                  <span aria-hidden="true">
                    <i class="ti ti-chevrons-right fs-4"></i>
                  </span>
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <div
      class="modal fade"
      id="getChatbotRequestDeatils"
      tabindex="-1"
      role="dialog"
      aria-labelledby="getChatbotRequestDeatilsTitle"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              ChatBot Deatils :
            </h5>
          </div>
          <div class="modal-body">
            <ul class="details">
              <li>
                <strong>Customer Name : </strong
                ><span>{{ data.customerName }}</span>
              </li>
              <li>
                <strong>Service Name : </strong
                ><span>{{ data.serviceName }}</span>
              </li>
              <li>
                <strong>Phone Number : </strong
                ><span>{{ data.phoneNumber }}</span>
              </li>
              <li>
                <strong>Location : </strong><span>{{ data.location }}</span>
              </li>
              <li>
                <strong>NO. Reservations : </strong
                ><span>{{ data.noReservations }}</span>
              </li>
              <li>
                <strong>NO. Of Users : </strong
                ><span>{{ data.numOfUsers }}</span>
              </li>
              <li>
                <strong>Service ID : </strong><span>{{ data.serviceId }}</span>
              </li>
              <li>
                <strong>User ID : </strong><span>{{ data.userId }}</span>
              </li>
              <li>
                <strong>Date And Time : </strong
                ><span>{{ formatDate(data.dateAndTime) }}</span>
              </li>
              <li>
                <strong>Created At : </strong
                ><span>{{ formatDate(data.createdAt) }}</span>
              </li>
            </ul>
          </div>
          <div class="modal-footer">
            <button
              @click="closeModal"
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  layout: "adminLte",
  head() {
    return {
      title: "Booking | Manage",
      link: [
        {
          rel: "stylesheet",
          href: "https://fonts.googleapis.com/icon?family=Material+Icons",
        },
        {
          rel: "stylesheet",
          href: "https://cdn.datatables.net/1.10.20/css/dataTables.bootstrap.min.css",
        },
      ],
      script: [
        {
          src: "/assets/Modernize/vendor/fullcalendar/index.global.min.js",
          defer: true,
          body: true,
          callback: this.onScriptLoaded,
        },
        {
          src: "https://cdn.datatables.net/1.10.20/js/jquery.dataTables.min.js",
          defer: true,
          body: true,
          callback: this.onScriptLoaded,
        },
        {
          src: "https://cdn.datatables.net/1.10.20/js/dataTables.bootstrap4.min.js",
          body: true,
          skip: !this.externalLoaded,
        },
      ],
    };
  },
  name: "usertype",
  data() {
    return {
      dataTable: null,
      link: "/base/api/chatbotrequests",
      load: false,
      data: {
        id: null,
        customerName: "X",
        serviceName: "X",
        phoneNumber: "X",
        location: "X",
        noReservations: "X",
        serviceId: "X",
        numOfUsers: "X",
        dateAndTime: "X",
        userId: "X",
        createdAt: "X",
      },
      step: "ALL",
      config: {
        auth: {
          username: "user",
          password: "123456",
        },
      },
    };
  },
  async mounted() {
    if (!process.client) return;
    $(".preloader").show();
    let token = JSON.parse(localStorage.getItem("data"));
    if (!token) this.$router.push(this.localeLocation({ name: "login" }));
    this.getAll();
  },
  component: {},
  methods: {
    pagination(e, page) {
      e.preventDefault();
      if (page == 0) page = 1;
      axios
        .get(this.link + "?page=" + page + "&size=10", this.config)
        .then((response) => {
          if (response.data.success == true) {
            this.listing = response.data.response;
            setTimeout(() => {
              this.load = true;
            }, 500);
          } else {
            this.listing = [];
          }
        })
        .catch((error) => {
          this.$toast.error(error.response.data.messageText).goAway(1500);
        });
      return false;
    },
    money(amount) {
      const formatter = new Intl.NumberFormat("ae-AE", {
        style: "currency",
        currency: "AED",
      });

      return formatter.format(amount);
    },
    filter(key, e) {
      this.load = false;
      let value = null;
      if (key != "status") value = e.target.value;
      else value = e;
      let url = this.link;
      if (key == "service") {
        url = "/base/api/bookings/byServiceId?serviceId=" + value;
        this.step = "ALL";
      }
      if (key == "status") {
        url = "/base/api/bookings/byStatus?status=" + value;
        if (value == "ALL") {
          url = this.link;
        }
        this.step = value;
      }
      if (!value) url = this.link;
      axios
        .get(url, this.config)
        .then((response) => {
          if (response.data) {
            this.listing = response.data.response;
            setTimeout(() => {
              this.load = true;
            }, 500);
          } else this.$toast.error(response.data.message).goAway(1500);
        })
        .catch((error) => {
          this.$toast.error(error.response.data.messageText).goAway(1500);
        });
    },
    getDate(timing) {
      return moment.utc(timing).format("MMMM DD, YYYY HH:mm:ss");
    },
    async getAll() {
      $(".preloader").show();
      await axios.all([axios.get(this.link, this.config)]).then(
        axios.spread((listing) => {
          this.listing = listing.data.response;
          console.log(listing.data.response);
          // this.customers = customers.data.response;
          // this.services = services.data.response;
          // this.statuses = statuses.data.response;
          if (listing.data.success == true) {
            setTimeout(() => {
              this.load = true;
              $(".preloader").hide();
            }, 500);
          }
          this.$forceUpdate();
        })
      );
    },
    performAction(id = null) {
      this.$confirm("Are you sure you want to perform this action?").then(
        () => {
          this.token = JSON.parse(localStorage.getItem("access_token"));
          axios
            .delete(this.link + "/" + id, this.config)
            .then((response) => {
              if (response.data.success) {
                this.load = false;
                this.$toast.success(response.data.response.msg).goAway(1500);
                this.getAll();
                setTimeout(() => {
                  this.load = true;
                }, 500);
              } else this.$toast.error(response.data.response.msg).goAway(1500);
            })
            .catch((error) => {
              this.$toast.error(error.response.data.messageText).goAway(1500);
            });
        }
      );
    },
    onScriptLoaded() {
      this.externalLoaded = true;
      setTimeout(() => {
        this.dataTable = $("#example").DataTable({
          authWidth: true,
          responsive: true,
          bDestroy: true,
        });
      }, 300);
    },
    handleSingleFileUpload(file) {
      this.file = file;
    },
    goto(code) {
      axios
        .get(this.link + "/" + code, this.config)
        .then((response) => {
          if (response.data.success) {
            const data = response.data.response;
            // alert(JSON.stringify(data))
            this.data = data;
            $("#getChatbotRequestDeatils").modal("show");
          } else this.$toast.error(response.data.message).goAway(1500);
        })
        .catch((error) => {
          this.$toast.error(error.response.data.messageText).goAway(1500);
        });

      // $("#getChatbotRequestDeatils").modal("show");
      // alert(code);
      // this.$router.push(
      //     this.localeLocation({
      //         path: `${code}`,
      //     })
      // );
    },
    closeModal() {
      $("#getChatbotRequestDeatils").modal("hide");
    },
    formatDate(dateString) {
      try {
        const date = new Date(dateString);
        if (isNaN(date)) {
          throw new Error("Invalid Date");
        }

        // Extract parts of the date
        const year = date.getUTCFullYear();
        const month = date.toLocaleString("default", {
          month: "long",
          timeZone: "UTC",
        });
        const day = date.getUTCDate();
        const hours = date.getUTCHours().toString().padStart(2, "0");
        const minutes = date.getUTCMinutes().toString().padStart(2, "0");
        const seconds = date.getUTCSeconds().toString().padStart(2, "0");

        return `${month} ${day}, ${year}, ${hours}:${minutes}:${seconds} UTC`;
      } catch (error) {
        console.error("Error formatting date:", error);
        return "Invalid date";
      }
    },
  },
};
</script>

<style scoped>
ul.details {
  list-style-type: disc;
  padding: 20px;
}

ul.details li span {
  font-size: 17px;
}
ul.details li strong {
  font-size: 18px;
}
</style>
<!--  -->
