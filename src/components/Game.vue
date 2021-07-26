<template>
    <v-container fluid>
        <!-- TOP BANNER -->
        <v-app-bar dense fixed style="background-color:brown;height:80px">
            <v-row style="margin-top:0%">
                <!-- START OVER BUTTON -->
                <v-col cols="3" style="margin-top:3%">
                    <v-btn @click="restartGame" style="text-align:right;background-color:red;color:white;font-weight:900;margin:0%">
                        START OVER
                    </v-btn>
                </v-col>

                <!-- CURRENT SCORE & MOVES -->
                <v-col cols="3" style="margin-top:2%">
                    <p v-if="moveCount <= 8" style="font-size:1rem;color:gold;font-weight:700;margin:0%">
                        CURRENT MOVES : <span style="color:green;font-weight:900;font-size:1.5rem;">{{ moveCount }}</span>
                    </p>
                    <p v-if="moveCount > 8" style="font-size:1rem;color:gold;font-weight:700;margin:0%">
                        CURRENT MOVES : <span style="color:red;font-weight:900;font-size:1.5rem;"> - {{ (moveCount - 8) * 2 }}</span>
                    </p>
                    <p style="font-size:1rem;color:gold;font-weight:700;margin:0%">
                        BOARD VALUE : <span style="color:silver;font-weight:900;font-size:1.5rem;">{{ boardValue }}</span>
                    </p>
                </v-col>

                <!-- CURRENT TOP 3 SCORES -->
                <v-col cols="6">
                    <v-row style="margin-top:3%;">
                        <v-col cols="3" style="border-right:2px solid black;border-left:2px solid black;">
                            <p style="color:white;margin:0%;font-size:1.2rem;;font-weight:700;">Current Highscores</p>
                        </v-col>
                        <v-col v-for="info in topScores" :key="info.id" cols="3">
                            <p style="margin:0%;color:white;">User: {{ info.username }}</p>
                            <!-- <br /> -->
                            <p style="margin:0%;color:white;">Score: {{ info.score }}</p>
                        </v-col>
                    </v-row>
                </v-col>
            </v-row>
        </v-app-bar>

        <!-- LOADING SCREEN -->
        <v-row v-if="loading === true && !playerVictory">
            <v-col cols="12">
                <p style="color:purple;font-size:4rem;font-weight:900;text-align:center;margin-top:15%;">
                    LOADING!
                </p>
            </v-col>
        </v-row>

        <!-- PAGE CONTENT - LEFT AND RIGHT SIDE -->
        <v-row v-if="loading === false && !playerVictory" style="margin-top:5%">
            <!-- LEFT SIDE - GAME BOARD AND BUTTONS -->
            <v-col cols="6" v-if="!levelUpPanel">
                <!-- GAME BOARD!!! -->
                <v-row style="width: 600px; height: 600px; margin:5%;">
                    <v-col
                        v-for="item in gameBoard3x3"
                        :key="item.id"
                        cols="4"
                        style="
          background-image: '..assets/forest.png';
          border: 1px ridge crimson;
          width: 200px;
          height: 200px;
          padding: 1%;
          background-color: black;
          color: white;
        "
                    >
                        <!-- ALL TERRAIN POSSIBILITIES -->
                        <div v-if="item.terrain === 'plains'" class="plainsbg" style="z-index:-1">
                            <v-img v-if="item.residentPlayer" max-height="75" max-width="75" src="../assets/player_image.png" style="border-radius:20px;"></v-img>

                            <!-- ALL NPC TYPES - CONDITIONAL -->
                            <div v-if="item.residentNpc">
                                <!-- GOBLIN -->
                                <v-img v-if="item.npcType.name === 'Goblin'" max-height="75" max-width="75" src="../assets/npc_goblin_image.png" style="border-radius:20px;"></v-img>
                                <!-- ORC -->
                                <v-img v-if="item.npcType.name === 'Orc'" max-height="75" max-width="75" src="../assets/npc_orc_image.png" style="border-radius:20px;"></v-img>
                                <!-- OGRE -->
                                <v-img v-if="item.npcType.name === 'Ogre'" max-height="75" max-width="75" src="../assets/npc_ogre_image.png" style="border-radius:20px;"></v-img>
                            </div>
                        </div>

                        <div v-if="item.terrain === 'forest'" class="forestbg">
                            <v-img v-if="item.residentPlayer" max-height="75" max-width="75" src="../assets/player_image.png" style="border-radius:20px;"></v-img>

                            <!-- ALL NPC TYPES - CONDITIONAL -->
                            <div v-if="item.residentNpc">
                                <!-- GOBLIN -->
                                <v-img v-if="item.npcType.name === 'Goblin'" max-height="75" max-width="75" src="../assets/npc_goblin_image.png" style="border-radius:20px;"></v-img>
                                <!-- ORC -->
                                <v-img v-if="item.npcType.name === 'Orc'" max-height="75" max-width="75" src="../assets/npc_orc_image.png" style="border-radius:20px;"></v-img>
                                <!-- OGRE -->
                                <v-img v-if="item.npcType.name === 'Ogre'" max-height="75" max-width="75" src="../assets/npc_ogre_image.png" style="border-radius:20px;"></v-img>
                            </div>
                        </div>

                        <div v-if="item.terrain === 'mountains'" class="mountainsbg">
                            <v-img v-if="item.residentPlayer" max-height="75" max-width="75" src="../assets/player_image.png" style="border-radius:20px;"></v-img>

                            <!-- ALL NPC TYPES - CONDITIONAL -->
                            <div v-if="item.residentNpc">
                                <!-- GOBLIN -->
                                <v-img v-if="item.npcType.name === 'Goblin'" max-height="75" max-width="75" src="../assets/npc_goblin_image.png" style="border-radius:20px;"></v-img>
                                <!-- ORC -->
                                <v-img v-if="item.npcType.name === 'Orc'" max-height="75" max-width="75" src="../assets/npc_orc_image.png" style="border-radius:20px;"></v-img>
                                <!-- OGRE -->
                                <v-img v-if="item.npcType.name === 'Ogre'" max-height="75" max-width="75" src="../assets/npc_ogre_image.png" style="border-radius:20px;"></v-img>
                            </div>
                        </div>

                        <div v-if="item.terrain === 'ocean'" class="oceanbg">
                            <v-img v-if="item.residentPlayer" max-height="75" max-width="75" src="../assets/player_image.png" style="border-radius:20px;"></v-img>

                            <!-- ALL NPC TYPES - CONDITIONAL -->
                            <div v-if="item.residentNpc">
                                <!-- GOBLIN -->
                                <v-img v-if="item.npcType.name === 'Goblin'" max-height="75" max-width="75" src="../assets/npc_goblin_image.png" style="border-radius:20px;"></v-img>
                                <!-- ORC -->
                                <v-img v-if="item.npcType.name === 'Orc'" max-height="75" max-width="75" src="../assets/npc_orc_image.png" style="border-radius:20px;"></v-img>
                                <!-- OGRE -->
                                <v-img v-if="item.npcType.name === 'Ogre'" max-height="75" max-width="75" src="../assets/npc_ogre_image.png" style="border-radius:20px;"></v-img>
                            </div>
                        </div>
                    </v-col>
                </v-row>

                <!-- GAME BUTTONS - TOP ROW -->
                <v-row v-if="showBtn && !gameOver" style="width: 600px; margin-left:7%;">
                    <v-col cols="12" :elevation="50">
                        <button
                            id="control_Btn"
                            @click="moveUp()"
                            style="
            border: 2px ridge silver;
            width: 180px;
            height: 45px;
            margin-left: 175px;
            background-color: tan;
          "
                        >
                            Up
                        </button>
                    </v-col>
                </v-row>
                <!-- BOTTOM ROW OF GAME BUTTONS -->
                <v-row v-if="showBtn && !gameOver" style="width: 600px; margin-left:7%;">
                    <v-col cols="4">
                        <button id="control_Btn" @click="moveLeft()" style="border:2px ridge silver;width:130px;height:40px;margin-left:55px;background-color:tan;">
                            Left
                        </button>
                    </v-col>
                    <v-col cols="4">
                        <button id="control_Btn" @click="moveDown()" style="border:2px ridge silver;width:130px;height:40px;background-color:tan;">
                            Down
                        </button>
                    </v-col>
                    <v-col cols="4">
                        <button id="control_Btn" @click="moveRight()" style="border:2px ridge silver;width:130px;height:40px;margin-left:-55px;background-color:tan;">
                            Right
                        </button>
                    </v-col>
                </v-row>
                <v-row v-if="!showBtn" style="width: 600px; margin-left:7%;">
                    <v-col cols="12">
                        <p style="font-size:5rem;color:red;font-weight:900">
                            COMBAT
                        </p>
                    </v-col>
                </v-row>
                <v-row v-if="gameOver" style="width: 600px; margin-left:7%;">
                    <v-col cols="12">
                        <p style="font-size:5rem;color:red;font-weight:900">
                            GAME OVER!
                        </p>
                    </v-col>
                </v-row>
            </v-col>

            <!-- LEVEL UP PANEL -->
            <v-col cols="6" v-if="levelUpPanel">
                <v-row style="border:1px solid gold;margin-top:8%;margin-left:8%;margin-right:5%;height:400px;">
                    <v-col cols="12" style="background-color:rgb(0, 70, 70)">
                        <p style="color:whitesmoke;text-align:center;font-weight:900;font-size:2rem;">You are now level {{ this.player.lvl }}</p>
                        <br />
                        <v-btn @click="playerIncreaseAttack" style="background-color:whitesmoke;font-weight:600;font-size:1.5rem;margin:2%;">
                            Increase Attack
                        </v-btn>
                        <br />
                        <v-btn @click="playerIncreaseArmor" style="background-color:whitesmoke;font-weight:600;font-size:1.5rem;margin:2%;">
                            Increase Armor
                        </v-btn>
                    </v-col>
                </v-row>
            </v-col>

            <!-- RIGHT SIDE -->
            <v-col cols="6">
                <!-- PLAYER AND NPC INFORMATION WINDOWS -->
                <v-row style="margin:5%;border:3px solid gold;">
                    <v-col cols="6" style="border: 1px solid red;color:white;">
                        <!-- THIS WILL SHOW THE COMBAT AND WHAT IS HAPPENING IN THE GAME -->
                        <p>
                            Enemy Type :
                            <span v-if="this.gameBoard3x3[this.player.boardIndex].npcType"> {{ this.gameBoard3x3[this.player.boardIndex].npcType.name }}</span>
                        </p>
                        <p v-if="this.gameBoard3x3[this.player.boardIndex].npcType">
                            Enemy Health :
                            <progress id="npcHealth" value="100" max="100" style="background-color:green;"></progress>
                        </p>
                        <p>
                            Enemy Attack Power :
                            <span v-if="this.gameBoard3x3[this.player.boardIndex].npcType"> {{ this.gameBoard3x3[this.player.boardIndex].npcType.ap }}</span>
                        </p>
                        <p>
                            Enemy Armor Rating :
                            <span v-if="this.gameBoard3x3[this.player.boardIndex].npcType"> {{ this.gameBoard3x3[this.player.boardIndex].npcType.ar }}</span>
                        </p>
                    </v-col>
                    <v-col cols="6" style="border: 1px solid green;color:white;">
                        <!-- THIS WILL SHOW THE CURRENT PLAYER STATS -->
                        <p>Player Level : {{ this.player.lvl }}</p>
                        <p>Player XP-Pool : {{ this.player.xpPool }} / {{ this.player.lvl * 2 }}</p>
                        <p>Player Health : {{ this.player.hp }}</p>
                        <p>
                            Player Health :
                            <progress id="playerHealth" value="100" max="100" style="background-color:green"></progress>
                        </p>
                        <p>Player Attack Power : {{ this.player.ap }}</p>
                        <p>Player Armor Rating : {{ this.player.ar }}</p>
                    </v-col>
                </v-row>

                <!-- BATTLE LOG ARCHIVE EXPANSIONS -->
                <v-row justify="center" style="border:2px solid gold;margin-right:20px;border-radius:7px;">
                    <v-expansion-panels accordion>
                        <v-expansion-panel v-for="(combatLogs, i) in totalCombatText" :key="i">
                            <v-expansion-panel-header style="background-color:brown;color:whitesmoke"> Enemy Encounter {{ i + 1 }} </v-expansion-panel-header>
                            <v-expansion-panel-content style="background-color:rgb(71, 0, 71);">
                                <div v-for="(text, j) in combatLogs" :key="j">
                                    <p style="color:whitesmoke">{{ text }}</p>
                                </div>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </v-row>
            </v-col>
        </v-row>

        <!-- VICTORY SCREEN AND USER NAME INPUT -->
        <v-row v-if="loading === false && playerVictory">
            <v-col cols="12" style="background-color:rgb(114, 88, 23)">
                <v-row>
                    <v-col cols="12">
                        <p style="color:gold;font-size:6rem;font-weight:900;text-align:center;margin-top:15%;">
                            VICTORY!
                        </p>
                    </v-col>
                </v-row>

                <v-row>
                    <v-col cols="12">
                        <p style="color:gold;font-size:3rem;font-weight:600;text-align:center;margin-top:2%;">
                            Please choose 3 capital letters for your name.
                        </p>
                    </v-col>
                </v-row>

                <v-row>
                    <v-col cols="12">
                        <v-text-field v-model="title" type="text" pattern="[A-Z]+" :maxlength="maxlength" v-on:keypress="isLetter($event)" clearable style="width:25%;border:5px ridge gold;margin:auto;padding:15px;"></v-text-field>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col cols="12">
                        <v-btn @click="recordPlayerScoreAfterWin" style="margin-left:46%;background-color:black;color:white;font-size:1.5rem;font-weight:700;">Submit!</v-btn>
                    </v-col>
                </v-row>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import { axios } from '@/plugins/axios'

