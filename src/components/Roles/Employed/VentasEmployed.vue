<template>
  <div id="container-ventas">
    <h1>VENTAS:</h1>
    <div id="con_buton_sale">
      <button @click="filtrar(0)"><i class="bi bi-cart"></i></button>
      <button @click="filtrar(1)"><i class="bi bi-cart-check-fill"></i></button>
      <button @click="filtrar(2)"><i class="bi bi-airplane-engines-fill"></i></button>
      <button @click="filtrar(3)"><i class="bi bi-send-plus-fill"></i></button>
    </div>
    <div id="ventas">
      <table class="table table-hover table-dark ventas">
        <thead>
          <tr>
            <th scope="col"><B>ID</B></th>
            <th scope="col"><b>CODIGO DEL ARTICULO</b></th>
            <th scope="col"><b>USUARIO</b></th>
            <th scope="col"><b>FECHA</b></th>
            <th scope="col"><b>NUMERO DE LA VENTA</b></th>
            <th scope="col"><b>ESTADO DE LA VENTA</b></th>
            <th scope="col"><b>TOTAL</b></th>
            <th scope="col"><b></b></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="s in sales_list_show">

            <td>{{ s.id }}</td>
            <td>{{ s.code }}</td>
            <td>{{ s.user }}</td>
            <td>{{ s.created_at }}</td>
            <td>{{ s.number }}</td>
            <td>{{ s.state }}</td>
            <td>{{ s.total }}</td>
            <td>
              <router-link to="/factura">
                <button type="button" class="btn btn-primary"  @click="datos()">
                  <i class="bi bi-arrow-down-square-fill" id="desc"></i>
                </button>
              </router-link>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <!-- Button trigger modal -->

  
  <!-- Modal -->
  <div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">FACTURA</h1>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body" id="factura-mmodal"></div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
          <button type="button" class="btn btn-primary">Save changes</button>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
#container-ventas {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  color: white;
}

#ventas {
  padding: 1rem;
  width: 100%;
  height: 100%;
  overflow-y: scroll;
  /* border-radius: 10%; */
  /* border: 1px solid red; */
  border-radius: 1rem;
}

.ventas {
  border-radius: 1rem;
}

#desc{
  background-color: blueviolet;
  width: 16rem;
}
</style>
<script>
export default {
  data() {
    return {
      sales_list: [],
      sales_list_show: [],
      search: "",
    };
  },
  mounted() {
    this.get_sales();
  },
  methods: {
    async get_sales() {
      let response = await this.axios.get("/api/sales");
      this.sales_list = response.data;

      this.sales_list_show = this.sales_list;
      this.sales_list_show.forEach((element) => {
        // console.log(element);
      });
      this.filtrar(0);
    },
    filtrar(p) {
      this.search = p;
      console.log(this.search);
      this.sales_list_show = this.sales_list.filter(
        (s) =>
          s.state.toLowerCase().toString().indexOf(this.search.toString()) > -1
      );
    },

    datos(sales) {
      
      let item = this.sales_list_show.find((p) => p.sales_number == sales);
      console.log(item)
      localStorage.setItem('items', JSON.stringify(item))
    },
  },
};
</script>
