<template>
  <div id="app">
  <div  v-if ="!this.entered && this.initial" > 
   <div>
    <v-jumbotron dark>
      <v-container fill-height>
        <v-layout align-center>
          <v-flex text-xs-center>
            <h3 class="display-3">Welcome</h3>
            <h2 class="display-1">Lets Play Connect 4</h2>
            <v-btn
            @click="entering"
            class="mx-0"
            large
          >
            Enter
          </v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-jumbotron>
  </div>
  </div>
  <div  v-if ="!this.entered && !this.initial" > 
   <div>
    <v-jumbotron dark>
      <v-container fill-height>
        <v-layout align-center>
          <v-flex text-xs-center>
            <h3 class="display-3">We have a Winner!!</h3>
            <h3 class="display-3">Hope you enjoyed</h3>
            <h2 class="display-1">Let's Play Again 4</h2>
            <v-btn
            @click="entering"
            class="mx-0"
            large
          >
            Sure
          </v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-jumbotron>
  </div>
  </div>
  <div  v-if ="this.entered" id="game-board">
	<div class="row-6" id="6">
    <svg  v-for="index in 7" :key="index"  height="100" width="100" class="row-5 " >
		  <circle @click="drop(index-1)" cx="50" cy="50"  r="40" :stroke= "stroke" :stroke-width= "width" v-bind:class=board[index-1][6] />
		</svg> 
	</div>
    <div class="row-5" id="5" >
    <svg v-for="index in 7" :key="index" height="100" width="100" class="row-5">
		  <circle  @click="drop(index-1)"  cx="50" cy="50" r="40" :stroke= "stroke" :stroke-width= "width" v-bind:class=board[index-1][5] />
		</svg> 
	</div>
  	<div class="row-4" id="4">
    <svg v-for="index in 7" :key="index"  height="100" width="100" class="row-5">
		  <circle @click="drop(index-1)" cx="50" cy="50" r="40" :stroke= "stroke" :stroke-width= "width"  v-bind:class=board[index-1][4] />
		</svg> 
	</div>
  	<div class="row-3" id="3">
    <svg v-for="index in 7" :key="index"  height="100" width="100" class="row-5">
		  <circle @click="drop(index-1)" cx="50" cy="50" r="40" :stroke= "stroke" :stroke-width= "width"  v-bind:class=board[index-1][3] />
		</svg> 
	</div>
  	<div class="row-2" id="2">
    <svg v-for="index in 7" :key="index" height="100" width="100" class="row-5">
		  <circle  @click="drop(index-1)" cx="50" cy="50" r="40" :stroke= "stroke" :stroke-width= "width"  v-bind:class=board[index-1][2] />
		</svg> 
	</div>
    <div class="row-1" id="1">
    <svg v-for="index in 7" :key="index"  height="100" width="100" class="row-5">
		  <circle @click="drop(index-1)" cx="50" cy="50" r="40" :stroke= "stroke" :stroke-width= "width"  v-bind:class=board[index-1][1] />
		</svg> 
	</div>
  <div class="row-0" id="0">
    <svg v-for="index in 7" :key="index" height="100" width="100" class="row-5">
		  <circle  @click="drop(index-1)" cx="50" cy="50" r="40" :stroke= "stroke" :stroke-width= "width" v-bind:class=board[index-1][0] />
		</svg> 
	</div>
</div>


  </div>
