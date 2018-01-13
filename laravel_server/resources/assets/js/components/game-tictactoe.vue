<template>
	<div class="gameseparator">
        <div>
            <h2 class="text-center">Game {{game.gameID}}</h2>
            <br>
        </div>
        <div class="game-zone-content">       
            <div class="alert" :class="alerttype">
                <strong>{{ message }} &nbsp;&nbsp;&nbsp;&nbsp;<a v-on:click.prevent="closeGame">Close Game</a></strong>
            </div>
            <div class="board">
                <div v-for="(piece, key) of game.board" >
                    <img v-bind:src="pieceImageURL(piece)" v-on:click="clickPiece(key)">
                </div>
            </div>
            <hr>
        </div>  
    </div>			
</template>

<script type="text/javascript">
	export default {
        props: ['game'],
        data: function(){
			return {

            }
        },
        computed: {


            ownPlayerNumber(){
                if(this.game.player1SocketID == this.$parent.socketId){
                    return 1;
                } else if(this.game.player2SocketID == this.$parent.socketId){
                    return 2;
                }
                return 0;
            },

            adversaryName(){
                var ownNumber = this.ownPlayerNumber;
                if(ownNumber == 1){
                    return this.game.player2;
                }
                if(ownNumber == 2){
                    return this.game.player1;
                }
                return 0;
            },
            alerttype(){
                if(!this.game.gameStarted) {
                    return "alert-warning";
                } else if(this.game.gameEnded) {
                    if(this.game.winner == this, ownPlayerNumber){
                        return "alert-success";
                    } else if (this.game.winner == 0){
                        return "alert-info";
                    }
                        return "alert-danger";

                } else {
                    if(this.game.playerTurn == this.ownPlayerNumber){
                        return "alert-success";
                    } else{
                        return "alert-info"; 
                    }
                }
            },

            message(){
                if(!this.game.gameStarted) {
                    return "Game has not started yet!";
                } else if(this.game.gameEnded) {
                    if(this.game.winner == this, ownPlayerNumber){
                        return "Game has ended. YOU WIN!";
                    } else if (this.game.winner == 0){
                        return "Game has ended. THERE WAS A TIE!";
                    }
                        return "Game has ended. " + this.adversaryName+" has won!";
                    

                } else {
                    if(this.game.playerTurn == this.ownPlayerNumber){
                        return "It's your turn";
                    } else{
                        return "It's " +this.adversaryName+ "s' turn."; 
                    }
                }
            },
        },
        methods: {
            closeGame (){
                this.$parent.close(this.game);
            },
            clickPiece(index){
                if(!this.game.gameEnded){
                    if(this.game.playerTurn != this.ownPlayerNumber){
                        alert("It's not your turn to play");
                    } else {
                        if(this.game.board[index] == 0) {
                            this.$parent.play(this.game, index);
                        }
                    }
                }
            }
        }
    }
</script>

<style scoped>	
.gameseparator{
    border-style: solid;
    border-width: 2px 0 0 0;
    border-color: black;
}

</style>