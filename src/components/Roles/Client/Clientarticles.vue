<template>
  <div id="container_clinarts">
    <div id="contcar">
      <input
        name="buscar articulo"
        type="text"
        id="buscar"
        v-model="search"
        @keyup="filtrar()"
        placeholder="search"
      />
      <label id="buycar" for="car"><i class="bi bi-cart-plus"></i></label>
      <input type="checkbox" name="" id="car" />
      <div class="carrito">
        <carhead> Factura de venta </carhead>
        <carbody>
          <div id="container_carbody">
            <table class="table table-hover">
              <thead>
                <tr>
                  <th>NOMBRE</th>
                  <th>PRECIO</th>
                  <th>CODIGO</th>
                  <th>CANTIDAD</th>
                  <th>SUBTOTAL</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(a, index) in sales.articles"
                  :key="'article' + a.id"
                >
                  <td>{{ a.name }}</td>

                  <td>${{ a.price }}</td>
                  <td>{{ a.code }}</td>
                  <td>
                    <span @click="restar(a)"
                      ><i class="bi bi-file-minus-fill"></i
                    ></span>
                    <span>{{ a.amount }}</span>
                    <span @click="sumar(a)"
                      ><i class="bi bi-file-plus-fill"></i
                    ></span>
                  </td>

                  <td>${{ a.total }}</td>

                  <td>
                    <span @click="eliminar(index)"
                      ><i class="bi bi-trash"></i
                    ></span>
                  </td>

                  <span @click="a.stock"></span>
                </tr>
              </tbody>
            </table>
          </div>
        </carbody>

        <carfooter>
          <button id="pay">
            <span id="price">
              <p>${{ sales.total }}</p>
            </span>

            <span id="sale" @click="pagar()"> PAGAR </span>
          </button>
        </carfooter>
      </div>
    </div>
    <div id="Client_articles">
      <div class="cont-art">
        <article
          v-for="a in articles_list_mostrar"
          class="articles"
          @click="insert(a)"
          :key="'article_list' + a.id"
        >
          <div class="face front">
            <figure>
              <img v-if="a.image != null" :src="axios.defaults.baseURL + a.image" alt="" />
              <img v-if="a.image == null" src="public/uploads/default.jpg" alt="" />
              
            </figure>

            <footer>
              <p>
                {{ a.name }}
              </p>
              <p>Precio:{{ a.selling_price }}</p>
            </footer>
          </div>

          <div class="face back">
            <p>{{ a.description }}</p>
          </div>
        </article>
      </div>
    </div>
  </div>
  <div id="bar">
    <div>
      <router-link to="/Buys" class="btn buton"
        ><i class="bi bi-cart-check-fill"></i>
        Mis compras
      </router-link>
      <router-link to="/Clientarticles" class="btn buton"
        ><i class="bi bi-cart4"></i>
        Artículos
      </router-link>
      <button
        class="btn buton"
        type="button"
        data-bs-toggle="offcanvas"
        data-bs-target="#offcanvasWithBothOptions"
      >
        <i class="bi bi-person-circle"></i>
        Usuario
      </button>
    </div>
  </div>
</template>

<style>
.table {
  text-align: center;
}

/*#price {
 
  font-size: 1rem !important;
  
}*/

#container_clinarts {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

#Client_articles {
  width: 100%;
  height: 100%;
  overflow-y: scroll;
}

.cont-art {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

#contcar {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem;
}

#buycar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 2rem;
  width: 3rem;
  background-color: rgb(255, 255, 255);
  border-radius: 0.5rem;
  text-align: center;
  z-index: 11;
  color: rgb(58, 5, 128);
}

#car {
  display: none;
}

.carrito {
  display: none;
  flex-direction: column;
  height: 75vh;
  width: 30rem;
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background-color: rgb(255, 255, 255);
  border-radius: 0.5rem;
  z-index: 10;
  animation: carrito 200ms;
  transition: ease-in-out 10ms;
  padding: 0.5rem;
}

carhead {
  height: 10%;
  border-bottom: 1px solid blueviolet;
  color: black;
  font-size: xx-large;
}

carbody {
  height: 80%;
  width: 100%;
  /* border-bottom: 1px solid blueviolet; */
  color: black;
  overflow-y: scroll;
}

#container_carbody {
  display: flex;
  flex-direction: column;
  height: max-content;
  width: 90%;
}

.carticle {
  border-bottom: 1px solid blueviolet;
}

carfooter {
  /* display: flex;
    justify-content: center; */
  height: 10%;
  width: 100%;
  color: black;
  /* padding: 0.2rem; */
}

carfooter button {
  display: flex;
  justify-content: space-between;
  background-color: rgb(69, 5, 121) !important;

  height: 80% !important;
  width: 96% !important;
  border-radius: 0.7rem;
  padding: 0.5rem;
}

carfooter div:active {
  transform: scale(0.98);
  background-color: rgb(92, 17, 153);
}

#sale {
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: white;
  font-size: xx-large;
  border-radius: 0.5rem;
  /* background-color: blueviolet; */
  width: 30%;
}
#bar {
  position: absolute;
  bottom: 0;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  height: 3.5rem;
  width: 100%;
  background-color: rgb(102, 27, 187);
  
  padding: 0.5rem;
}

