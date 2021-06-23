<template>
	<div class="gameScene" v-if="state.isInGame === true">
    <div className="drawCardAnim" id="drawCardAnim"></div>
    <div class="lastPlayerCard" v-bind:text="state.lastPlayed">{{state.lastPlayed}}</div>
    <div class="cards" id="playerHand">
      <div className="card" v-for="card in state.cards" v-bind:key="card" v-on:click="PlayCard(card, 0)" v-bind:id="card.uuid">
        <div className="title">{{card}}</div>
      </div>
    </div>
    <div class="cards_left" id="playerHand_left">
      <div className="oponent_card" v-for="card in state.cards_left" v-bind:key="card" v-bind:id="card.uuid">
        <div className="title">{{card}}</div>
      </div>
    </div>
    <div class="cards_top" id="playerHand_top">
      <div className="oponent_card" v-for="card in state.cards_top" v-bind:key="card" v-bind:id="card.uuid">
        <div className="title">{{card}}</div>
      </div>
    </div>
    <div class="cards_right" id="playerHand_right">
      <div className="oponent_card" v-for="card in state.cards_right" v-bind:key="card" v-bind:id="card.uuid">
        <div className="title">{{card}}</div>
      </div>
    </div>
    <div className="quitButton" v-on:click="SetState(false)">
      <div className="title">Quit</div>
    </div>
    <div className="addCardButton" v-on:click="AddCard(0)">
      <div className="title" v-text="'Add card (' + state.cards.length + ')'"></div>
    </div>
    <div className="addOpponentCardButton" v-on:click="AddCard(1)">
      <div className="title" v-text="'Add card to left (' + state.cards_left.length + ')'"></div>
    </div>
    <div className="addTopCardButton" v-on:click="AddCard(2)">
      <div className="title" v-text="'Add card to top (' + state.cards_top.length + ')'"></div>
    </div>
    <div className="addRightCardButton" v-on:click="AddCard(3)">
      <div className="title" v-text="'Add card to right (' + state.cards_right.length + ')'"></div>
    </div>
  </div>

  <div v-if="state.isInGame === false">
    <div class="menuCards">
      <div className="menuCard" v-on:click="SetState(true)">
        <div className="title">Play game</div>
      </div>
      <div className="menuCard">
        <div className="title">Quit</div>
      </div>
    </div>
  </div>
</template>


