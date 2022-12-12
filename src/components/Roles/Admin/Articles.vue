<template>
  <div id="container-articles">
    <div id="search">
      <div id="container-search">
        <button
          type="button"
          class="btn btn-secondary text-black fs-5 em"
          data-bs-toggle="modal"
          data-bs-target="#createArticle"
          data-bs-whatever="@mdo"
        >
          <i class="bi bi-plus-circle"></i>
        </button>
        <input
          name="buscar articulo"
          class="em"
          type="text"
          id="buscar"
          v-model="search"
          @keyup="filtrar()"
          placeholder="Search"
        />
      </div>
    </div>

    <div id="articles">
      <div id="sect_art">
        <article
          v-for="p in articles_list_mostrar"
          class="articles"
          :key="'article' + p.id"
        >
          <div class="face front">
            <figure>
              <img :src="p.image" alt="" />
            </figure>

            <p>Nombre: {{ p.name }}</p>
            <p>Precio: {{ p.selling_price }}</p>
          </div>

          <div class="face back">
            <div>
              <p>Codigo: {{ p.code }}</p>
              <p>Categori: {{ p.categories_id }}</p>
              <p>Descripcion: {{ p.description }}</p>
              <p>Stock: {{ p.stock }}</p>
            </div>

            <footer>
              <button
                type="button"
                class="text-black"
                data-bs-toggle="modal"
                data-bs-target="#editArticle"
                @click="edit_article(p)"
              >
                <i class="bi bi-pencil-square"></i>
              </button>
              <button @click="borrar(p.id)">
                <i class="bi bi-trash3-fill"></i>
              </button>
            </footer>
          </div>
        </article>
      </div>
    </div>

    <!-- Toast -->
    <div class="toast-container position-fixed bottom-0 p-3">
      <div
        id="liveToast"
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

    <!-- Create modal -->
    <div
      class="modal fade"
      id="createArticle"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-3 text-black" id="exampleModalLabel">
              CREAR ARTICULO
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body formnew">
            <form id="formnew" enctype="multipart/form-data">
              <!-- Image management -->
              <section class="photo-container">
                <div class="photo-prev">
                  <input
                    type="file"
                    id="new-article-input"
                    @change="show_image"
                    style="display: none"
                  />
                  <!-- Image client exist and is not loading a new image -->
                  <div class="preview" v-if="articles.url && !loading">
                    <span
                      class="material-symbols-outlined clear-image"
                      @click="clear_image('new-article-input')"
                    >
                      close
                    </span>
                    <img
                      @click="open_browser('new-article-input')"
                      :src="articles.url"
                    />
                  </div>
                  <!-- Image client not exist and is not loading a new image -->
                  <span
                    v-if="!articles.url && !loading"
                    class="material-symbols-outlined"
                    @click="open_browser('new-article-input')"
                  >
                    account_circle
                  </span>

                  <div
                    v-if="loading"
                    class="loading"
                    @click="open_browser('new-article-input')"
                  ></div>
                  <!-- User can stop the image loading -->
                  <span
                    v-if="loading"
                    class="image_text"
                    :class="{ stop: loading }"
                    @click="stop_loading()"
                    @mouseover="image_text = 'Stop loading!'"
                    @mouseleave="image_text = 'Loading...'"
                    >{{ image_text }}</span
                  >
                  <span v-if="!loading" class="image_text">TU IMAGEN</span>
                </div>
                <div class="form-text" v-if="errors.image">
                  {{ errors.image[0] }}
                </div>
              </section>
              <!-- Image management -->

              <div class="mb-2">
                <label for="message-text" class="col-form-label text-black"
                  >Nombre:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="recipient-name"
                  v-model="articles.name"
                />
              </div>
              <div class="mb-2">
                <label for="categori" class="col-form-label text-black"
                  >Categoria:
                </label>
                <select
                  class="form-select"
                  aria-label="Default select example"
                  v-model="articles.categories_id"
                >
                  <option
                    v-for="item in categories"
                    :key="'categories' + item.id"
                    :value="item.id"
                  >
                    {{ item.name }}
                  </option>
                </select>
              </div>
              <div class="mb-2">
                <label for="code" class="col-form-label text-black"
                  >Codigo:
                </label>
                <input
                  type="text"
                  class="form-control"
                  id="code"
                  v-model="articles.code"
                />
              </div>
              <div class="mb-2">
                <label for="price" class="col-form-label text-black"
                  >Precio:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="price"
                  v-model="articles.selling_price"
                />
              </div>
              <div class="mb-2">
                <label for="stock" class="col-form-label text-black"
                  >Stock:
                </label>
                <input
                  type="text"
                  class="form-control"
                  id="stock"
                  v-model="articles.stock"
                />
              </div>
              <div class="mb-2">
                <label for="description" class="col-form-label text-black"
                  >Descripcion:
                </label>
                <input
                  type="text"
                  class="form-control"
                  id="description"
                  v-model="articles.description"
                />
              </div>
              <div class="mb-2">
                <div class="form-check form-switch">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    role="switch"
                    id="flexSwitchCheckChecked"
                    v-model="articles.active"
                  />
                  <label class="form-check-label" for="flexSwitchCheckChecked"
                    >The Article is active</label
                  >
                </div>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="text-black"
              data-bs-dismiss="modal"
              @click="new_article()"
            >
              CREAR
            </button>
          </div>
        </div>
      </div>
    </div>

    <div
      class="modal fade"
      id="editArticle"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-3 text-black" id="exampleModalLabel">
              Edit Article
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body formnew">
            <form id="formnew" enctype="multipart/form-data">
              <div class="mb-2">
                <label for="image" class="col-form-label text-black"
                  >Imagen:
                </label>
                <input
                  type="file"
                  class="form-control"
                  id="image"
                  @change="get_image"
                />
              </div>
              <figure>
                <img
                  v-if="imagenminiatura !== null"
                  :src="imagen"
                  width="50%"
                  alt=""
                />
                <img
                  v-if="imagenminiatura == null"
                  :src="articles_edit.image"
                  width="50%"
                  height="20%"
                  alt=""
                />
              </figure>

              <div class="mb-2">
                <label for="message-text" class="col-form-label text-black"
                  >Name:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="recipient-name"
                  v-model="articles_edit.name"
                />
              </div>
              <div class="mb-2">
                <label for="categori" class="col-form-label text-black"
                  >Categori:
                </label>
                <input
                  type="text"
                  class="form-control"
                  id="categori"
                  v-model="articles_edit.categories_id"
                />
              </div>
              <div class="mb-2">
                <label for="code" class="col-form-label text-black"
                  >Code:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="code"
                  v-model="articles_edit.code"
                />
              </div>
              <div class="mb-2">
                <label for="price" class="col-form-label text-black"
                  >Price:
                </label>
                <input
                  type="text"
                  class="form-control"
                  id="price"
                  v-model="articles_edit.selling_price"
                />
              </div>
              <div class="mb-2">
                <label for="stock" class="col-form-label text-black"
                  >Stock:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="stock"
                  v-model="articles_edit.stock"
                />
              </div>
              <div class="mb-2">
                <label for="description" class="col-form-label text-black"
                  >Description:
                </label>
                <input
                  type="text"
                  class="form-control"
                  id="description"
                  v-model="articles_edit.description"
                />
              </div>
              <div class="mb-2">
                <label for="active" class="col-form-label text-black"
                  >Active:</label
                >
                <div class="form-check form-switch">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    role="switch"
                    id="flexSwitchCheckChecked"
                    v-model="articles_edit.active"
                  />
                  <label class="form-check-label" for="flexSwitchCheckChecked"
                    >Checked switch checkbox input</label
                  >
                </div>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="text-black"
              data-bs-dismiss="modal"
              @click="update_article()"
            >
              Edit
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* @import url("../assets/administerarticles.css"); */

