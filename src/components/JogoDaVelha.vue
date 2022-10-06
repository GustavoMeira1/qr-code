<template>
    <v-app>
        <v-row justify="center" class="topPage">
            <v-card class="restartBtn">
                <button class="btn" @click="circleWins=0;xWins=0;ties=0">
                    <v-icon x-large color="white">mdi-close-circle-outline</v-icon>
                </button>
            </v-card>
            <v-card class="turnCard">
                <p>{{turnName}} - TURN</p>
            </v-card>
            <v-card class="restartBtn">
                <button  class="btn"><v-icon x-large color="white">mdi-restart</v-icon></button>
            </v-card>
        </v-row>


        <div class="table">
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
        </div>

        <v-row justify="center">
            <v-card  class="totalcard">
                <span> X</span>
                <p>{{xWins}}</p>  
            </v-card>
            <v-card class="totalcard">
                <span> TIES</span> 
                <p>{{ties}}</p> 
            </v-card>
            <v-card class="totalcard">
                <span> ◯ </span>
                <p>{{circleWins}}</p>
            </v-card>
        </v-row>
        <br>



        <v-dialog v-model="dialog"  width="500" height="350">
            <v-card class="dial" dark>
            <br>
                <span class="text">{{winner}}</span>
              <v-card-actions>
                <button class="btn"  @click="dialog=false" >FECHAR</button>
              </v-card-actions>
            </v-card>
        </v-dialog>

    </v-app>
</template>

<script>
export default {
    name: 'Velha',
    data(){
        return{
            dialog: false,
            circleWins: 0,
            xWins:0,
            ties:0,
            winner: "",
            turnName: "X"
        };
    },
    mounted(){

        /* Selecting objects */
        const cellElements = document.querySelectorAll(".cell")
        const board = document.querySelector(".table")
        const buttons = document.querySelectorAll(".btn")

        /* Winning combinations on tic tac toe */
        const winningCombination = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ]
        
        let isCircleTurn = false

        /* Starting game by cleaning the cells after a game and adding the click events */
        const startGame = () =>{
        for (const cell of cellElements){
            cell.classList.remove("circle")
            cell.classList.remove("x")
            if(isCircleTurn){
                board.classList.add("circle")
                this.turnName = "◯"
            } else {
                board.classList.add("x")
                this.turnName = "X"
            }
            cell.removeEventListener("click", handleClick)
            cell.addEventListener("click", handleClick, {once: true})
        }
        }

        /* Checking the cells for a winning combination */
        const checkForWin = (currentPlayer) =>{
            return winningCombination.some((combination) =>{
                return combination.every((index)=>{
                    return cellElements[index].classList.contains(currentPlayer)
                })
            })
        }

        /* Checking if every cell are already filled meaning a possible draw */
        const checkForDraw = () =>{
            return [...cellElements].every((cell) => {
              return  cell.classList.contains("x") || cell.classList.contains("circle")
            })
            }

        /* Changing the class that will be added to the cell after a click */
        const swapTurns = () => {
            isCircleTurn = !isCircleTurn
            
            board.classList.remove("circle")
            board.classList.remove("x")

            if(isCircleTurn){
                board.classList.add("circle")
                this.turnName = "◯"
            } else {
                board.classList.add("x")
                this.turnName = "X"
            }
        }

        /* The ending message */
        const endGame = (isDraw) => {
            if (isDraw){
                this.winner = ("Empate")
            } else {
                if(isCircleTurn){
                    this.winner = ("Bolinha Venceu!")
                } else {
                    this.winner = ("X Venceu!")
                }
            }
        }

        /* Click event to add the classes and check the game situation */
        const handleClick = (e) => {
            const cell = e.target;
            const classToAdd = isCircleTurn ? 'circle' : 'x';

            cell.classList.add(classToAdd)

            const isWin = checkForWin(classToAdd)
            const isDraw = checkForDraw()
        
            if (isWin){
                endGame(false)
            board.classList.remove("circle")
            board.classList.remove("x")
                for (const cell of cellElements){
                    cell.removeEventListener("click", handleClick)
                }
                if(isCircleTurn){
                    this.circleWins++
                    isCircleTurn=true
                }else{
                    this.xWins++
                    isCircleTurn=false
                }
                this.dialog = true
            } else if(isDraw){
            board.classList.remove("circle")
            board.classList.remove("x")
            this.ties++
                endGame(true)
                this.dialog = true
            } else {
                swapTurns()
            }
        }

        startGame()
        for(const btn of  buttons){
        btn.addEventListener("click",startGame)
        }
    },
    methods:{
    }

}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Merriweather&display=swap');

