<template>
  <div id="home">
<b>Gabarito</b> 
      
      <div id="Question1">
           <b>Questão 1:</b> 
          <input type="radio" id="a1" value="A" v-model="QuestionCheck1">
          <label for="a1">A</label>

          <input type="radio" id="b1" value="B" v-model="QuestionCheck1">
          <label for="b1">B</label>

          <input type="radio" id="c1" value="C" v-model="QuestionCheck1">
          <label for="c1">C</label>
          
      </div><br>

      <div id="Question2">
           <b>Questão 2:</b> 
          <input type="radio" id="a2" value="A" v-model="QuestionCheck2">
          <label for="a2">A</label>

          <input type="radio" id="b2" value="B" v-model="QuestionCheck2">
          <label for="b2">B</label>

          <input type="radio" id="c2" value="C" v-model="QuestionCheck2">
          <label for="c2">C</label>
          
      </div><br>

      <div id="Question3">
           <b>Questão 3:</b> 
          <input type="radio" id="a3" value="A" v-model="QuestionCheck3">
          <label for="a3">A</label>

          <input type="radio" id="b3" value="B" v-model="QuestionCheck3">
          <label for="b3">B</label>

          <input type="radio" id="c3" value="C" v-model="QuestionCheck3">
          <label for="c3">C</label>
          
      </div><br>

        <button @click="limpar();salvar()"><i class="material-icons left">save</i>Salvar</button><br><br>

    <div class="respost">

      <b>Nome do Aluno:</b> <input type="text" placeholder="NomeAluno" v-model="AlunoDigit" class="nomeAluno"><br><br>

      <div id="Question1Resp">
           <b>Questão 1:</b> 
          <input type="radio" id="a1resp" value="A" v-model="QuestionRespCheck1">
          <label for="a1resp">A</label>

          <input type="radio" id="b1resp" value="B" v-model="QuestionRespCheck1">
          <label for="b1resp">B</label>

          <input type="radio" id="c1resp" value="C" v-model="QuestionRespCheck1">
          <label for="c1resp">C</label>
          
      </div><br>

      <div id="Question2Resp">
           <b>Questão 2:</b> 
          <input type="radio" id="a2resp" value="A" v-model="QuestionRespCheck2">
          <label for="a2resp">A</label>

          <input type="radio" id="b2resp" value="B" v-model="QuestionRespCheck2">
          <label for="b2resp">B</label>

          <input type="radio" id="c2resp" value="C" v-model="QuestionRespCheck2">
          <label for="c2resp">C</label><br>
      </div><br>

      <div id="Question3Resp">
           <b>Questão 3:</b> 
          <input type="radio" id="a3resp" value="A" v-model="QuestionRespCheck3">
          <label for="a3resp">A</label>

          <input type="radio" id="b3resp" value="B" v-model="QuestionRespCheck3">
          <label for="b3resp">B</label>

          <input type="radio" id="c3resp" value="C" v-model="QuestionRespCheck3">
          <label for="c3resp">C</label>
          
          
      </div><br>
<button @click="salvarResp(); verifica(); carregaTabela();"><i class="material-icons left">save</i>Salvar</button>
        
  </div><br>
  <b>Lista de Alunos Aprovados</b><br>
  <table class="tabela">
          <thead>
              <tr>
                  <th>Nome</th>
                  <th>Nota</th>
                  <th>Ações</th>
              </tr>
          </thead>
          <tbody>
              <tr :value="alunos.id" v-for="alunos in alunos2" :key="alunos.id" class="dados">
                  <td>{{alunos.nome}}</td>
                  <td>{{alunos.nota}}</td>
                  <td><button @click="deletar(alunos.id)"><i class="material-icons">delete_sweep</i>Deletar</button></td>
              </tr>
          </tbody>
      </table>
  </div>
</template>