@import "@/assets/images.css";

#container-articles {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 100%;
}

.form-switch {
  margin-left: 5px;
}

.form-select {
  padding: 3px !important;
  margin-left: 7px;
}

#articles {
  height: calc(100% - 5rem);
  width: 100%;
  overflow-y: scroll !important;
  overflow-x: hidden !important;
}

#sect_art {
  display: flex;
  flex-wrap: wrap;
  height: max-content;
  justify-content: center;
  background-color: rgba(19, 17, 20, 0.438);
  border-radius: 1rem;
  margin: 2rem;
  padding: 2rem;
}

.articles {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  width: 20rem;
  height: 20rem;
  margin: 1rem;
  padding: 1rem;
  border-radius: 1rem;
  /* border: 1px solid rgb(92, 0, 128); */
  color: white;
}

footer {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

#search {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 5rem;
  width: 100%;
  padding: 0.5rem;
  /* border: 1px solid red; */
}

#container-search {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  /* border: 1px solid red; */
}

#buscar {
  width: 10rem;
  height: 2.2rem !important;
  background-color: rgba(255, 255, 255, 0);
  border: 1px solid;
  color: white;
  backdrop-filter: blur(5px);
}

#buscar:focus-visible {
  background-color: rgba(55, 9, 82, 0.479);
  width: 20rem;
  height: 20rem;
}

#buscar::placeholder {
  color: white;
}

input {
  margin: 0.5rem;
}

.formnew {
  height: 40rem;
  overflow-y: scroll;
  overflow-x: hidden;
}

#formnew {
  display: flex;
  flex-direction: column;
  height: max-content;
}

@media (max-width: 400px) {
  .em {
    display: block;
  }
}
</style>