export default {
    name: 'BlockGameNoCanvas',
    components: {},
    data() {
        return {
            maxlength: 3,
            playerVictory: false,
            moveCount: 0,
            boardValue: 0,
            score: 0,
            levelUpPanel: false,
            gameOver: false,
            showBtn: true,
            // GAME BOARD RELATED STUFF
            // GAME BOARD RELATED STUFF
            // GAME BOARD RELATED STUFF
            gameBoard3x3: [
                {
                    // BOARD INFO
                    id: 0,
                    position: {
                        row: 1,
                        col: 1,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 1,
                    position: {
                        row: 1,
                        col: 2,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 2,
                    position: {
                        row: 1,
                        col: 3,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 3,
                    position: {
                        row: 2,
                        col: 1,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 4,
                    position: {
                        row: 2,
                        col: 2,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 5,
                    position: {
                        row: 2,
                        col: 3,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 6,
                    position: {
                        row: 3,
                        col: 1,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 7,
                    position: {
                        row: 3,
                        col: 2,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 8,
                    position: {
                        row: 3,
                        col: 3,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
            ],
            terrainTypes: ['plains', 'mountains', 'forest', 'ocean'],
            emptyNpcType: {
                id: 0,
                name: 'empty',
                hp: 0,
                ap: 0,
                ar: 0,
            },

            // PLAYERS RELATED STUFF
            // PLAYERS RELATED STUFF
            // PLAYERS RELATED STUFF
            playerPlaced: false,
            playerEncounteredNPCBoardPosition: null,
            player: {
                boardIndex: 6,
                position: {
                    row: null,
                    col: null,
                },
                lvl: 1,
                xpPool: 0,
                hp: 100,
                ap: 2,
                ar: 2,
            },

            // NPC RELATED STUFF
            // NPC RELATED STUFF
            // NPC RELATED STUFF
            npcOnMap: [],
            npcTypes: [
                {
                    id: 1,
                    name: 'Goblin',
                    hp: 100,
                    ap: 1,
                    ar: 0,
                    xpReward: 2,
                },
                {
                    id: 2,
                    name: 'Orc',
                    hp: 100,
                    ap: 2,
                    ar: 2,
                    xpReward: 3,
                },
                {
                    id: 3,
                    name: 'Ogre',
                    hp: 100,
                    ap: 4,
                    ar: 1,
                    xpReward: 6,
                },
            ],
            currentCombatText: [],
            totalCombatText: [],
            npcObjectCopy: null,
            loading: true,
            gameRestartHit: false,
            topScores: [],
            title: null,
        }
    },
    // before Vue renders
    created() {
        // window.addEventListener('keypress', this.wasdMovement)
    },
    destroyed() {
        // window.removeEventListener('keypress', this.wasdMovement)
    },
    // after Vue renders
    async mounted() {
        this.gameStart()
    },
    computed: {},
    methods: {
        // GAME START & GENERATION FUNCTIONS
        gameStart() {
            // First layer should be the terrain
            setTimeout(() => {
                this.setTerrain()
                // Second Layer will set the player Square, for now it will always be the bottom left square
                setTimeout(() => {
                    this.setPlayer()
                    // Third Layer will set NPCs into all remaining squares
                    setTimeout(() => {
                        this.setNPCs()
                        this.gameRestartHit = false
                        // Fourth Layer is NOT currently used
                        setTimeout(() => {
                            this.calculateBoardDifficulty()
                            this.calculateScore()
                            this.getTopScores()
                            this.loading = false
                        }, 100)
                    }, 500)
                }, 500)
            }, 500)
        },
        setTerrain() {
            for (let i = 0; i < this.gameBoard3x3.length; i++) {
                // find a random terrain type, and apply it to the current gameBoard Square
                let randomTerrainType = Math.floor(Math.random() * 4)
                this.gameBoard3x3[i].terrain = this.terrainTypes[randomTerrainType]
            }
        },
        setPlayer() {
            // FIND A RANDOM SQUARE TO PLACE THE PLAYER INTO
            // let randomPlayerSquare = Math.floor(Math.random() * 9) + 1;
            this.playerPlaced = true
            this.gameBoard3x3[6].residentPlayer = true
            this.player.position.row = this.gameBoard3x3[6].position.row
            this.player.position.col = this.gameBoard3x3[6].position.col
        },
        setNPCs() {
            for (let i = 0; i < this.gameBoard3x3.length; i++) {
                if (this.gameBoard3x3[i].residentPlayer === false) {
                    // FIND A RANDOM NPC TYPE TO PLACE AT THIS LOCATION
                    let randomNpcType = Math.floor(Math.random() * 3)

                    this.gameBoard3x3[i].residentNpc = true
                    this.gameBoard3x3[i].npcType = this.npcTypes[randomNpcType]
                }
            }
        },
        getTopScores() {
            axios
                .get('http://localhost:6060/api/users/champs')
                .then(res => {
                    this.topScores = res.data
                    console.log(res.data, 'this should be the list of current users on the database')
                })
                .catch(err => {
                    console.log(err, 'error')
                })
        },

        // MOVEMENT RELATED FUNCTIONS
        moveUp() {
            let startingPlace = null
            let endingPlace = null

            if (this.player.position.row === 1) {
                return
            } else {
                // LOOP TO FIND STARTING PLACE
                for (let k = 0; k < this.gameBoard3x3.length; k++) {
                    let posRowTile = this.gameBoard3x3[k].position.row
                    let posColTile = this.gameBoard3x3[k].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc) {
                        startingPlace = k
                    }
                }
                // LOOP TO FIND ENDING PLACE
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc - 1 && posColTile == posColPc) {
                        endingPlace = i
                    }
                }
                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[endingPlace].position.row
                this.player.position.col = this.gameBoard3x3[endingPlace].position.col
                this.player.boardIndex = endingPlace
                this.moveCount += 1
            }
            this.checkForEnemy()
            setTimeout(() => {
                this.combatComplete(endingPlace)
            }, 1000)
        },
        moveLeft() {
            let startingPlace = null
            let endingPlace = null

            if (this.player.position.col === 1) {
                return
            } else {
                // LOOP TO FIND STARTING PLACE - SAME FOR ALL MOVEMENT FUNCTIONS
                for (let k = 0; k < this.gameBoard3x3.length; k++) {
                    let posRowTile = this.gameBoard3x3[k].position.row
                    let posColTile = this.gameBoard3x3[k].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc) {
                        startingPlace = k
                    }
                }
                // LOOP TO FIND ENDING PLACE - ADJUSTED TILE POSITION FOR DESIRED LOCATION
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc - 1) {
                        endingPlace = i
                    }
                }
                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[endingPlace].position.row
                this.player.position.col = this.gameBoard3x3[endingPlace].position.col
                this.player.boardIndex = endingPlace
                this.moveCount += 1
            }
            this.checkForEnemy()
            setTimeout(() => {
                this.combatComplete(endingPlace)
            }, 1000)
        },
        moveRight() {
            let startingPlace = null
            let endingPlace = null

            if (this.player.position.col === 3) {
                return
            } else {
                // LOOP TO FIND STARTING PLACE - SAME FOR ALL MOVEMENT FUNCTIONS
                for (let k = 0; k < this.gameBoard3x3.length; k++) {
                    let posRowTile = this.gameBoard3x3[k].position.row
                    let posColTile = this.gameBoard3x3[k].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc) {
                        startingPlace = k
                    }
                }
                // LOOP TO FIND ENDING PLACE - ADJUSTED TILE POSITION FOR DESIRED LOCATION
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc + 1) {
                        endingPlace = i
                    }
                }
                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[endingPlace].position.row
                this.player.position.col = this.gameBoard3x3[endingPlace].position.col
                this.player.boardIndex = endingPlace
                this.moveCount += 1
            }
            this.checkForEnemy()
            setTimeout(() => {
                this.combatComplete(endingPlace)
            }, 1000)
        },
        moveDown() {
            let startingPlace = null
            let endingPlace = null

            if (this.player.position.row === 3) {
                return
            } else {
                // LOOP TO FIND STARTING PLACE - SAME FOR ALL MOVEMENT FUNCTIONS
                for (let k = 0; k < this.gameBoard3x3.length; k++) {
                    let posRowTile = this.gameBoard3x3[k].position.row
                    let posColTile = this.gameBoard3x3[k].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc) {
                        startingPlace = k
                    }
                }
                // LOOP TO FIND ENDING PLACE - ADJUSTED TILE POSITION FOR DESIRED LOCATION
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc + 1 && posColTile == posColPc) {
                        endingPlace = i
                    }
                }
                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[endingPlace].position.row
                this.player.position.col = this.gameBoard3x3[endingPlace].position.col
                this.player.boardIndex = endingPlace
                this.moveCount += 1
            }
            this.checkForEnemy()
            setTimeout(() => {
                this.combatComplete(endingPlace)
            }, 1000)
        },

        // COMBAT RELATED FUNCTIONS
        checkForEnemy() {
            if (this.gameBoard3x3[this.player.boardIndex].residentNpc === true) {
                this.showBtn = false
            }
        },
        shootingPhase() {
            // player shoots

            // get player attack damage, minus the npc armor
            let playerAttack = this.player.ap * this.randomNumber() - this.npcObjectCopy.ar
            if (playerAttack < 0) {
                playerAttack = 0
            }

            // edit the npc health by removing the player damage done
            this.npcObjectCopy.hp = this.npcObjectCopy.hp - playerAttack
            let playerCombatLog = 'You strike the ' + this.npcObjectCopy.name + ' for ' + playerAttack + ' damage!'

            let npcHpBar = document.getElementById('npcHealth')
            npcHpBar.value = this.npcObjectCopy.hp

            // get npc attack damage, minus the player armor
            let npcAttack = this.npcObjectCopy.ap * this.randomNumber() - this.player.ar
            if (npcAttack < 0) {
                npcAttack = 0
            }

            // edit the player health by removing the npc damage done
            this.player.hp = this.player.hp - npcAttack
            let npcCombatLog = 'The ' + this.npcObjectCopy.name + ' strikes you for ' + npcAttack + ' damage!'

            let playerHpBar = document.getElementById('playerHealth')
            playerHpBar.value = this.player.hp

            // push combat logs
            this.currentCombatText.push(playerCombatLog)
            this.currentCombatText.push(npcCombatLog)
        },
        combatComplete(boardIndex) {
            if (this.gameBoard3x3[boardIndex].npcType.name !== 'empty') {
                // MAKE A COPY OF THE NPC REFERENCE IN THE GAME BOARD
                this.npcObjectCopy = Object.assign({}, this.gameBoard3x3[boardIndex].npcType)

                let combatLoop = setInterval(() => {
                    if (this.gameRestartHit === true) {
                        clearInterval(combatLoop)
                    }
                    this.shootingPhase(boardIndex)

                    let humanHP = this.player.hp
                    let compHP = this.npcObjectCopy.hp
                    let bothDieText = 'Both the Hero and the enemy have died in combat!'
                    let npcDieText = 'The Hero slayed the enemy!'
                    let playerDieText = 'The enemy has slain the Hero!'

                    if (humanHP <= 0 && compHP <= 0) {
                        // IF BOTH THE PLAYER AND NPC DIE
                        this.currentCombatText.push(bothDieText)
                        this.totalCombatText.push(this.currentCombatText)
                        this.currentCombatText = []
                        this.npcObjectCopy = null
                        this.showBtn = true
                        this.gameOver = true
                        clearInterval(combatLoop)
                    } else if (compHP <= 0) {
                        // IF THE NPC DIES AND PLAYER WINS
                        this.addKillXpToPlayer(this.npcObjectCopy.xpReward)
                        this.didPlayerLevelUp()
                        this.calculateScore()
                        this.checkForVictory()
                        this.currentCombatText.push(npcDieText)
                        this.totalCombatText.push(this.currentCombatText)
                        this.currentCombatText = []
                        this.gameBoard3x3[boardIndex].npcType = this.emptyNpcType
                        this.gameBoard3x3[boardIndex].residentNpc = false
                        this.npcObjectCopy = null
                        this.showBtn = true
                        clearInterval(combatLoop)
                    } else if (humanHP <= 0) {
                        // IF THE PLAYER DIES AND NPC WINS
                        this.currentCombatText.push(playerDieText)
                        this.totalCombatText.push(this.currentCombatText)
                        this.currentCombatText = []
                        this.npcObjectCopy = null
                        this.showBtn = true
                        this.gameOver = true
                        clearInterval(combatLoop)
                    }
                }, 1000)
            }
        },

        // LEVEL UP FUNCTIONS
        didPlayerLevelUp() {
            let requirementToLevel = this.player.lvl * 2
            let currentXp = this.player.xpPool
            let playerHpBarRefresh = document.getElementById('playerHealth')

            // small 25 point heal to player regardless of win
            if (this.player.hp + 25 > 100) {
                this.player.hp = 100
            } else {
                this.player.hp += 25
            }
            playerHpBarRefresh.value = this.player.hp

            // first deal with the level gain
            // did the player level?
            if (currentXp >= requirementToLevel) {
                // player gains level
                this.player.lvl = this.player.lvl + 1
                this.player.hp = 100
                playerHpBarRefresh.value = this.player.hp
                this.levelUpPanel = true

                this.player.xpPool -= requirementToLevel

                // check for remainder
                let checkLeftOver = currentXp - requirementToLevel
                if (checkLeftOver > 0) {
                    this.player.xpPool = checkLeftOver
                }
            }
        },
        addKillXpToPlayer(xpReward) {
            this.player.xpPool = this.player.xpPool + xpReward
        },
        playerIncreaseAttack() {
            this.player.ap += 1
            this.levelUpPanel = false
        },
        playerIncreaseArmor() {
            this.player.ar += 1
            this.levelUpPanel = false
        },

        // SCORE RELATED FUNCTIONS
        calculateBoardDifficulty() {
            for (let i = 0; i < this.gameBoard3x3.length; i++) {
                if (this.gameBoard3x3[i].npcType.name === 'Goblin') {
                    this.boardValue += 2
                } else if (this.gameBoard3x3[i].npcType.name === 'Orc') {
                    this.boardValue += 3
                } else if (this.gameBoard3x3[i].npcType.name === 'Ogre') {
                    this.boardValue += 5
                } else {
                    this.boardValue += 0
                }
            }
        },
        calculateScore() {
            if (this.gameOver === true) {
                this.score = 0
            } else {
                if (this.moveCount <= 8) {
                    this.score = this.boardValue
                } else {
                    let movesOver = this.moveCount - 8
                    this.score = this.boardValue - movesOver * 2
                }
            }
        },

        // WIN OR LOSE FUNCTIONS
        checkForVictory() {
            let npcNumberRemaining = 8
            if (this.gameOver === false) {
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    if (this.gameBoard3x3[i].npcType.name === 'empty') {
                        npcNumberRemaining -= 1
                    }
                }
                console.log(npcNumberRemaining, 'remaining NPCs')
                if (npcNumberRemaining <= 0) {
                    this.playerVictory = true
                    console.log('VICTORY')
                }
            }
        },
        recordPlayerScoreAfterWin() {
            if (this.title === null || this.title.length < 3) {
                alert('Please fill in 3 letters your user name')
            } else if (this.playerVictory === true) {
                axios
                    .post('http://localhost:6060/api/users/post', {
                        username: this.title,
                        score: this.score,
                    })
                    .then(res => {
                        console.log(res.data)
                        res.status(201).json({ message: 'user info posted successfully' })
                    })
                    .catch(err => {
                        console.log(err, 'user info and score not posted to database')
                    })
            }
            setTimeout(() => {
                this.player.xpPool = 0
                this.restartGame()
            }, 500)
        },
        restartGame() {
            this.playerVictory = false
            this.score = 0
            this.boardValue = 0
            this.moveCount = 0
            this.gameOver = false
            this.gameRestartHit = true
            this.loading = true
            this.showBtn = true
            this.playerPlaced = false
            this.playerEncounteredNPCBoardPosition = null
            this.npcOnMap = []
            this.currentCombatText = []
            this.totalCombatText = []
            this.npcObjectCopy = null

            this.player = {
                boardIndex: 6,
                position: {
                    row: null,
                    col: null,
                },
                lvl: 1,
                xpPool: 0,
                hp: 100,
                ap: 2,
                ar: 2,
            }

            this.gameBoard3x3 = [
                {
                    // BOARD INFO
                    id: 0,
                    position: {
                        row: 1,
                        col: 1,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 1,
                    position: {
                        row: 1,
                        col: 2,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 2,
                    position: {
                        row: 1,
                        col: 3,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 3,
                    position: {
                        row: 2,
                        col: 1,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 4,
                    position: {
                        row: 2,
                        col: 2,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 5,
                    position: {
                        row: 2,
                        col: 3,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 6,
                    position: {
                        row: 3,
                        col: 1,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 7,
                    position: {
                        row: 3,
                        col: 2,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
                {
                    // BOARD INFO
                    id: 8,
                    position: {
                        row: 3,
                        col: 3,
                    },
                    terrain: null,
                    // NPC BOARD INFO
                    npcType: {
                        id: 0,
                        name: 'empty',
                        hp: 0,
                        ap: 0,
                        ar: 0,
                    },
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
            ]

            this.gameStart()
        },
        isLetter(e) {
            let char = String.fromCharCode(e.keyCode) // Get the character code
            if (/^[A-Z]/.test(char)) return true
            // Match with regex
            else e.preventDefault() // If not match, don't add to input text
        },

        // RANDOM RNG
        randomNumber() {
            return Math.floor(Math.random() * 5 + 1)
        },
    },
}
</script>

<style scoped>
#control_Btn {
    box-shadow: 3px 3px 5px;
}

.plainsbg {
    height: 185px;
    background: url('../assets/plains.png') no-repeat center center;
    background-size: cover;
}
.forestbg {
    height: 185px;
    background: url('../assets/forest.png') no-repeat center center;
    background-size: cover;
}
.mountainsbg {
    height: 185px;
    background: url('../assets/mountains.png') no-repeat center center;
    background-size: cover;
}
.oceanbg {
    height: 185px;
    background: url('../assets/ocean.png') no-repeat center center;
    background-size: cover;
}
.goblin_npc {
    z-index: 10;
    height: 75px;
    width: 75px;
    position: absolute;
    image: '../assets/npc_goblin_image.png';
}
</style>
