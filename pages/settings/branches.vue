<template>
  <div class="body-wrapper">
    <div class="container-fluid">
      <div
        class="card bg-info-subtle shadow-none position-relative overflow-hidden mb-4"
      >
        <div class="card-body px-4 py-3">
          <div class="row align-items-center">
            <div class="col-9">
              <h4 class="fw-semibold mb-8">{{ $t("Branches") }}</h4>
              <nav aria-label="breadcrumb">
                <ol class="breadcrumb">
                  <li class="breadcrumb-item">
                    <a class="text-muted text-decoration-none" href="/">{{
                      $t("Home")
                    }}</a>
                  </li>
                  <li class="breadcrumb-item" aria-current="page">
                    {{ $t("Branches") }}
                  </li>
                </ol>
              </nav>
            </div>
            <div class="col-3">
              <div class="text-center mb-n5">
                <img
                  src="/Modernize/images/breadcrumb/ChatBc.png"
                  alt=""
                  class="img-fluid mb-n4"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <form class="mt-4 row" novalidate="">
        <div class="mb-3 form-group col-2">
          <label>
            Country
            <span class="text-danger">*</span>
          </label>
          <div class="controls">
            <div class="controls">
              <select
                @change="filter('country', $event)"
                name="select"
                id="select"
                required=""
                class="form-control"
                aria-invalid="false"
              >
                <option value="">All</option>
                <!-- <option
                  :value="item.id"
                  v-for="item in branches"
                  :key="item.id"
                >
                  {{ item.nameEn }}
                </option> -->
              </select>
              <div class="help-block"></div>
            </div>
            <div class="help-block"></div>
          </div>
        </div>
        <!-- <div class="mb-3 form-group col-2">
          <label>
            Status
            <span class="text-danger">*</span>
          </label>
          <div class="controls">
            <select
              @change="filter('status', $event)"
              name="select"
              id="select"
              required=""
              class="form-control"
              aria-invalid="false"
            >
              <option value="">All</option>
              <option value="Active">Active</option>
              <option value="Inactive">Inactive</option>
            </select>
            <div class="help-block"></div>
          </div>
        </div> -->
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
            class="nav-link note-link d-flex align-items-center justify-content-center active px-3 px-md-3 me-0 me-md-2 text-body-color"
            id="all-category"
          >
            <i class="ti ti-list fill-white me-0 me-md-1"></i>
            <span
              @click="filter('status', 'all')"
              class="d-none d-md-block font-weight-medium"
              >All</span
            >
          </a>
        </li>
        <li class="nav-item">
          <a
            @click="filter('status', 1)"
            href="javascript:void(0)"
            class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
            id="note-business"
          >
            <span class="d-none d-md-block font-weight-medium">Active</span>
          </a>
        </li>
        <li class="nav-item">
          <a
            @click="filter('status', 0)"
            href="javascript:void(0)"
            class="nav-link note-link d-flex align-items-center justify-content-center px-3 px-md-3 me-0 me-md-2 text-body-color"
            id="note-social"
          >
            <span class="d-none d-md-block font-weight-medium">Inactive</span>
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

      <div class="card card-body">
        <h3>Branches ({{ listing.length }})</h3>
        <div class="table-responsive" v-show="load">
          <table
            id=""
            class="table table-striped table-bordered"
            style="width: 100%"
          >
            <thead>
              <tr>
                <th>#</th>
                <th>Name</th>
                <th>Country</th>
                <th>Created On</th>
                <th>Description</th>
                <th>Working Hours</th>
                <th>Status</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in listing" :key="item.createdDate">
                <td>{{ item.id }}</td>
                <td>{{ item.nameEn }}</td>
                <td>{{ item.countryId }}</td>
                <td>{{ getDate(item.createdDate) }}</td>
                <td>{{ item.descriptionAr }}</td>
                <td>{{ item.operationHours }}</td>
                <td>
                  <span v-if="item.isActive" class="btn btn-sm badge-success"
                    >Active</span
                  >
                  <span v-else class="btn btn-sm badge-danger">Inactive</span>
                </td>
                <td>
                  <div class="action-btn">
                    <a
                      @click="get(item.id)"
                      href="javascript:void(0)"
                      class="ms-2 btn btn-info btn-sm"
                    >
                      Edit
                    </a>
                    <a
                      @click="performAction(item.id)"
                      href="javascript:void(0)"
                      class="ms-2 btn badge-danger btn-sm"
                    >
                      Delete
                    </a>
                  </div>
                </td>
              </tr>
            </tbody>
            <tfoot>
              <tr>
                <th>#</th>
                <th>Name</th>
                <th>Country</th>
                <th>Created On</th>
                <th>Description</th>
                <th>Working Hours</th>
                <th>Status</th>
                <th>Action</th>
              </tr>
            </tfoot>
          </table>
        </div>
      </div>
      <div
        class="modal fade"
        id="addVendorModal"
        tabindex="-1"
        role="dialog"
        aria-labelledby="addVendorModalTitle"
        aria-hidden="true"
      >
        <div
          class="modal-dialog modal-dialog-centered"
          role="document"
          style="width: 80% !important; max-width: 80% !important"
        >
          <div class="modal-content">
            <div class="modal-header d-flex align-items-center">
              <h5 class="modal-title">{{ $t("Add Branches") }}</h5>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <div class="add-contact-box">
                <div class="add-contact-content">
                  <ul class="nav nav-underline" id="myTab" role="tablist">
                    <li class="nav-item" role="presentation">
                      <a
                        class="nav-link active"
                        id="active-tab"
                        data-bs-toggle="tab"
                        href="#english"
                        role="tab"
                        aria-controls="active"
                        aria-expanded="true"
                        aria-selected="true"
                      >
                        <span>English</span>
                      </a>
                    </li>
                    <li class="nav-item" role="presentation">
                      <a
                        class="nav-link"
                        id="link1-tab"
                        data-bs-toggle="tab"
                        href="#arabic"
                        role="tab"
                        aria-controls="link1"
                        aria-selected="false"
                        tabindex="-1"
                      >
                        <span>Arabic</span>
                      </a>
                    </li>
                  </ul>
                  <div
                    class="tab-content tabcontent-border py-3"
                    id="myTabContent"
                  >
                    <div
                      role="tabpanel"
                      class="tab-pane fade active show"
                      id="english"
                      aria-labelledby="active-tab"
                    >
                      <div class="row">
                        <form class="mt-4 col-12 row" novalidate="">
                          <div class="mb-3 form-group col-12">
                            <label>
                              Branch Name (EN)
                              <span class="text-danger">*</span>
                            </label>
                            <div class="controls">
                              <input
                                v-model="data.nameEn"
                                type="text"
                                name="text"
                                class="form-control"
                                required=""
                                data-validation-required-message="This field is required"
                                aria-invalid="false"
                              />
                              <div class="help-block"></div>
                            </div>
                          </div>
                          <div class="mb-3 form-group col-12">
                            <label>
                              Branch Description (EN)
                              <span class="text-danger">*</span>
                            </label>
                            <div class="controls">
                              <textarea
                                v-model="data.descriptionEn"
                                class="form-control"
                                required=""
                                data-validation-required-message="This field is required"
                                aria-invalid="false"
                              ></textarea>
                              <div class="help-block"></div>
                            </div>
                          </div>
                        </form>
                      </div>
                    </div>
                    <div
                      class="tab-pane fade"
                      id="arabic"
                      role="tabpanel"
                      aria-labelledby="link1-tab"
                    >
                      <form class="mt-4 col-12 row" novalidate="">
                        <div class="mb-3 form-group col-12">
                          <label>
                            Branch Name (JO)
                            <span class="text-danger">*</span>
                          </label>
                          <div class="controls">
                            <input
                              v-model="data.nameAr"
                              type="text"
                              name="text"
                              class="form-control"
                              required=""
                              data-validation-required-message="This field is required"
                              aria-invalid="false"
                            />
                            <div class="help-block"></div>
                          </div>
                        </div>
                        <div class="mb-3 form-group col-12">
                          <label>
                            Branch Description (JO)
                            <span class="text-danger">*</span>
                          </label>
                          <div class="controls">
                            <textarea
                              v-model="data.descriptionAr"
                              class="form-control"
                              required=""
                              data-validation-required-message="This field is required"
                              aria-invalid="false"
                            ></textarea>
                            <div class="help-block"></div>
                          </div>
                        </div>
                      </form>
                    </div>
                  </div>
                  <div class="row">
                    <div class="mb-3 form-group col-6">
                      <label>Country <span class="text-danger">*</span></label>
                      <div class="controls">
                        <select
                          v-model="data.countryId"
                          name="select"
                          id="select"
                          required=""
                          class="form-control"
                          aria-invalid="false"
                        >
                          <option value="">Select Country</option>
                          <option value="13">Algeria</option>
                          <option value="2">Jordan</option>
                        </select>
                        <div class="help-block"></div>
                      </div>
                    </div>
                    <div class="mb-3 form-group col-6">
                      <label>Status <span class="text-danger">*</span></label>
                      <div class="controls">
                        <select
                          v-model="data.isActive"
                          name="select"
                          id="select"
                          required=""
                          class="form-control"
                          aria-invalid="false"
                        >
                          <option value="">Select Status</option>
                          <option value="1">Active</option>
                          <option value="0">Inactive</option>
                        </select>
                        <div class="help-block"></div>
                      </div>
                    </div>
                    <div class="mb-3 form-group col-6">
                      <label
                        >Working Hours <span class="text-danger">*</span></label
                      >
                      <div class="controls">
                        <select
                          v-model="data.operationHours"
                          name="select"
                          id="select"
                          required=""
                          class="form-control"
                          aria-invalid="false"
                        >
                          <option value="">Select Wokring Hours</option>
                          <option selected value="1">1</option>
                          <option value="8">2</option>
                        </select>
                        <div class="help-block"></div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              <button
                id=""
                @click="submit()"
                class="btn btn-success rounded-pill px-4"
              >
                Add
              </button>
              <button
                class="btn btn-danger rounded-pill px-4"
                data-bs-dismiss="modal"
              >
                Discard
              </button>
            </div>
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
      title: "Dashboard | Branches",
      link: [
        {
          rel: "stylesheet",
          href: "https://cdn.datatables.net/1.10.20/css/dataTables.bootstrap.min.css",
        },
        {
          rel: "stylesheet",
          href: "https://fonts.googleapis.com/icon?family=Material+Icons",
        },
      ],
      script: [
        {
          src: "/assets/Modernize/vendor/fullcalendar/index.global.min.js",
          defer: true,
          body: true,
          callback: this.onScriptLoaded,
        },
      ],
    };
  },
  name: "branches",
  data() {
    return {
      dataTable: null,
      data: {
        nameAr: null,
        nameEn: null,
        descriptionAr: null,
        descriptionEn: null,
        operationHours: null,
        countryId: null,
        isActive: null,
      },
      listing: [],
      load: false,
      id: null,
      branches: [],
      link: "/branch/api/branches",
      action: "insert",
      username: "user",
      password: "123456",
    };
  },
  mounted() {
    if (!process.client) return;
    this.getAll();
  },
  component: {},
  methods: {
    filter(key, e) {
      this.load = false;
      let value = e;
      let url = this.link;
      axios
        .get(url, this.config)
        .then((response) => {
          if (response.data.success == true) {
            if (e != "all")
              this.listing = response.data.response.filter(
                (obj) => obj.isActive === value
              );
            else this.listing = response.data.response;

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
    },
    getDate(timing) {
      return moment.utc(timing).format("MMMM DD, YYYY HH:mm:ss");
    },
    async getAll() {
      $(".preloader").show();
      this.load = false;
      await axios
        .get(this.link, {
          auth: {
            username: this.username,
            password: this.password,
          },
        })
        .then((response) => {
          if (response.data.success) {
            this.listing = response.data.response;
            setTimeout(() => {
              this.load = true;
              $(".preloader").hide();
            }, 500);
          } else this.$toast.error(response.data.message).goAway(1500);
        })
        .catch((error) => {
          this.$toast.error(error.response.data.messageText).goAway(1500);
        });
    },
    performAction(id = null) {
      this.$confirm("You sure you want to delete this item?").then(() => {
        this.token = JSON.parse(localStorage.getItem("access_token"));
        const config = {
          auth: {
            username: this.username,
            password: this.password,
          },
        };
        axios
          .delete(this.link + "/" + id, config)
          .then((response) => {
            if (response.data.success) {
              this.$toast.success(response.data.message).goAway(1500);
              this.getAll();
            } else this.$toast.error(response.data.message).goAway(1500);
          })
          .catch((error) => {
            this.$toast.error(error.response.data.messageText).goAway(1500);
          });
      });
    },
    get(code) {
      const config = {
        auth: {
          username: this.username,
          password: this.password,
        },
      };
      axios
        .get(this.link + "/" + code, config)
        .then((response) => {
          if (response.data.success) {
            this.data = response.data.response;
            this.id = this.data.id;
            this.action = "update";
            $("#addVendorModal").modal("show");
          } else this.$toast.error(response.data.message).goAway(1500);
        })
        .catch((error) => {
          this.$toast.error(error.response.data.messageText).goAway(1500);
        });
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
    submit() {
      const config = {
        auth: {
          username: "user",
          password: "123456",
        },
      };
      const formData = {
        ...this.data,
        adminId: 1,
      };
      if (this.action == "insert") {
        axios
          .post(this.link, formData, config)
          .then((response) => {
            if (response.data.success == true) {
              this.$toast.success(response.data.messageText).goAway(1500);
              this.getAll();
              $("#addVendorModal").modal("hide");
            } else this.$toast.error("Error").goAway(1500);
          })
          .catch((error) => {
            if (error.response.data.success == false) {
              this.$toast.error("Error").goAway(1500);
            }
          });
      } else {
        axios
          .put(this.link + "/" + this.id, formData, config)
          .then((response) => {
            if (response.data.success == true) {
              this.$toast.success(response.data.messageText).goAway(1500);
              this.link = "/branch/api/branches";
              this.getAll();
              $("#addVendorModal").modal("hide");
            } else this.$toast.error("Error").goAway(1500);
          })
          .catch((error) => {
            if (error.response.data.success == false) {
              this.$toast.error("Error").goAway(1500);
            }
          });
      }
    },
    showModal(id) {
      $("#" + id).modal("toggle");
    },
  },
};
</script>
<style>
.upload-prescription {
  display: flex;
  position: relative;
}

.upload-prescription > p {
  font-size: 27px;
  font-family: Poppins-Medium;
  color: #171717;
  position: absolute;
  left: 4%;
  top: 50px;
}

.uploadingzone {
  border: 2px dashed rgba(41, 45, 50, 35%);
  border-radius: 13px;
  padding: 8px;
  display: flex;
}

.uploaddetails {
  position: relative;
  padding-left: 3%;
}

.icon svg {
  margin: 22px;
}

.uploaddetails progress {
  width: 80px;
  position: absolute;
  bottom: 9px;
  height: 7px;
  margin-left: 25%;
}

.uploaddetails .percentageindicator {
  margin: 0;
  position: absolute;
  bottom: 4px;
  font-size: 9px;
  font-family: Poppins-Medium;
  color: #1c1c1c;
  opacity: 100%;
  left: 13%;
}
</style>
