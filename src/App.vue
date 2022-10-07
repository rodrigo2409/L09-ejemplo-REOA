<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12">
        <p/>
        <div><h1 class="bg-dark text-white text-center" >vuejs</h1></div>
        <p/>
        <div id="app" >
          <table class="table table-stripped">
            <thead>
              <tr>
                <th style="width: 40px"></th>
                <th>descripcion</th>
                <th style="width: 200px">cantidad</th>
                <th style="width: 200px">cancelado</th>
              </tr>
            </thead>
            <tbody>

              <tr>
                <td></td>
                <td>
                  
                  <input type="text" class="form-control" v-model="newEntry.name" />
                </td>
                <td>
                  <input type="text" class="form-control" v-model="newEntry.amount" />
                </td>
                <td>
                  <button @click="add()" type="button" class="btn btn-success btn-block">agregar</button>
                </td>
              </tr>

              <tr v-if="items.length===0" >
                <td colspan="4" class="text-center">no hay registros</td>
              </tr>

              <tr v-for="item, index in items" >
                <td>
                  <button @click="remove(index)" type="button" class="btn btn-danger btn-xs">
                    <i class="glyphicon glyphicon-trash" ></i>
                  </button>
                </td>
                <td>{{ item.name }}</td>
                <td> {{ item.amount.toFixed(2) }} </td>
                <td class="text-center" :title="item.paid ? 'Si' : 'No' " >
                  <button 
                  @click="changeToPaid(item)"
                  class="btn btn-default btn-sm" 
                  :class="{ 'btn-success' : item.paid }" >
                    <i v-if="item.paid" class="glyphicon glyphicon-ok" ></i>
                    <i v-if="!item.paid" class="glyphicon glyphicon-remove" ></i>
                  </button> 
                </td>
              </tr>
            </tbody>
            <tfoot>

              <tr>
                <td></td>
                <td class="text-right text-danger ">debe</td>
                <td class="text-right text-danger " >{{ totalAmount(0) }}</td>
                <td></td>
              </tr>

              <tr>
                <td></td>
                <td class="text-right text-success">pagado</td>
                <td class="text-right text-success" >{{ totalAmount(1) }}</td>
                <td></td>
              </tr>

              <tr>
                <td></td>
                <td class="text-right text-primary">total</td>
                <td class="text-right text-primary" >{{ totalAmount(2) }}</td>
                <td></td>
              </tr>
            </tfoot>
          </table>
        </div>
      </div>
    </div>
  </div>


</template>

<script>  
  export default{
    data: () => ({
      newEntry:{
        name: '',
        amount: 0,
        

      },
      items: [
        { name: 'Servicios', amount: 200, paid: false },
        { name: 'Hosting de Anexsoft', amount: 90, paid: true }, 
      ]
    }),
    methods:{
      remove: function(index){
        this.items.splice(index,1);
      },
     
      add(){
        this.items.push({
          name: this.newEntry.name,
          amount: parseFloat(this.newEntry.amount),
          paid: false
        });
        this.newEntry.name = '';
        this.newEntry.amount = 0;
      },
      changeToPaid(item){
        item.paid = !(item.paid);
      },
      totalAmount(t){
        var total = this.items.reduce(function(a, b){
          switch(t){
            case 0: return a + (!b.paid ? b.amount : 0); 
            case 1: return a + (b.paid ? b.amount : 0); 
            case 2: return a + b.amount;
          }
        },0);
        return total.toFixed(2);
      },
    },
  };

</script>