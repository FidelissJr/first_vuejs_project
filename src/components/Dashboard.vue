<template>   
    <div id="burger-table">
        <Message :msg="msg" v-show="msg"/>
        <table class="table">
  <thead>
    <tr style="background-color: #222;">
      <th scope="col">#</th>
      <th scope="col">Cliente</th>
      <th scope="col">Pão</th>
      <th scope="col">Carne</th>
      <th scope="col">Opcionais</th>
      <th scope="col">Ações</th>
    </tr>
  </thead>
  <tbody v-for="burger in burgers" :key="burger.id">
      <tr>
        <td>{{burger.id}}</td>
        <td>{{burger.nome}}</td>
        <td>{{burger.pao}}</td>
        <td>{{burger.carne}}</td>
        <td>
            <ul v-for="(opcional, index) in burger.opcionais" :key="index">
                <li>
                    {{opcional}}
                </li>
            </ul>
        </td>
        <td>
          <select @change="AtualizarStatus(burger.id, $event)">
              <option value="">Selecione um Status</option>
              <option v-for="(st, index) in status" :key="index" :value="st.id" :selected="burger.status == st.tipo">{{st.tipo}}</option>
          </select>
          <button class="delete-btn" @click="removerBurger(burger.id)">Cancelar</button>
      </td>
    </tr>   
  </tbody>
</table>
    </div>
</template>

<script>

import '../style/global.css'
import Message from './Message.vue'

export default {
    name: 'Dashboard',
    components:{
        Message
    },
    data(){
        return{
            burgers : null,
            burgerId : null,
            status: [],
            msg: null
        }
    },
    methods:{
        async GetBurgers(){
            const req = await fetch("http://localhost:3000/burgers");
            const data = await req.json();
            this.burgers = data;
            
            
        },
        async GetStatus(){
            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();

            this.status = data;
  
        },
        async removerBurger(id){
            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "DELETE"
            });

            this.msg = `Pedido N ${id} cancelado com sucesso`;

            setTimeout(() => this.msg="", 3000);
            
        this.GetBurgers();   
        },
        async AtualizarStatus(id, event){
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });

            console.log(option);


            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                methods: "PUT",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            
            const res = await req.json();

            console.log(res);


        }
    },
    mounted(){
        this.GetBurgers();
        this.GetStatus();
    }
}
</script>

<style scoped>
    #burger-table{
        max-width: 1200px;
        margin: 0 auto;

    }
     
     .burger-table-row{
         width: 100%;
         
     }

     thead{
         color: #FCBA03;
     }

     select {
    padding: 12px 6px;
    margin-right: 12px;
  }
  .delete-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }

     

</style>