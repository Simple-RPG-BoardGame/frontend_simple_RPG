<template>
    <v-container fluid>
        <v-row>
            <!-- LEFT SIDE -->
            <v-col cols="6">
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
                        <div v-if="item.terrain === 'plains'" class="plainsbg">
                            <p style="color: white; padding: 5%">
                                {{ item.terrain }}
                            </p>
                            <p v-if="item.residentPlayer">PLAYER Position</p>
                            <p v-if="item.residentNpc">
                                {{ item.npcType.name }}
                            </p>
                        </div>

                        <div v-if="item.terrain === 'forest'" class="forestbg">
                            <p style="color: white; padding: 5%">
                                {{ item.terrain }}
                            </p>
                            <p v-if="item.residentPlayer">PLAYER Position</p>
                            <p v-if="item.residentNpc">
                                {{ item.npcType.name }}
                            </p>
                        </div>

                        <div
                            v-if="item.terrain === 'mountains'"
                            class="mountainsbg"
                        >
                            <p style="color: white; padding: 5%">
                                {{ item.terrain }}
                            </p>
                            <p v-if="item.residentPlayer">PLAYER Position</p>
                            <p v-if="item.residentNpc">
                                {{ item.npcType.name }}
                            </p>
                        </div>

                        <div v-if="item.terrain === 'ocean'" class="oceanbg">
                            <p style="color: white; padding: 5%">
                                {{ item.terrain }}
                            </p>
                            <p v-if="item.residentPlayer">PLAYER Position</p>
                            <p v-if="item.residentNpc">
                                {{ item.npcType.name }}
                            </p>
                        </div>
                    </v-col>
                </v-row>

                <!-- GAME BUTTONS - TOP ROW -->
                <v-row
                    v-if="showBtn && !gameOver"
                    style="width: 600px; margin-left:7%;"
                >
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
                <v-row
                    v-if="showBtn && !gameOver"
                    style="width: 600px; margin-left:7%;"
                >
                    <v-col cols="4">
                        <button
                            id="control_Btn"
                            @click="moveLeft()"
                            style="border:2px ridge silver;width:130px;height:40px;margin-left:55px;background-color:tan;"
                        >
                            Left
                        </button>
                    </v-col>
                    <v-col cols="4">
                        <button
                            id="control_Btn"
                            @click="moveDown()"
                            style="border:2px ridge silver;width:130px;height:40px;background-color:tan;"
                        >
                            Down
                        </button>
                    </v-col>
                    <v-col cols="4">
                        <button
                            id="control_Btn"
                            @click="moveRight()"
                            style="border:2px ridge silver;width:130px;height:40px;margin-left:-55px;background-color:tan;"
                        >
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

            <!-- RIGHT SIDE -->
            <v-col cols="6">
                <!-- PLAYER AND NPC INFORMATION WINDOWS -->
                <v-row style="margin:5%;border:3px solid gold;">
                    <v-col cols="6" style="border: 1px solid red;color:white;">
                        <!-- THIS WILL SHOW THE COMBAT AND WHAT IS HAPPENING IN THE GAME -->
                        <p>
                            Enemy Type :
                            <span
                                v-if="
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType
                                "
                            >
                                {{
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType.name
                                }}</span
                            >
                        </p>
                        <p>
                            Enemy Health :
                            <span
                                v-if="
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType
                                "
                            >
                                {{
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType.hp
                                }}</span
                            >
                        </p>
                        <p>
                            Enemy Attack Power :
                            <span
                                v-if="
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType
                                "
                            >
                                {{
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType.ap
                                }}</span
                            >
                        </p>
                        <p>
                            Enemy Armor Rating :
                            <span
                                v-if="
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType
                                "
                            >
                                {{
                                    this.gameBoard3x3[this.player.boardIndex]
                                        .npcType.ar
                                }}</span
                            >
                        </p>
                    </v-col>
                    <v-col
                        cols="6"
                        style="border: 1px solid green;color:white;"
                    >
                        <!-- THIS WILL SHOW THE CURRENT PLAYER STATS -->
                        <p>Player Level : {{ this.player.lvl }}</p>
                        <p>Player Health : {{ this.player.hp }}</p>
                        <p>Player Attack Power : {{ this.player.ap }}</p>
                        <p>Player Armor Rating : {{ this.player.ar }}</p>
                    </v-col>
                </v-row>

                <!-- BATTLE LOG ARCHIVE EXPANSIONS -->
                <v-row
                    justify="center"
                    style="border:2px solid gold;margin-right:20px;border-radius:7px;"
                >
                    <v-expansion-panels accordion>
                        <v-expansion-panel
                            v-for="(combatLogs, i) in totalCombatText"
                            :key="i"
                        >
                            <v-expansion-panel-header
                                style="background-color:brown;color:whitesmoke"
                            >
                                Enemy Encounter {{ i + 1 }}
                            </v-expansion-panel-header>
                            <v-expansion-panel-content
                                style="background-color:rgb(71, 0, 71);"
                            >
                                <div v-for="(text, j) in combatLogs" :key="j">
                                    <p style="color:whitesmoke">{{ text }}</p>
                                </div>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </v-row>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'BlockGameNoCanvas',
    components: {},
    data() {
        return {
            gameOver: false,
            showBtn: true,
            test: '..assets/forest.png',
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
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
                    npcType: null,
                    residentNpc: false,
                    // PLAYER BOARD INFO
                    residentPlayer: false,
                },
            ],
            terrainTypes: ['plains', 'mountains', 'forest', 'ocean'],

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
                ap: 1,
                ar: 1,
            },

            // NPC RELATED STUFF
            // NPC RELATED STUFF
            // NPC RELATED STUFF
            npcOnMap: [],
            npcTypes: [
                {
                    id: 1,
                    name: 'Goblin',
                    hp: 50,
                    ap: 1,
                    ar: 1,
                    xpReward: 1,
                },
                {
                    id: 2,
                    name: 'Orc',
                    hp: 100,
                    ap: 2,
                    ar: 2,
                    xpReward: 2,
                },
                {
                    id: 3,
                    name: 'Ogre',
                    hp: 200,
                    ap: 3,
                    ar: 3,
                    xpReward: 5,
                },
            ],
            testArr1: [1, 2],
            testArr2: [1, 2],
            currentCombatText: [],
            totalCombatText: [],
        }
    },
    // before Vue renders
    created() {
        // this.socket = io('http://localhost:3000')
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
        // GAME START
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

                        // Fourth Layer is NOT currently used
                        // setTimeout(() => {}, 1000)
                    }, 1000)
                }, 1000)
            }, 1000)
            console.log(this.gameBoard3x3)
        },
        setTerrain() {
            for (let i = 0; i < this.gameBoard3x3.length; i++) {
                // find a random terrain type, and apply it to the current gameBoard Square
                let randomTerrainType = Math.floor(Math.random() * 4)
                this.gameBoard3x3[i].terrain = this.terrainTypes[
                    randomTerrainType
                ]
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
                        // console.log('STARTING TILE POSITION IS', k)
                        startingPlace = k
                    }
                }
                // console.log(startingPlace, 'starting place? should be 6')

                // LOOP TO FIND ENDING PLACE
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc - 1 && posColTile == posColPc) {
                        // console.log('ENDING TILE POSITION IS', i)
                        endingPlace = i
                    }
                }
                // console.log(endingPlace, 'ending place? should be 3')

                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                // tile edits
                // this.gameBoard3x3[endingPlace].residentNpc = false
                // this.gameBoard3x3[endingPlace].npcType = null
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[
                    endingPlace
                ].position.row
                this.player.position.col = this.gameBoard3x3[
                    endingPlace
                ].position.col
                this.player.boardIndex = endingPlace
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
                        // console.log('STARTING TILE POSITION IS', k)
                        startingPlace = k
                    }
                }
                // console.log(startingPlace, 'starting place? should be 6')

                // LOOP TO FIND ENDING PLACE - ADJUSTED TILE POSITION FOR DESIRED LOCATION
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc - 1) {
                        // console.log('ENDING TILE POSITION IS', i)
                        endingPlace = i
                    }
                }
                // console.log(endingPlace, 'ending place? should be 3')

                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                // tile edits
                // this.gameBoard3x3[endingPlace].residentNpc = false
                // this.gameBoard3x3[endingPlace].npcType = null
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[
                    endingPlace
                ].position.row
                this.player.position.col = this.gameBoard3x3[
                    endingPlace
                ].position.col
                this.player.boardIndex = endingPlace
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
                        // console.log('STARTING TILE POSITION IS', k)
                        startingPlace = k
                    }
                }
                // console.log(startingPlace, 'starting place? should be 6')

                // LOOP TO FIND ENDING PLACE - ADJUSTED TILE POSITION FOR DESIRED LOCATION
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc && posColTile == posColPc + 1) {
                        // console.log('ENDING TILE POSITION IS', i)
                        endingPlace = i
                    }
                }
                // console.log(endingPlace, 'ending place? should be 3')

                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                // tile edits
                // this.gameBoard3x3[endingPlace].residentNpc = false
                // this.gameBoard3x3[endingPlace].npcType = null
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[
                    endingPlace
                ].position.row
                this.player.position.col = this.gameBoard3x3[
                    endingPlace
                ].position.col
                this.player.boardIndex = endingPlace
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
                        // console.log('STARTING TILE POSITION IS', k)
                        startingPlace = k
                    }
                }
                // console.log(startingPlace, 'starting place? should be 6')

                // LOOP TO FIND ENDING PLACE - ADJUSTED TILE POSITION FOR DESIRED LOCATION
                for (let i = 0; i < this.gameBoard3x3.length; i++) {
                    let posRowTile = this.gameBoard3x3[i].position.row
                    let posColTile = this.gameBoard3x3[i].position.col

                    let posRowPc = this.player.position.row
                    let posColPc = this.player.position.col
                    if (posRowTile == posRowPc + 1 && posColTile == posColPc) {
                        // console.log('ENDING TILE POSITION IS', i)
                        endingPlace = i
                    }
                }
                // console.log(endingPlace, 'ending place? should be 3')

                // EDITS TO STARTING LOCATION
                this.gameBoard3x3[startingPlace].residentPlayer = false

                // EDITS TO ENDING LOCATION
                // tile edits
                // this.gameBoard3x3[endingPlace].residentNpc = false
                // this.gameBoard3x3[endingPlace].npcType = null
                this.gameBoard3x3[endingPlace].residentPlayer = true
                // player edits
                this.player.position.row = this.gameBoard3x3[
                    endingPlace
                ].position.row
                this.player.position.col = this.gameBoard3x3[
                    endingPlace
                ].position.col
                this.player.boardIndex = endingPlace
            }
            this.checkForEnemy()
            setTimeout(() => {
                this.combatComplete(endingPlace)
            }, 1000)
        },

        // COMBAT RELATED FUNCTIONS
        // for reference this.npcTypes[1,2,3].name === 'Goblin' yada yada
        // encounterGoblin(positionNPC){
        //     if(positionNPC === )
        // }
        checkForEnemy() {
            if (
                this.gameBoard3x3[this.player.boardIndex].residentNpc === true
            ) {
                this.showBtn = false
            }
        },
        combatComplete(boardIndex) {
            let combatLoop = setInterval(() => {
                this.shootingPhase(boardIndex)

                let humanHP = this.player.hp
                let compHP = this.gameBoard3x3[boardIndex].npcType.hp
                let bothDieText =
                    'Both the Hero and the enemy have died in combat!'
                let npcDieText = 'The Hero slayed the enemy!'
                let playerDieText = 'The enemy has slain the Hero!'

                if (humanHP <= 0 && compHP <= 0) {
                    // IF BOTH THE PLAYER AND NPC DIE
                    this.currentCombatText.push(bothDieText)
                    this.totalCombatText.push(this.currentCombatText)
                    this.currentCombatText = []
                    this.showBtn = true
                    clearInterval(combatLoop)
                } else if (compHP <= 0) {
                    // IF THE NPC DIES AND PLAYER WINS
                    this.currentCombatText.push(npcDieText)
                    this.totalCombatText.push(this.currentCombatText)
                    this.currentCombatText = []
                    this.showBtn = true
                    clearInterval(combatLoop)
                } else if (humanHP <= 0) {
                    // IF THE PLAYER DIES AND NPC WINS
                    this.currentCombatText.push(playerDieText)
                    this.totalCombatText.push(this.currentCombatText)
                    this.currentCombatText = []
                    this.showBtn = true
                    this.gameOver = true
                    clearInterval(combatLoop)
                }
            }, 1000)
        },
        shootingPhase(boardIndex) {
            // player shoots
            let playerAttack = this.player.ap * this.randomNumber()
            this.gameBoard3x3[boardIndex].npcType.hp =
                this.gameBoard3x3[boardIndex].npcType.hp - playerAttack
            let playerCombatLog =
                'You strike the ' +
                this.gameBoard3x3[boardIndex].npcType.name +
                ' for ' +
                playerAttack +
                ' damage!'

            // npc shoots
            let npcAttack =
                this.gameBoard3x3[boardIndex].npcType.ap * this.randomNumber()
            this.player.hp = this.player.hp - npcAttack
            let npcCombatLog =
                'The ' +
                this.gameBoard3x3[boardIndex].npcType.name +
                ' strikes you for ' +
                npcAttack +
                ' damage!'

            // push combat logs
            this.currentCombatText.push(playerCombatLog)
            this.currentCombatText.push(npcCombatLog)
        },
        randomNumber() {
            return Math.floor(Math.random() * 10 + 1)
        },
        playerLevelUp(xpAmount) {
            this.player.lvl += xpAmount
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
</style>