.score-title{
    text-align: center;
    font-size: 20px;
    text-decoration: underline;
}
.score{
    text-align: center;
}
.points{
    font-family: 'Merriweather', serif;
    font-size: 40px;
    padding: 5px;
    border: 1px solid black;
    font-weight: bold;
}
.left{
    border-left: none;
}
.right{
    padding-left: 15px;
    border-right: none;
}


.table{
    display: grid;
    width: 100%;
    height: 100%;
    justify-content: center;
    align-content: center;
    justify-items: center;
    align-items: center;
    grid-template-columns: repeat(3,auto);
}
.table.x .cell:not(.x):not(.circle):hover::after,
.table.x .cell:not(.x):not(.circle):hover::before,
.table.circle .cell:not(.x):not(.circle):hover::after,
.table.circle .cell:not(.x):not(.circle):hover::before{
    background: rgba(0, 0, 0, .4) !important;
}


.cell{
    width: 100px;
    height: 100px;
    border: 3px solid rgb(0, 0, 0);
    display: flex;
    justify-content: center;
    align-items: center;
}
.cell:nth-child(1),
.cell:nth-child(2),
.cell:nth-child(3){
    border-top: none;
}
.cell:nth-child(3),
.cell:nth-child(6),
.cell:nth-child(9){
    border-right: none;
}
.cell:nth-child(1),
.cell:nth-child(4),
.cell:nth-child(7){
    border-left: none;
}
.cell:nth-child(7),
.cell:nth-child(8),
.cell:nth-child(9){
    border-bottom: none;
}

.cell.x::before,
.cell.x::after,
.table.x .cell:not(.x):not(.circle):hover::after,
.table.x .cell:not(.x):not(.circle):hover::before{
    content: "";
    height: calc(100px * .15);
    width: calc(100px * .9);
    background: rgb(0, 0, 0);
    position: absolute;
    border-radius: 50%;
}
.cell.x::before,
.table.x .cell:not(.x):not(.circle):hover::before{
    transform: rotate(45deg);
}
.cell.x::after,
.table.x .cell:not(.x):not(.circle):hover::after{
    transform: rotate(135deg);
}

.cell.circle::before,
.cell.circle::after,
.table.circle .cell:not(.x):not(.circle):hover::after,
.table.circle .cell:not(.x):not(.circle):hover::before{
    content: "";
    height: calc(100px * .8);
    width: calc(100px * .8);
    background: rgb(0, 0, 0);
    position: absolute;
    border-radius: 50%;
}

.win-message{
    color: aqua;
}
.dial{
    display: flex;
    background-color: rgba(0, 0, 0, .4);
    align-items: center;
    flex-direction: column;
}
.text{
    display: block;
    text-align: center;
    font-size: 65px;
    font-family: 'Merriweather', serif;

}
/*.btn{
    background-color: rgba(255, 255, 255, 0.219);
    margin: 20px;
    padding: 15px;
    border-radius: 20px;
    font-family: 'Merriweather', serif;
    font-size: 35px;
    color: black;
    font-weight: bold;
    transition: 1s;
}
.btn:hover{
    background-color: rgb(255, 255, 255);
}*/
.totalcard{
    background-color: rgb(255, 136, 0) !important;
    border-radius: 10px !important;
    justify-content: center;
    justify-items: center;
    text-align: center;
    padding-top: 4px;
    width: 100px;
    height: 80px;
    margin: 10px;
    font-weight: bold;
    border-bottom: 5px solid rgb(196, 104, 0) !important;
    color: white !important;
}
.totalcard span{
    margin-top: -80px !important;
    font-size: 20px;
    font-weight: bolder;
}
.totalcard p {
    font-size: 30px !important;
    margin-top: -8px;
    font-weight: bolder !important;
}
.restartBtn{
    background-color: rgb(255, 136, 0) !important;
    height: 50px;
    width: 50px;
    display: flex;
    justify-content: center;
    margin: 15px;
    border-radius: 5px !important;
    border-bottom: 5px solid rgb(196, 104, 0) !important;
    color: white !important;
}
.turnCard{
    background-color: rgb(255, 136, 0) !important;
    height: 45px;
    width: 100px;
    text-align: center;
    padding-top: 5px;
    margin: 15px;
    border-radius: 10px !important;
    border-bottom: 5px solid rgb(196, 104, 0) !important;
}
.turnCard p{
    font-weight: bolder;
    font-size: 20px;
    color: white;
}
.topPage{
    padding-top: 90px;
}
</style>