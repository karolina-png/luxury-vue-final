<template>
   <div id="container-sales">
     <h1>VENTAS:</h1>
     <div id="con_buton_sale">
       <button @click="filtrar(0)">En proceso</button>
       <button @click="filtrar(1)">Compra finalizada</button>
       <button @click="filtrar(2)">Producto enviado</button>
       <button @click="filtrar(3)">Producto entregado</button>
       <!-- <button
         type="button"
         class="btn btn-secondary text-black fs-5 em"
         data-bs-toggle="modal"
         data-bs-target="#editConfig"
         data-bs-whatever="@mdo"
       >
         Config
       </button> -->
     </div>
 
     <div id="sales">
       <div id="container_sales">
         <article v-for="s in sales_list_show" :key="'sales' + s.id">
           <p>Date: {{ s.date }}</p>
 
           <p>invoice number: {{ s.number }}</p>
           <p>User: {{ s.user}}</p>
           <p>Article: {{ s.articles }}</p>
           <p>State: {{ s.state }}</p>
           <p>Total: {{ s.total }}</p>
         </article>


       </div>
     </div>
   </div>
 
   <!-- Toast
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
 
   Button trigger modal
   <button
     type="button"
     class="btn btn-primary"
     data-bs-toggle="modal"
     data-bs-target="#editConfig"
   >
     Launch demo modal
   </button>
  -->
   <!-- Modal
   <div
     class="modal fade"
     id="editConfig"
     tabindex="-1"
     aria-labelledby="exampleModalLabel"
     aria-hidden="true"
   >
     <div class="modal-dialog">
       <div class="modal-content">
         <div class="modal-header">
           <h5 class="modal-title" id="exampleModalLabel">Modificar Iva</h5>
           <button
             type="button"
             class="btn-close"
             data-bs-dismiss="modal"
             aria-label="Close"
           ></button>
         </div>
         <div class="modal-body">
           <div class="mb-3 row">
             <label for="iva" class="col-sm-2 col-form-label">Iva (%)</label>
             <div class="col-sm-10">
               <input
                 type="text"
                 class="form-control-plaintext"
                 id="iva"
                 v-model="config.iva_percent"
               />
               <div class="form-text" v-if="errors.iva_percent">
                 {{ errors.iva_percent[0] }}
               </div>
             </div>
           </div>
 
           <div class="mb-3 row">
             <label for="shipping_price" class="col-sm-2 col-form-label"
               >Domicilio</label
             >
             <div class="col-sm-10">
               <input
                 type="text"
                 class="form-control-plaintext"
                 id="shipping_price"
                 v-model="config.shipping_price"
               />
               <div class="form-text" v-if="errors.shipping_price">
                 {{ errors.shipping_price[0] }}
               </div>
             </div>
           </div>
         </div>
 
         <div class="modal-footer">
           <button
             type="button"
             class="text-black"
             @click="update_iva()"
           >
             actualizar
           </button>
         </div>
       </div>
     </div>
   </div>
  -->
   
 </template>
 <style>
 #container-sales {
   width: 100%;
   height: 100%;
 }
 
 #sales {
   padding: 1rem;
   width: 100%;
   height: 100%;
   overflow-y: scroll;
 }
 
 #container_sales {
   display: flex;
   flex-wrap: wrap;
   height: max-content;
 }
 #con_buton_sale {
   display: flex;
   flex-direction: row;
   justify-content: end;
 }
 button {
   margin: 0.5rem;
 }
 </style>
 <script>
 export default {
   data() {
     return {
       sales_list: [],
       sales_list_show: [],
       search: 0,
      //  config: {
      //    iva_percent: 0,
      //    shipping_price: 0,
      //  },
       toast: null,
       alert: "",
       modal: null,
       errors: {},
     };
   },
   mounted() {
     this.get_sales();
    //  this.get_iva();
   },
   methods: {
     //Manage bootstrap modals and toast
     prepare_elements() {
       const myModal = document.getElementById("editConfig"); //Nombre del modal
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
 
     async get_sales() {
       let response = await this.axios.get("/api/sales");
       this.sales_list = response.data;
 
       this.sales_list_show = this.sales_list;
       // this.sales_list_show.forEach(element => {
       //    console.log(element);
 
       // });
       this.filtrar(0);
     },
 
    //  async update_iva() {
    //    console.log(this.config);
 
    //    this.prepare_elements();
    //    try {
    //      let res = await this.axios.put("/api/config/1", this.config);
    //      console.log(res.data)
    //      this.alert = res.data.message;
    //      this.get_iva();
    //      this.modal.hide();
    //      this.toast.show();
    //    } catch (e) {
    //      this.manage_error_messages(e);
    //    }
    //  },
 
    //  async get_iva() {
    //    try {
    //      let response = await this.axios.get("/api/config");
    //      this.config = response.data.config;
    //    } catch (e) {
    //      this.manage_error_messages(e);
    //    }
    //  },
 
     filtrar(p) {
       this.search = p;
       console.log(this.search);
       this.sales_list_show = this.sales_list.filter(
         (s) =>
           s.state
             .toString()
             .toLowerCase()
             .toString()
             .indexOf(this.search.toString()) > -1
       );
       console.log("whaaaat");
     },
   },
 };
 </script>
 