#price {
  display: flex;
  flex-direction: column;
  width: 50em;
  color: white;
  font-size: xx-large;

  /* background-color: blueviolet; */
  width: 40%;
}

.articles {
  position: relative;
}

.face {
  position: absolute;
  backface-visibility: hidden;
}

.front {
  display: flex;
  flex-direction: column;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  padding: 0.5rem;
  /* border: 1px solid red; */
  transform: perspective(600px) rotatey(0);
  transition: ease-in-out 300ms;
}

figure {
  display: flex;
  flex-direction: column;
  height: 70%;
  width: 100%;
}

img {
  border-radius: 0.5rem;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.back {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  width: 100%;
  height: 100%;
  /* position: absolute; */
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.466);
  padding: 0.5rem;
  /* border: 1px solid red; */
  border-radius: 1rem;
  transition: ease-in-out 300ms;
  transform: perspective(600px) rotateY(180deg);
}

.articles:hover .front {
  transform: perspective(600px) rotateY(180deg);
}

.articles:hover .back {
  transform: perspective(600px) rotateY(360deg);
}

@keyframes carrito {
  0% {
    width: 5rem;
    height: 10rem;
  }

  5% {
    width: 7rem;
    height: 12rem;
  }

  10% {
    width: 8rem;
    height: 15rem;
  }

  15% {
    width: 9rem;
    height: 17rem;
  }

  20% {
    width: 10rem;
    height: 20rem;
  }

  25% {
    width: 11rem;
    height: 23rem;
  }

  30% {
    width: 12rem;
    height: 25rem;
  }

  35% {
    width: 13rem;
    height: 26rem;
  }

  40% {
    width: 14rem;
    height: 27rem;
  }

  50% {
    width: 15rem;
    height: 30rem;
  }

  60% {
    width: 17rem;
    height: 33rem;
  }

  70% {
    width: 20rem;
    height: 36rem;
  }

  75% {
    width: 23rem;
    height: 37rem;
  }

  80% {
    width: 25rem;
    height: 38rem;
  }

  90% {
    width: 28rem;
    height: 40rem;
  }

  100% {
    width: 30rem;
    height: 44rem;
  }
}

.select {
  display: none;
}

#car:checked ~ .carrito {
  display: flex;
}

@media (max-width: 500px) {
  .carrito {
    width: 96%;
  }
}
</style>
<script>
export default {
  data() {
    return {
      user: {},
      articles_list: [],
      articles_list_mostrar: [],
      search: "",
      cont: 1,
      //venta actual
      sales: {
        users_id: null,
        date: null,
        sales_number: null,
        state: 0, //factura abierta
        total: 0,
        articles: [], //articulos del carrito
      },
    };
  },
  mounted() {
    if (localStorage.user) {
      this.user = JSON.parse(localStorage.user);
    } else {
      //Se va para el login
    }
    this.get_products();
    // this.total =  this.total+sales.articles.price
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
    async get_products() {
      let response = await this.axios.get("/api/articles");
      this.articles_list = response.data;
      this.articles_list_mostrar = this.articles_list;

      // this.get_images();
    },
    // async get_images(){
    //     let id=126;
    //      let response = await this.axios.get("/api/articles_images/");
    //      console.log(response.data);
    // },

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
            .indexOf(this.search.toLowerCase()) > -1
      );
    },

    insert(a) {
      let item = this.sales.articles.find((art) => art.id == a.id);
      console.log(item);
      if (item == undefined) {
        let iva = a.selling_price / 1.19;
        iva = Number(iva.toFixed(2)); //redondeamos a 2 decimales

        let article = {
          id: a.id,
          name: a.name,
          code: a.code,
          stock: a.stock,
          article_id: a.id,
          users_id: this.user.id,
          amount: 1,
          price: a.selling_price,
          subtotal: a.selling_price - iva,
          iva: iva, //iva desde la tabla config
          total: a.selling_price,
        };
        this.sales.articles.push(article);
        this.sales.total += article.price;
      }
    },

    sumar(a) {
      let item = this.sales.articles.find((article) => article.id == a.id);

      if (item.amount < item.stock) {
        item.amount++;
        item.total = item.amount * item.price;
        item.iva = item.total / 1.19; //iva desde la tabla config
        item.subtotal = item.total - item.iva;
        this.sales.articles.total += item.price;
        this.sales.total += item.price;
      }
    },

    restar(a) {
      let item = this.sales.articles.find((article) => article.id == a.id);

      if (item.amount > 1) {
        item.amount--;
        item.total = item.amount * item.price;
        item.iva = item.total / 1.19; //iva desde la tabla config
        item.subtotal = item.total - item.iva;
        this.sales.articles.total += item.price;
        this.sales.total -= item.price;
      }
    },

    eliminar(index) {
      let item = this.sales.articles[index];
      this.sales.total -= item.total;
      this.sales.articles.splice(index, 1);
    },

    pagar() {
      localStorage.setItem("sales", JSON.stringify(this.sales.articles));
    },
  },
};
</script>
