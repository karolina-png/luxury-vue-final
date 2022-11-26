<template>
  <h1>My Buys:</h1>
  <div id="client_buys">
    <div id="container_buys">
      <article id="sales">

        
      </article>
    </div>
  </div>

</template>
<style>
#client_buys {
  width: 100%;
  height: 100%;
}

#container_buys {
  display: flex;
  flex-wrap: wrap;
  height: max-content;
  width: 100%;
  justify-content: center;
}
</style>

<script>
export default {
  data() {
    return {
      buys_list: [],
      buys_list_show: [],
      user: {},
      search: "",
      sales_detail: []
    }
  },
  mounted() {
    // this.get_buys();
    this.user = JSON.parse(localStorage.user);
    this.search = this.user.id;
    console.log("rol: " + this.search)

    let array1 = localStorage.getItem("sales")
    this.sales_detail = JSON.parse(array1);

    this.sales()
    console.log(this.sales_detail)
  },
  methods: {
    // async get_buys() {
    //   let response = await this.axios.get("/api/sales");
    //   this.buys_list = response.data;

    //   this.buys_list_show = this.buys_list;
    //   this.buys_list_show.forEach(element => {
    //     console.log(element);

    //   });

    //   this.filtrar();
    // },
    filtrar() {

      this.buys_list_show = this.buys_list.filter(
        (s) =>
          (s.users_id.toString().toLowerCase().toString().indexOf(this.search.toString()) > -1)

      );
      console.log('whaaaat');
    },

    sales() {
      let sale = document.getElementById('sales')
      sale.innerHTML = `
      <p>${this.sales_detail[0].name}</p>
      <p>${this.sales_detail[0].article_id}</p>
      <p>${this.sales_detail[0].code}</p>
      <p>${this.sales_detail[0].iva}</p>
      <p>${this.sales_detail[0].price}</p>
      <p>${this.sales_detail[0].total}</p>
      <p>${this.sales_detail[0].id}</p>
      
      `
    }
  }
};
</script>