<script>
import { reactive } from '@vue/reactivity'
export default {
  
  setup() {
    const state = reactive({
      isInGame: false,
      cards: [],
      cards_left: [],
      cards_top: [],
      cards_right: [],
      lastPlayed: "Nothing"
      });

    const SetState = (value) =>{
      state.isInGame = value

      if(value === false){
        state.cards = []
        state.cards_left = []
        state.cards_top = []
        state.cards_right = []
      }
    }

    const rightTransform = () => {
      return (parseInt((document.getElementById("playerHand").style.width.replace("px", "")) / 2) - 50);
    }

    const AddCard = async (hand) =>{

      if(hand == 0){
        var newCard = {
          id: Math.floor(Math.random() * 100),
          uuid: uuid()      
        };

        state.cards.push(newCard)
        DisplayHand(hand)

        document.getElementById("drawCardAnim").style.transition = "0.5s"
        document.getElementById("drawCardAnim").style.transform = "translate(0px, 300px)"
        document.getElementById("drawCardAnim").style.backgroundColor = "rgba(255, 255, 255, 0.0)"
        document.getElementById("drawCardAnim").style.boxShadow = "rgba(0, 0, 0, 0.0) 0px 0px 15px 5px"
        await delay(300);
        DrawCardAnimPos()
      }
      else if(hand == 1){
        state.cards_left.push("card")
        DisplayHand(hand)

        document.getElementById("drawCardAnim").style.transition = "0.5s"
        document.getElementById("drawCardAnim").style.transform = "translate(0px, 300px)"
        document.getElementById("drawCardAnim").style.backgroundColor = "rgba(255, 255, 255, 0.0)"
        document.getElementById("drawCardAnim").style.boxShadow = "rgba(0, 0, 0, 0.0) 0px 0px 15px 5px"
        await delay(300);
        DrawCardAnimPos()
      }
      else if(hand == 2){
        state.cards_top.push("card")
        DisplayHand(hand)

        document.getElementById("drawCardAnim").style.transition = "0.5s"
        document.getElementById("drawCardAnim").style.transform = "translate(0px, 300px)"
        document.getElementById("drawCardAnim").style.backgroundColor = "rgba(255, 255, 255, 0.0)"
        document.getElementById("drawCardAnim").style.boxShadow = "rgba(0, 0, 0, 0.0) 0px 0px 15px 5px"
        await delay(300);
        DrawCardAnimPos()
      }
      else if(hand == 3){
        state.cards_right.push("card")
        DisplayHand(hand)

        document.getElementById("drawCardAnim").style.transition = "0.5s"
        document.getElementById("drawCardAnim").style.transform = "translate(0px, 300px)"
        document.getElementById("drawCardAnim").style.backgroundColor = "rgba(255, 255, 255, 0.0)"
        document.getElementById("drawCardAnim").style.boxShadow = "rgba(0, 0, 0, 0.0) 0px 0px 15px 5px"
        await delay(300);
        DrawCardAnimPos()
      }
    }

    const DrawCardAnimPos = () =>{
      document.getElementById("drawCardAnim").style.transition = "0s"
      document.getElementById("drawCardAnim").style.transform = "translate(" + rightTransform() + "px, -400px)"
      document.getElementById("drawCardAnim").style.backgroundColor = "rgba(255, 255, 255, 1.0)"
    } 

    function uuid() {
      return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16));
    }

    function displayWidth(){
      return document.documentElement.clientWidth
    }

    function displayHeight(){
      return document.documentElement.clientHeight
    }

    const delay = ms => new Promise(res => setTimeout(res, ms));

    const DisplayHand = (hand) =>{
      console.log(displayWidth() + "---" + displayHeight())
      console.log(hand)
      if(hand == 0){
        if(((state.cards.length + 1) * 80) + (displayWidth() / 2 + 120) > document.documentElement.clientWidth){
          document.getElementById("playerHand").style.width = (document.documentElement.clientWidth - (displayWidth() / 2 + 120)).toString() + "px"
        }
        else{
          document.getElementById("playerHand").style.width = ((state.cards.length + 1) * 80).toString() + "px"
        }
      }
      else if(hand == 1){
        if(((state.cards_left.length + 1) * 50) + (displayHeight() / 2 + 150) > document.documentElement.clientHeight){
          document.getElementById("playerHand_left").style.width = (document.documentElement.clientHeight - (displayHeight() / 2 + 150)).toString() + "px"
        }
        else{
          document.getElementById("playerHand_left").style.width = ((state.cards_left.length + 1) * 50).toString() + "px"
        }
      }
      else if(hand == 2){
        console.log("babaeiwfjuerhu")
        if(((state.cards_top.length + 1) * 50) + (displayWidth() / 2 + 400) > document.documentElement.clientWidth){
          document.getElementById("playerHand_top").style.width = (document.documentElement.clientWidth - (displayWidth() / 2 + 400)).toString() + "px"
        }
        else{
          document.getElementById("playerHand_top").style.width = ((state.cards_top.length + 1) * 50).toString() + "px"
        }
      }
      else if(hand == 3){
        if(((state.cards_right.length + 1) * 50) + (displayHeight() / 2 + 150) > document.documentElement.clientHeight){
          document.getElementById("playerHand_right").style.width = (document.documentElement.clientHeight - (displayHeight() / 2 + 150)).toString() + "px"
        }
        else{
          document.getElementById("playerHand_right").style.width = ((state.cards_right.length + 1) * 50).toString() + "px"
        }
      }

      ReposHands();
    }

    const ReposHands = () =>{
      var phWidth = parseInt(document.getElementById("playerHand").style.width.replace("px", ""))
      console.log("!!!!! " + document.getElementById("playerHand").style.width)
      if(document.getElementById("playerHand").style.width == ""){
        phWidth = 160
      }

      var phTopWidth = parseInt(document.getElementById("playerHand_top").style.width.replace("px", ""))
      if(document.getElementById("playerHand_top").style.width == ""){
        phTopWidth = 160
      }

      var phLeftWidth = parseInt(document.getElementById("playerHand_left").style.width.replace("px", ""))
      if(document.getElementById("playerHand_left").style.width == ""){
        phLeftWidth = 160
      }

      var phRightWidth = parseInt(document.getElementById("playerHand_right").style.width.replace("px", ""))
      if(document.getElementById("playerHand_right").style.width == ""){
        phRightWidth = 160
      }

      console.log(phWidth + " " + phTopWidth + " " + phLeftWidth + " " + phRightWidth)

      var pos = phLeftWidth / 2 + (displayWidth() / 2) - 85
      pos -= phWidth / 2
      document.getElementById("playerHand_left").style.transform = "rotate(90deg) translate(-36vh, " + pos + "px)"
      var posTop = phTopWidth / 2
      posTop -= phWidth / 2
      var posTopVertAlign = (displayHeight()) - 195
      document.getElementById("playerHand_top").style.transform = "rotate(180deg) translate(" + posTop + "px, " + posTopVertAlign + "px)"
      var posRight = (displayWidth() / 2) - 85
      posRight += phWidth / 2
      posRight -= phRightWidth / 2
      document.getElementById("playerHand_right").style.transform = "rotate(270deg) translate(36vh, " + posRight + "px)"
      var posPhVertAlign = (displayHeight() / 2) - 100
      document.getElementById("playerHand").style.transform = "translate(0px, " + posPhVertAlign + "px)"
    }

    const PlayCard = (card, hand) =>{
      console.log("play card: " + card.id)

      state.cards = state.cards.filter(function(value){ 
        return value.id != card.id;
      });

      DisplayHand(hand)
      DrawCardAnimPos()

      state.lastPlayed = card
    }

    return {
      SetState,
      AddCard,
      PlayCard,
      state
    }
  }
}
</script>

<style lang="scss">
@import './App.scss'
</style>