<script>
export default {
  data() {
    return {
      categories: [],
      articles_list: [],
      articles_list_mostrar: [],
      search: "",
      articles_copy: {},
      articles: {
        image: "0",
        name: "",
        categories_id: "",
        code: "",
        selling_price: "",
        stock: "",
        description: "",
        active: true,
        url: null,
        preview: null,
        updated: null, //backend action
      },
      imagenminiatura: null,
      articles_edit: {},

      //images
      alert: "",
      loading: false,
      image_text: "Loading image...",
      client_copy: {},
      modal: null,
      toast: null,
      errors: {},
    };
  },
  mounted() {
    this.get_products();
    this.get_categories();

    // this.new_article();
  },

  methods: {
    //Manage bootstrap modals and toast
    prepare_elements(name) {
      const myModal = document.getElementById(name); //Nombre del modal
      const myAlert = document.querySelector(".toast");
      this.modal = bootstrap.Modal.getInstance(myModal);
      this.toast = new bootstrap.Toast(myAlert);
    },
    //Manage errors
    manage_error_messages(e) {
      console.log(e);
      this.errors = {};
      if (e.response.data.errors) this.errors = e.response.data.errors;
      else if (e.response.data.message == "Unauthenticated.") {
        this.$router.push({
          name: "Login",
          params: {
            message: "Tu sessión ha expirado, por favor intentalo de nuevo",
          },
        });
        this.modal.hide();
        this.toast.show();
      }
    },

    async get_categories() {
      let response = await this.axios.get("/api/categories");
      this.categories = response.data;
    },

    async get_products() {
      
      let response = await this.axios.get("/api/articles");
      this.articles_list = response.data;
      this.articles_list_mostrar = this.articles_list;
    },

    async new_article() {
      let response = await this.axios
        .post("/api/articles/", this.articles, {
          headers: {
            // Authorization: "Bearer " + localStorage.token,
            "Content-Type": "multipart/form-data", //Permite enviar imágenes
          },
        })
        .then((response) => {
          console.log("Ehh:" + response.data);
        });

      this.get_products();

      // console.log(this.articles);
      // let response = await this.axios.post("/api/articles/", this.articles)
      // .then(response => {
      //     console.log("ehh:"+response.data);
      //   });
      // this.get_products();
      this.articles = "";
      this.imagenminiatura = "";
    },

    edit_article(p) {
      this.articles_edit = p;
      this.articles.preview = false;
      this.articles.updated = null;
      this.articles.url = this.articles.image
        ? this.axios.defaults.baseURL + this.articles.image
        : null;
      this.articles_copy = Object.assign({}, this.articles);

      this.image_text = "You profile photo";
      this.loading = false;
    },

    async update() {
      // this.articles_edit.image="image";
      // console.log("Image: "+this.articles_edit.image)
      let formData_edit = new FormData();
      formData_edit.append("image", this.articles_edit.image);
      formData_edit.append("name", this.articles_edit.name);
      formData_edit.append("categories_id", this.articles_edit.categories_id);
      formData_edit.append("code", this.articles_edit.code);
      formData_edit.append("selling_price", this.articles_edit.selling_price);
      formData_edit.append("stock", this.articles_edit.stock);
      formData_edit.append("description", this.articles_edit.description);
      formData_edit.append("active", this.articles_edit.active);

      console.log("Editar: " + formData_edit);
      let id = this.articles_edit.id;
      console.log("Id: " + id);
      await this.axios
        .put("/api/articles/" + id, formData_edit)
        .then((response) => {
          console.log("ehh:" + response);
        });

      this.get_products();
      this.articles_edit = "";

      // console.log("Articles: "+ this.articles_edit);

      // let id = this.articles_edit.id;

      // await this.axios.put("/api/articles/" + id, this.articles_edit);
      // this.get_products()
      // this.articles_edit = "";
    },

    async borrar(id) {
      console.log(id);

      if (confirm("seguro de eliminar producto")) {
        let response = await this.axios.delete("/api/articles/" + id);
        console.log("se borro(?)" + response.data);
        this.get_products();
      }
    },

    filtrar() {
      this.articles_list_mostrar = this.articles_list.filter(
        (p) =>
          p.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1 ||
          p.code.toString().toLowerCase().indexOf(this.search.toLowerCase()) >
            -1 ||
          p.description.toLowerCase().indexOf(this.search.toLowerCase()) > -1 ||
          p.selling_price
            .toString()
            .toLowerCase()
            .indexOf(this.search.toLowerCase()) > -1 ||
          p.stock
            .toString()
            .toLowerCase()
            .toString()
            .indexOf(this.search.toLowerCase()) > -1 ||
          p.categories_id
            .toString()
            .toLowerCase()
            .toString()
            .indexOf(this.search.toLowerCase()) > -1
      );
    },

    cancel_form() {
      Object.assign(this.articles, this.articles_copy);
      this.loading = false;
      this.articles.updated = null;
    },

    open_browser(input_name) {
      const input = document.getElementById(input_name);
      input.click();
      this.loading = true;
      this.articles.updated = null;
      this.image_text = "Loading...";
    },

    show_image(e) {
      if (e.target.files[0]) {
        console.log("updated!");
        this.articles.updated = true;

        this.articles.image = e.target.files[0];
        this.articles.url = URL.createObjectURL(e.target.files[0]);
      } else {
        console.log("No se seleccionó ninguna imagen!!");
        this.articles.url = this.articles_copy.url;
      }

      this.loading = false;
    },

    clear_image(input_name) {
      this.articles.image = null;
      this.articles.updated = true;
      this.articles.url = null;
      document.getElementById(input_name).value = null; //clear input file
    },

    stop_loading() {
      console.log("cancelaste la carga!!");
      this.articles.url = this.articles_copy.url;
      this.loading = false;
    },
  },
};
</script>
