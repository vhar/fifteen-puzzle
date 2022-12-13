<template lang="">
    <div class="fifteen">
        <kh-dialog v-model:show="dialogVisible"><div v-html="dialogContent"></div></kh-dialog>
        <div class="game-status">
            <kh-button @click="soundSwitch" class="sound-switch">
                <div v-if="gameSound">&#128263;</div>
                <div v-else="gameSound">&#128266;</div>
            </kh-button>
            <div>Осталось времени</div>
            <kh-timer :time="timeLeft"></kh-timer>
            <div>Сделано ходов</div>
            <div>{{gameMoves}}</div>
        </div>
        <div class="game-field">
            <template  v-for="knuckle in knuckles">
                <kh-knuckle v-if="knuckle.trim() != ''" @click="knuckleMove($event)">{{knuckle}}</kh-knuckle>
                <kh-knuckle v-else class="blank"></kh-knuckle>
            </template>
        </div>
        <div class="game-restart">
            <kh-button @click="gameRestart">Перемешать</kh-button>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            dialogVisible: false,
            knuckles: ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15"," "],
            gameResult: null,
            gameStarted: false,
            gameSound: false,
            gameTime: 0,
            gameMoves: 0,
            gameTimer: null,
            dialogContent: "",
            minutesWord: ["минуту", "минуты", "минут"],
            secondsWord: ["секунду", "секунды", "секунд"],
            movesWord: ["ход", "хода", "ходов"],
            soundClick: new Audio(require('./sounds/click.mp3')),
            soundBackground: new Audio(require('./sounds/background.mp3')),
        }
    },
    computed: {
        timeLeft () {
            let time = this.gameTime / 60
            let minutes = parseInt(time)
            let seconds = Math.round((time - minutes) * 60)
            return minutes + ":" + (Math.floor(seconds / 10) % 10) + (seconds % 10);
        }
    },
    mounted() {
        this.soundBackground.loop = true;
        this.soundBackground.volume = .3;
        this.welcomeDialog();
    },
    methods: {
        welcomeDialog() {
            this.dialogContent = 'Для начала игры щелкните по кнопке "Перемешать". <br> Для перемещения кликните по нужной костяшке.';
            this.dialogVisible = true;
        },
        failedDialog() {
            this.dialogContent = "К сожалению, отведенное на кон время закончилось. <br>Попробуйте еще раз, Вам обязательно повезет!";
            this.dialogVisible = true;
        },
        congradulationDialog() {
            let minutesWord;
            let secondsWord;
            let movesWord;

            let time    = (1800 - this.gameTime) / 60;
            let minutes = parseInt(time);
            let seconds = Math.round((time - minutes) * 60);

            switch (minutes.toString().slice(-1)[0]) {
                case "1":
                    minutesWord = this.minutesWord[0];
                    break;
                case "2":
                case "3":
                case "4":
                    minutesWord = this.minutesWord[1];
                    break;
                default:
                    minutesWord = this.minutesWord[2];
            }

            if (minutes.toString().length > 1 && minutes.toString().slice(-2)[0] == 1) {
                console.log(minutes);
                minutesWord = this.minutesWord[2];
            }

            switch (seconds.toString().slice(-1)[0]) {
                case "1":
                    secondsWord = this.secondsWord[0];
                    break;
                case "2":
                case "3":
                case "4":
                    secondsWord = this.secondsWord[1];
                    break;
                default:
                    secondsWord = this.secondsWord[2];
            }

            if (seconds.toString().length > 1 && seconds.toString().slice(-2)[0] == 1) {
                secondsWord = this.secondsWord[2];
            }

            switch (this.gameMoves.toString().slice(-1)[0]) {
                case "1":
                    movesWord = this.movesWord[0];
                    break;
                case "2":
                case "3":
                case "4":
                    movesWord = this.movesWord[1];
                    break;
                default:
                    movesWord = this.movesWord[2];
            }

            if (this.gameMoves.toString().length > 1 && this.gameMoves.toString().slice(-2)[0] == 1) {
                movesWord = this.movesWord[2];
            }

            this.dialogContent = `<center>Поздравляем!!!</center><br>Вам удалось собрать головоломку за ${minutes} ${minutesWord} ${seconds} ${secondsWord}, сделав ${this.gameMoves} ${movesWord}`;
            this.dialogVisible = true;
        },
        gameClear() {
            this.gameTime    = 0;
            this.gameMoves   = 0;
            this.knuckles    = ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15"," "];
            this.gameResult  = null;
            this.gameStarted = false;
            if (this.gameTimer) {
                clearInterval(this.gameTimer);
            }
            this.gameTimer = null;
        },
        gameRestart() {
            if (this.gameSound) {
                this.soundClick.play();
            }

            this.gameClear();
            this.knuckles = this.knuckles.sort((a,b) => Math.random() - 0.3);
            this.gameResult = this.knuckles.join("");
            this.gameTime = 1800;
            this.gameStarted = true;

            this.gameTimer = setInterval(() => {
                if (this.gameStarted) {
                    this.gameTime--;

                    if (this.gameTime < 1) {
                        this.failedDialog();
                        this.gameClear();
                    }
                }
            }, 1000);
        },
        knuckleMove(event) {
            if (this.gameStarted) {
                let idxFrom  = this.knuckles.indexOf(event.target.innerText);
                let fromCell = {
                    x: Math.floor(idxFrom / 4),
                    y: idxFrom % 4
                };

                let idxTo  = this.knuckles.indexOf(" ");
                let toCell = {
                    x: Math.floor(idxTo / 4),
                    y: idxTo % 4
                };

                if ( 
                    ( (Math.abs(fromCell.x - toCell.x) <= 1) && (Math.abs(fromCell.y - toCell.y) < 1) ) 
                    || 
                    ( (Math.abs(fromCell.y - toCell.y) <= 1) && (Math.abs(fromCell.x - toCell.x) < 1) ) 
                ) {
                    if (this.gameSound) {
                        this.soundClick.play();
                    }
                    this.knuckles[idxTo] = event.target.innerText;
                    this.knuckles[idxFrom] = " ";
                    this.gameResult = this.knuckles.join("");
                    this.gameMoves++;

                    if(this.gameResult == "123456789101112131415 ") {
                        this.gameStarted = false;
                        this.congradulationDialog();
                        this.gameClear();
                    }
                }
            } else {
                this.welcomeDialog();
            }
        },
        soundSwitch() {
            this.gameSound = !this.gameSound;
            if (this.gameSound) {
                this.soundBackground.play();
            } else {
                this.soundBackground.pause();
            }
        }
    }
}
</script>
<style>
@font-face {
    font-family: 'Oswald';
    src: url('~@/fonts/Oswald-Bold.woff2') format('woff2');
    font-weight: bold;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: 'Oswald';
    src: url('~@/fonts/Oswald-ExtraLight.woff2') format('woff2');
    font-weight: 200;
    font-style: normal;
    font-display: swap;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    border: none;
}
html, body {
    width: 100%;
    height: 100%;
}
body {
    position: relative;
    min-height: 420px;
    min-width: 320px;
}
.fifteen {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #b34c01;
    text-transform: uppercase;
    color: #ddd;
    font: 200 18px/24px Oswald;
}
.game-status {
    display: grid;
    grid-template-columns: 53px auto 36px;
    margin: 5px 5px 0;
    text-align: right;
    gap: 5px;
    user-select: none;
}
.game-field {
    display: grid;
    width: 320px;
    height: 320px;
    padding: 5px;
    gap: 2px;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
}
.game-restart {
    display: flex;
    justify-content: center;
    padding: 0 5px 5px;
}
.sound-switch {
    grid-row: 1 / 3;
    font-size: 32px;
    line-height: 34px;
}
</style>
