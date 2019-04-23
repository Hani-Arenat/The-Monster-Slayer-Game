<template>
      <div id="app">
        <section class="row">
            <div class="small-6 columns">
                <h1 class="text-center">YOU</h1>
                <div class="healthbar">
                    <div class="healthbar text-center" style="margin: 0; color: white; background-color:green;"
                                                       :style="{width : playerHealth + '%'}">
                        {{playerHealth}}
                    </div>
                </div>
            </div>
            <div class="small-6 columns">
                <h1 class="text-center">MONSTER</h1>
                <div class="healthbar">
                    <div class="healthbar text-center" style="margin: 0; color: white; background-color:green;"
                                                       :style="{width: monsterHealth + '%'}">
                        {{monsterHealth}}
                    </div>
                </div>
            </div>
        </section>
        <section class="row controls" v-if="!gameIsRunning">
            <div class="small-12 columns">
                <button id="start-game" @click="startGame">START NEW GAME</button>
            </div>
        </section>
        <section class="row controls" v-else>
            <div class="small-12 columns">
                <button id="attack" @click="attack">ATTACK</button>
                <button id="special-attack" @click="sattack" :disabled='specialAttack == 0'>SPECIAL ATTACK ({{specialAttack}})</button>
                <button id="heal" @click="heal" :disabled='Heal == 0'>HEAL ({{Heal}})</button>
                <button id="give-up" @click="giveUp">GIVE UP</button>
            </div>
        </section>
        <section class="row log" v-show="this.Damages.length > 0">
            <div class="small-12 columns">
                <ul>
                    <li v-for="(damage,index) in Damages" :key="index" :class="damage.isPlayer ? 'playerClass' : 'monsterClass'"> 
                   
                        {{damage.text}}
                        
                    </li>
                    
                </ul>
            </div>
        </section>
    </div>
</template>

<script>
export default {
  
  data(){
    return {
              playerHealth : 100,
              monsterHealth : 100,
              gameIsRunning : false,
              specialAttack : 3,
              Heal : 2 ,
              Damages : [],
             
    }
},

methods : {
    startGame(){
        
        this.gameIsRunning = true;
        this.playerHealth = 100;
        this.monsterHealth = 100;
        this.specialAttack = 3;
        this.Heal = 2;
        this.Damages = []
       // console.log(this.gameIsRunning);
    },
    attack(){

        let max =10;
        let min = 3;
        let damage= Math.max(Math.floor(Math.random() * max + 1), min);
        this.monsterHealth -= damage;
        this.Damages.unshift({isPlayer : true , text: 'Player hit Monster for '+ damage });

        if(this.monsterHealth <= 0)
        {
            alert("You Win .. Congrats!!!");
            this.gameIsRunning = false;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            return;
        }

        max = 10 ;
        min = 3;
        damage = Math.max(Math.floor(Math.random() * max + 1), min);
        this.playerHealth -= damage;
        this.Damages.unshift({isPlayer : false , text: 'Monster hit Player for '+ damage });

        if(this.playerHealth <= 0)
        {
            alert('You Lost .. Try Again');
            this.gameIsRunning = false;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            return;
        }
        

    },
    sattack(){
        
        this.specialAttack--;
        
        let max =10;
        let min = 6;
        let damage= Math.max(Math.floor(Math.random() * max + 1), min);
        this.monsterHealth -= damage;
        this.Damages.unshift({isPlayer : true , text: 'Player hit Monster HARD for '+ damage });
        

        if(this.monsterHealth <= 0)
        {
            alert("You Win .. Congrats!!!");
            this.gameIsRunning = false;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            return;
        }

        max = 5 ;
        min = 1;
        damage = Math.max(Math.floor(Math.random() * max + 1), min);
        this.playerHealth -= damage;
        this.Damages.unshift({isPlayer : false , text: 'Monster hit Player for '+ damage });


        if(this.playerHealth <= 0)
        {
            alert('You Lost .. Try Again');
            this.gameIsRunning = false;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            return;
        }
    },
    heal(){
        if(this.playerHealth == 100)
        {
            alert('You are Full of Energy .. Just Fight')
        }
        else if(this.playerHealth > 90)
        {
            this.Damages.unshift({isPlayer : true , text: 'Player Healing Himself By '+ (100 - this.playerHealth)});
            this.playerHealth = 100;
            this.Heal--;
        }else {
            this.Damages.unshift({isPlayer : true , text: 'Player healing himself by 10'});
            this.playerHealth += 10;
            this.Heal--;
        }
        
    },
    giveUp(){
            alert('You Lost .. Try Again');
            this.Damages.unshift({isPlayer : true , text: 'Player gave Up'});
            this.gameIsRunning = false;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            return;
    }
}

}
</script>

<style>
.text-center {
    text-align: center;
}

.healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    
}

.log ul li {
    margin-left: 385px;
    width : 660px;
    border: 1.5px solid black;
    border-radius: 8px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ccbbbb;
}

#give-up:hover {
    background-color: #c7c7c7;
}

.green {
    background-color: green;
}

.red {
    background-color: red;
}
.playerClass {
    background-color : #2196F3;
}
.monsterClass {
    background-color : #FF5722;
}

</style>
