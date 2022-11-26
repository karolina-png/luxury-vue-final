<style scoped>
@import "assets/css/main.css";
</style>

<template>
  <div id="fact">
    <div class="control-bar">
      <div class="container">
        <div class="row">
          <div class="col-2-4">
            <div class="slogan">FACTURACION</div>

            <label for="config_tax">IVA:
              <input type="checkbox" id="config_tax" />
            </label>
            <label for="config_tax_rate" class="taxrelated">TASA: <input type="text" id="config_tax_rate" value="13" />%
            </label>
            <label for="config_note">NOTA:
              <input type="checkbox" id="config_note" />
            </label>
          </div>
            <div class="col-4 text-right">
            <a href="javascript:window.print()" id="paint">IMPRIMIR</a>
          </div>
        
          <!--.col-->
        </div>
        <!--.row-->
      </div>
      <!--.container-->
    </div>
    <!--.control-bar-->

    <header class="row">
      <div class="logoholder text-center">
        <!-- <img src="assets/img/logo.png" /> -->
      </div>
      <!--.logoholder-->

      <div class="me">
        <p contenteditable>
          <strong>LUXURY SEXSHOP</strong><br />
          CALLE 3 # 23-45<br />
          Florencia-Caqueta.<br />
        </p>
      </div>
      <!--.me-->

      <div class="info" id="inf">
        <p contenteditable>
          WEB: <a href="http://volkerotto.net">www.luxury.com
          </a><br/>
          E-MAIL: <a href="mailto:info@obedalvarado.pw">luxury@gmail.com</a><br/>
          TEL: 3143452123<br/>
          TWITTER: @luxury
        </p>
      </div>
      <!-- .info -->

    </header>

    <div class="row section">
      <div class="col-2">
        <h1 contenteditable>FACTURA</h1>
      </div>
      <!--.col-->

      <div class="col-2 text-right details">
        <p contenteditable id="datos-factura">

        </p>
      </div>
      <!--.col-->

      <div class="col-2">
        <p contenteditable class="client" id="client">
          <strong>FACTURAR A:</strong>

        </p>
      </div>

      <!--.col-->
    </div>
    <!--.row-->

    <div class="row section" style="margin-top: -1rem">
      <div class="col-1">
        <table style="width: 100%">
          <thead contenteditable>
            <tr class="invoice_detail">
              <th width="25%" style="text-align">VENDEDOR</th>
              <th width="25%">ORDEN DE COMPRA</th>
              <th width="20%">ENVIAR POR</th>
            
            </tr>
          </thead>
          <tbody contenteditable  >
            <tr class="invoice_detail">
              <td width="25%" id="vender"></td>
              <td width="25%" id="serie"></td>
              <td width="20%">DHL</td>
              <td width="30%">PAGO ON LINE</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!--.row-->

    <div class="invoicelist-body">
      <table>
        <thead contenteditable>
          <th width="5%" >CODIGO</th>
          <th width="60%">PRECIO DEL ENVIO</th>

          <th width="10%">CANTIDAD</th>
          <th width="15%">PRECIO</th>
          <th class="taxrelated">IVA</th>
          <th width="10%">TOTAL</th>
        </thead>
        <tbody>
          <tr>
            <td width="5%">
              <!-- <a class="control removeRow" href="#">x</a> -->
              <span contenteditable id="cdg"></span>
            </td>
            <td width="60%"><span contenteditable id="letras"></span></td>
            <td id="amount"><input type="text" value="1"/></td>
            <td id="price"><input type="text" value="99"/></td>
            <td id="avi"><input type="text" value="2"/></td>
            <td id="lotal"><input type="text" value="99"/></td>
        -->
          </tr>
        </tbody>
      </table>
   
    </div>
    <!--.invoice-body-->

    <!-- <div class="invoicelist-footer" >
      <table contenteditable>
        <tr id="">
          <td>IVA:</td>
          <td id="total_tax"></td>
        </tr>
        <tr>
          <td><strong>Total:</strong></td>
          <td id="total_price"></td>
        </tr>
      </table>
    </div> -->

    <div class="note" contenteditable>
      <h2>Nota:</h2>
    </div>
    <!--.note-->

    <footer class="row">
      <div class="col-1 text-center">
        <p class="notaxrelated" contenteditable>
          El monto de la factura no incluye el impuesto sobre las ventas.
        </p>
      </div>
    </footer>
  </div>
</template>
<style>
#fact{
  background-color: black;
  color: white;
}

#client{
  background-color: rgb(9, 9, 9);
  color: white;
}

.control-bar{
  background-color: rgb(60, 6, 60) !important;
}
#paint{
  color: rgb(250, 250, 250);
  background-color:rgb(60, 6, 60);
border: solid 1px black;
}



</style>

<script>
export default {
  data() {
    return {
      factura: [],
    };
  },
  mounted() {
    let array = localStorage.getItem("items");
    this.factura = JSON.parse(array);
    this.datos()
    // console.log(this.factura)
  },
  methods: {


    datos() {
      let content = document.getElementById('client')
      content.innerHTML = `
        
        <p>Nombre: ${this.factura.user}</p>
        <p>Email: ${this.factura.email}</p> 
        <p>Direccion: ${this.factura.address}</p> 
        <p>Telefono: ${this.factura.phone_number}</p>  

      `
      let factura = document.getElementById('datos-factura')
      factura.innerHTML = `
        <p>Fecha: ${this.factura.created_at}</p>
        <p>Numero de factura: ${this.factura.number}</p>

      `
      let vender = document.getElementById('vender')
      vender.innerHTML = `
        <p>:${this.factura.user}</p>
      

      `
      let serie = document.getElementById('serie')
      serie.innerHTML = `
        <p>${this.factura.id}</p>
      

      `
      let cdg = document.getElementById('cdg')
      cdg.innerHTML = `
        <p>${this.factura.code}</p>
      

      `
      let letras = document.getElementById('letras')
      letras.innerHTML = `
        <p>${this.factura.shipping_price}</p>
      

      `
      let amount = document.getElementById('amount')
      amount.innerHTML = `
        <p>${this.factura.amount}</p>
      

      `
      let price = document.getElementById('price')
      price.innerHTML = `
        <p>$${this.factura.price}</p>
      

      `
      let avi = document.getElementById('avi')
      avi.innerHTML = `
        <p>${this.factura.iva_percent}%</p>
      

      `
      let lotal = document.getElementById('lotal')
      lotal.innerHTML = `
        <p>$${this.factura.total}</p>
      

      `
    }
  },
};
</script>



 
 