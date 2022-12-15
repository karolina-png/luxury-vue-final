<template>
  <div id="container_administator">
    <div id="administrator">
      <aside>
        <div>
          <router-link to="/Ventas" class="aside_art"> Sales </router-link>
          <router-link to="/Users" class="aside_art">
            <i class="bi bi-people-fill"></i></router-link>
          <router-link to="/ArticlesAdmin" class="aside_art">Articles</router-link>
        </div>

        <div>
          <button
            type="button"
            class="btn btn-secondary text-black fs-5 em"
            data-bs-toggle="modal"
            data-bs-target="#editConfig"
            data-bs-whatever="@mdo"
          >
            <i class="bi bi-gear-fill"></i>
          </button>
          <button
            class="aside_art"
            type="button"
            data-bs-toggle="offcanvas"
            data-bs-target="#offcanvasWithBothOptions"
          >

          <div id="pe">
            <i class="bi bi-person-fill"></i>
          </div>
           
          </button>
        </div>
      </aside>
      <section>
        <router-view> </router-view>
      </section>
    </div>
    <div
      class="offcanvas offcanvas-start bg-dark"
      data-bs-scroll="true"
      tabindex="-1"
      id="offcanvasWithBothOptions"
      aria-labelledby="offcanvasWithBothOptionsLabel"
    >
      <div class="offcanvas-header">
        <h5 class="offcanvas-title" id="offcanvasWithBothOptionsLabel">
          LUXURY SEX-SHOP
        </h5>
        <button
          type="button"
          class="btn-close"
          data-bs-dismiss="offcanvas"
          aria-label="Close"
        ></button>
      </div>
      <div class="offcanvas-body">
        <div id="user_data">
          <h1>MY PERFIL</h1>
          <h2>{{ role[user.roles_id] }}</h2>
          <p>NOMBRE: {{ user.name }}</p>
          <p>EMAIL: {{ user.email }}</p>

          <button @click="logout()"><i class="bi bi-box-arrow-right"></i></button>
        </div>
      </div>
    </div>
  </div>
  <!-- Toast -->
  <div class="toast-container position-fixed bottom-0 p-3">
    <div
      class="toast text-bg-success border-0"
      role="alert"
      aria-live="assertive"
      aria-atomic="true"
    >
      <div class="d-flex">
        <div class="toast-body">
          {{ alert }}
        </div>
        <button
          type="button"
          class="btn-close btn-close-white me-2 m-auto"
          data-bs-dismiss="toast"
          aria-label="Close"
        ></button>
      </div>
    </div>
  </div>

  <!-- Button trigger modal -->


 
      
   
  
</template>

<style scoped>
@import url(../assets/administrator.css);

#pe{
 padding: 1.2rem;
}
</style>

<script>
export default {
  data() {
    return {
      token: null,
      config: {
        iva_percent: 0,
        shipping_price: 0,
      },
      toast: null,
      alert: "",
      modal: null,
      errors: {},
      user: {},
      user_edit: {},
      role: ["ajam", " Client", " Employee", " Administrator"],
    };
  },

  mounted() {
    this.$router.push("ArticlesAdmin");
    this.get_iva();

    if (localStorage.token) {
      this.token = localStorage.token;
      this.user = JSON.parse(localStorage.user);
      console.log("Admin rol: " + this.user.roles_id);
      if (this.user.roles_id === 3) {
        this.token = localStorage.token;
        this.user = JSON.parse(localStorage.user);
        // this.token = localStorage.token;
        // this.get_user();
      } else {
        this.token = "";
        this.user = "";

        this.$router.push({
          name: "Login",
          params: {
            message: "No estas autorizado para acceder a esta cuenta",
          },
        });
      }
    } else {
      this.$router.push({
        name: "Login",
        params: {
          message: "No estas autorizado para acceder a esta cuenta",
        },
      });
    }
  },
  methods: {
    async logout() {
      try {
        console.log(this.token);
        const rs = await this.axios.get("/api/logout", {
          headers: { Authorization: `Bearer ${this.token}` },
        });

        localStorage.clear();

        this.$router.push({
          name: "Login",
          params: {
            message: rs.data.message,
          },
        });
      } catch (e) {
        this.$router.push({
          name: "Login",
          params: {
            message: e.response.data.message,
          },
        });
      }
    },
    edit_user(p) {
      this.user_edit = p;
      // console.log(this.articles_edit);
    },

    //Manage bootstrap modals and toast
    prepare_elements() {
      const myModal = document.getElementById('editConfig'); //Nombre del modal
      const myAlert = document.querySelector(".toast");
      this.modal = bootstrap.Modal.getInstance(myModal);
      this.toast = new bootstrap.Toast(myAlert);
    },

    async update_iva() {
      console.log(this.config);

      this.prepare_elements();
      try {
        let res = await this.axios.put("/api/config/1", this.config);
        console.log(res.data);
        this.alert = res.data.message;
        this.get_iva();
        this.modal.hide();
        this.toast.show();
      } catch (e) {
        this.manage_error_messages(e);
      }
    },

    async get_iva() {
      try {
        let response = await this.axios.get("/api/config");
        this.config = response.data.config;
      } catch (e) {
        this.manage_error_messages(e);
      }
    },
  },
};
</script>