</template>
<script src="https://code.jquery.com/jquery-1.10.2.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vuetify/dist/vuetify.js"></script>
<script>
export default {
  name: 'app',
  data () {
    return {
      dialog: false,
      stroke: 'rgb(49,65,58)',
      width: "10",
      playerIsRed:true,
      row:0,
      columns:[0,0,0,0,0,0,0],
      column:0,
      counter:1,
      columnCounter:1,
      counter2:0,
      entered:false,
      initial:true,
      rowCounter:1,
      errors:[],
      yellowPlayersMove:0,
      className:"red",
      board: [[],
              [],
              [],
              [],
              [],
              [],
              []]
    }
  },
  methods:{
/*
red player is the human player
check if the selected colunm has room
if it is red players turn(TODO: upgrade the app to multiplayer)
push a tile to ht e selected colunm
on each move (human or computer)
check if there is a winner
*/
    drop(column){
      this.column = column
      this.columns[column] +=1
      if(!this.playerIsRed){
        Alert("Please wait for your turn")
        }
      else{
        if(this.board[column].length<7 ){
          this.board[column].push("red")
          this.playerIsRed = !this.playerIsRed
          this.yellowPlayer()
          this.board[this.yellowPlayersMove].push("yellow") 
          this.playerIsRed = !this.playerIsRed
      }else{alert("This column is full, choose another one")}
      }  
      //TODO extend this to check if the yeloow is a winner
       this.checkWinner(this.column)
    },  
    samecolumnWinner(column){
      try {
         let itemRow = this.board[column].length-1
      //console.log('itemRow:',itemRow)
      let board = this.board
      //console.log('board:',board)
            for (let i=1; i<4; i++){
                if(board[column][itemRow]==board[column][itemRow-i]&& board[column][itemRow-i]!=undefined){
                  this.columnCounter++
                  //console.log("we hav a hit",this.rowCounter)
                  if (this.columnCounter==4){this.entered =!this.entered
                  //alert ('we have a winner')
                  }
                }
          } 
          this.columnCounter =1
      }
    catch(err) {
    this.errors.push(err)
    console.log(err)
} 
  },
  checkWinner(column){
      this.samecolumnWinner(column) 
      this.sameRowWinner(column)
      this.diagonalWinnerLefttoRight(column) 
      this.diagonalWinnerRighttoLeft(column)
  },
  yellowPlayer(){
  let arrayLengths=[]
  this.board.forEach((array)=>{
    //console.log('inside the loop')
     arrayLengths.push(array.length)})
     //console.log('arrayLengths: ',arrayLengths)
     let minCol = Math.min(...arrayLengths)
     let indexOfMinCol = arrayLengths.indexOf(minCol)
     this.yellowPlayersMove = indexOfMinCol
  },
  sameRowWinner(column){
    try {
      let itemRow = this.board[column].length-1
      console.log('itemRow:',itemRow)
      let board = this.board
      console.log('board:',board)
          for (let i=0; i<7;i++){
            this.rowCounter =1
            for (let j=0; j<3;j++){
                if(board[i][itemRow]==board[i+j+1][itemRow]&& board[i+j+1][itemRow]!=undefined){
                  this.rowCounter++
                  console.log("we have a hit",this.rowCounter)
                }
                if (this.rowCounter==4){  this.entered =!this.entered
                //alert('we have a winner')
                }
            }
          } 
    }
    catch(err) {
      this.errors.push(err)
      console.log(err)
    }  
  },
  diagonalWinnerLefttoRight(column){
  try {
    //console.log('inside diagomnal left to right')
    const itemRow = this.board[column].length-1
    const startingColumn = column>itemRow ? (column-itemRow) : 0
    const startingRow = itemRow>column ? (itemRow - column) : 0
    console.log('startingColumn: ',startingColumn)
    console.log('startingRow: ',startingRow)
    for(let i = 0; i<7 ; i++){
       this.counter= 1;
       if (this.board[(startingColumn)+i][startingRow+i]== undefined) break
       for(let j=0; i<4; j++){
        //console.log('(startingColumn)-i][startingRow+i]',(startingColumn)-i,startingRow+i)
        //console.log('(startingColumn)-j-1-i][startingRow+i+1+j]',(startingColumn)-j-1-i,startingRow+i+1+j)
         if(this.board[(startingColumn)+j+1+i][startingRow+i+1+j]== undefined)break
         if(this.board[(startingColumn)+i][startingRow+i]==this.board[(startingColumn)+j+1+i][startingRow+i+1+j])
            {this.counter = this.counter+1
            //console.log("this.counter",this.counter)
            if (this.counter == 4){this.entered =!this.entered
            //alert ('we have a winner')
            }
            }
       }     
      }
    }
    catch(err) {
      this.errors.push(err)
      console.log(err)
    }
  },
  diagonalWinnerRighttoLeft(column){
    try {
      console.log('inside')
     const itemRow = this.board[column].length-1
     const startingColumn = (column+itemRow)<=6 ? (column+itemRow) : 6
     //console.log('startingColumn: ',startingColumn)
     const startingRow = (column+itemRow)<=6 ? 0 : (column+itemRow-6)
     //console.log('startingRow: ',startingRow)
     for(let i = 0; i<7 ; i++){
       this.counter2= 1;
       if (this.board[(startingColumn)-i][startingRow+i]== undefined) break
       for(let j=0; i<4; j++){
         //console.log('(startingColumn)-i][startingRow+i]',(startingColumn)-i,startingRow+i)
        //console.log('(startingColumn)-j-1-i][startingRow+i+1+j]',(startingColumn)-j-1-i,startingRow+i+1+j)
         if(this.board[(startingColumn)-j-1-i][startingRow+i+1+j]== undefined)break
         if(this.board[(startingColumn)-i][startingRow+i]==this.board[(startingColumn)-j-1-i][startingRow+i+1+j])
            {this.counter2 = this.counter2+1
            //console.log("this.counter2",this.counter2)
            if (this.counter2 == 4){this.entered =!this.entered
            //alert("we have a winner")
            }
            }
       }     
      }
    }
    catch(err) {
      this.errors.push(err.message)
      console.log(err)
    }
  },
  entering(){
    this.entered=true
    this.initial= false
    this.playerIsRed=true,
     this.row=0,
      this.column=0,
      this.counter=1,
      this.columnCounter=1,
      this.counter2=0,
      this.rowCounter=1,
      this.errors=[],
      this.yellowPlayersMove=0,
      this.className="red",
      this.board= [[],
              [],
              [],
              [],
              [],
              [],
              []]
  }
},
  computed:{
    who(){
      return "test"
    },
  },
}
</script>
<style>
* {
	border: 0;
	padding: 0;
  background-color: lightslategray;
}
#game-board {
 background: grey;
 width: 730px;
 cursor: pointer;
  margin: auto;
}
.column {
	width: 100px;
	display: inline-block;
}
circle.red {
	fill: #D50000;
}
circle.yellow {
	fill: #DAD400;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