<script>
export default {
    data:function(){
        return{
            AlunoDigit:'',
            QuestionCheck1:'',
            QuestionCheck2:'',
            QuestionCheck3:'',

            QuestionRespCheck1:'',
            QuestionRespCheck2:'',
            QuestionRespCheck3:'',

            nota:0,
            id:0,

            gabarito2:[],
            resposta2:[],
            alunos2:[]
        }
    },methods:{
        limpar:function(){
            this.$store.state.gabarito.length =0
        },
        salvar:function(){
             
            this.$store.state.gabarito.push(
                this.QuestionCheck1,
                this.QuestionCheck2,
                this.QuestionCheck3,
            )

            this.gabarito2=this.$store.state.gabarito  
            console.log(this.gabarito2) 
        },
        salvarResp:function(){   

            this.$store.state.resposta.push(
                this.QuestionRespCheck1,
                this.QuestionRespCheck2,
                this.QuestionRespCheck3,
            )
            this.resposta2=this.$store.state.resposta
            console.log(this.resposta2)
        },
        verifica:function(){
            if(this.$store.state.alunos.length==100){
                return alert("Numero Maximo de alunos Cadastrados")
            }

            for(var i=0; i < 3; i++){
                if(this.resposta2[i]==this.gabarito2[i]){
                    this.nota=this.nota+3
                }
            }
            if(this.nota>7){
                this.$store.state.alunos.push({
                    id: this.$store.state.alunos.length,
                    nome: this.AlunoDigit,
                    nota: this.nota
                })
                console.log("salvo com sucesso")
                console.log(this.$store.state.alunos)
            }
            this.resposta2=[]
            this.$store.state.resposta=[]
            this.nota=0; 

            localStorage.setItem('dados',JSON.stringify(this.$store.state.alunos))
            var objSalvo2=localStorage.getItem('dados')
            console.log('objSalvo', JSON.parse(objSalvo2))
    
        },
        carregaTabela:function(){
            this.alunos2=this.$store.state.alunos
        },
        deletar:function(id){
            var item=this.$store.state.alunos.find(item => item.id == id)
            
            this.$store.state.alunos.splice(this.$store.state.alunos.indexOf(item.id),1)

            localStorage.setItem('dados',JSON.stringify(this.$store.state.alunos))
            var objSalvo2=localStorage.getItem('dados')
            console.log('objSalvo', JSON.parse(objSalvo2))

            this.carregaTabela()
        }

    }, mounted(){
            var objSalvo=localStorage.getItem('dados')
            
            this.$store.state.alunos=JSON.parse(objSalvo)
            console.log('store',this.$store.state.alunos)

            this.alunos2=JSON.parse(objSalvo)

            
            
    }

}
</script>

<style>
table{
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 30%;
}
td th{
    border: 1px solid #ddd;
  padding: 8px;
}
tr:nth-child(even){background-color: #f2f2f2;}
tr:hover {background-color: #ddd;}
th{
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: center;
  background-color: rgba(0,150,136,0.7);
  color: white;
}
button{
     display: flex;
     margin-left: 45%;
    -webkit-box-shadow:0 2px 2px 0 
    rgba(0,0,0,0.14),0 3px 1px -2px 
    rgba(0,0,0,0.12),0 1px 5px 0 
    rgba(0,0,0,0.2);
    box-shadow:0 2px 2px 0
     rgba(0,0,0,0.14),0 3px 1px -2px 
     rgba(0,0,0,0.12),0 1px 5px 0 
     rgba(0,0,0,0.2);
    background-color:rgba(0,150,136,0.7);
    height:32.4px;
    width: 110px;
    line-height:32.4px;
    font-size:13px;
    font-size:1.2rem;
    border: 0px;
    border-radius: 5px;
    color: white;
    text-align: right;
}
button i{
    padding-top: 2px;
}
.tabela button{
    display: flex;
    margin-left: 30%;
    background-color:rgba(244,67,54,0.7);
}
table{
    margin-left: 35%;
}
</style>