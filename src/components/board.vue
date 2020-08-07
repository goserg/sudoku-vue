<template>
    <div>
        <div class="title">
            Sudoku
        </div>
        <div class="board">
            <div
                    :class='index===active?"cell active":"cell"'
                    v-for="(cell, index) in gameBoard"
                    v-bind:key="index"
                    @click="setActive(index)"
            >
                {{ cell }}
            </div>
        </div>
        <div class="numbers_pad">
            <div
                    v-for="n in numbers"
                    :class='isValid(n) || active == null?"number valid":"number invalid"'
                    v-bind:key="n"
                    @click="inputNumber(n)"
            >
                {{ n }}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "board",
        data() {
            return {
                active: null,
                numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9],
                gameBoard: [
                    "", "", "1", "", "5", "8", "", "", "6",
                    "8", "6", "5", "9", "7", "", "", "", "2",
                    "", "", "", "", "", "", "5", "7", "8",
                    "", "7", "9", "5", "4", "2", "", "6", "3",
                    "4", "", "", "8", "", "1", "7", "", "",
                    "", "", "6", "", "9", "", "2", "1", "",
                    "", "4", "3", "7", "8", "5", "6", "", "",
                    "", "", "", "6", "1", "", "", "9", "",
                    "", "", "", "2", "3", "", "4", "8", "",
                ]
            }
        },
        methods: {
            inputNumber(n) {
                if (this.isValid(n) && this.active !== null) {
                    this.gameBoard[this.active] = n;
                    this.active = null;
                }
            },
            getValidNumbers() {
                let numberList = [1, 2, 3, 4, 5, 6, 7, 8, 9]
                if (this.gameBoard[this.active] !== "") {
                    numberList.splice(numberList.indexOf(Number(this.gameBoard[this.active])), 1)
                }
                let numbersInRow = this.getNumbersInRow()
                for (let n of numbersInRow) {
                    let i = numberList.indexOf(n)
                    if (i > -1) {
                        numberList.splice(i, 1)
                    }
                }
                let numbersInColumn = this.getNumbersInColumn()
                for (let n of numbersInColumn) {
                    let i = numberList.indexOf(n)
                    if (i > -1) {
                        numberList.splice(i, 1)
                    }
                }
                let numbersInBlock = this.getNumbersInBlock()
                for (let n of numbersInBlock) {
                    let i = numberList.indexOf(n)
                    if (i > -1) {
                        numberList.splice(i, 1)
                    }
                }
                return numberList
            },
            isValid(x) {
                let validNumbers = this.getValidNumbers()
                for (let n of validNumbers) {
                    if (x === n) {
                        return true;
                    }
                }
                return false;
            },
            setActive(index) {
                this.active = index;
                this.getValidNumbers();
            },
            getNumbersInRow() {
                let row = Math.floor(this.active / 9);
                let numbersInRow = this.gameBoard.slice(9 * row, 9 + 9 * row)
                let clearedNumbersInRow = []
                for (let n of numbersInRow) {
                    if (n !== "") {
                        clearedNumbersInRow.push(Number(n))
                    }
                }
                return clearedNumbersInRow;
            },
            getNumbersInColumn() {
                let column = this.active % 9
                let numbersInColumn = []
                for (let row = 0; row < 9; row++) {
                    let n = this.gameBoard[row * 9 + column];
                    if (n !== "") numbersInColumn.push(Number(this.gameBoard[row * 9 + column]))
                }
                return numbersInColumn
            },
            getNumbersInBlock() {
                let row = Math.floor(this.active / 9);
                let column = this.active % 9
                let blockX = Math.floor(column / 3)
                let blockY = Math.floor(row / 3)
                let numbersInBlock = []
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + blockY * 9 * 3]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + 1 + blockY * 9 * 3]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + 2 + blockY * 9 * 3]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + blockY * 9 * 3 + 9]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + 1 + blockY * 9 * 3 + 9]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + 2 + blockY * 9 * 3 + 9]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + blockY * 9 * 3 + 18]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + 1 + blockY * 9 * 3 + 18]))
                numbersInBlock.push(Number(this.gameBoard[blockX * 3 + 2 + blockY * 9 * 3 + 18]))
                return numbersInBlock
            }
        },
        created() {
            // document.addEventListener("keyup", e => {
            //     if (!isNaN(parseFloat(e.key))) {
            //         this.inputNumber(e.key);
            //     }
            // })
        }

    }
</script>

<style scoped>
    * {
        color: black;
    }
    .title {
        font-size: 3rem;
        margin-bottom: 2rem;
    }
    .board {
        margin: auto;
        display: grid;
        grid-template-columns: 30px 30px 32px 30px 30px 32px 30px 30px 30px;
        grid-template-rows: 30px 30px 32px 30px 30px 32px 30px 30px 30px;
        grid-gap: 1px;
        padding: 1px;
        background-color: black;
        width: min-content;
    }
    .cell {
        cursor: default;
        text-align: center;
        font-size: 1.7rem;
        border: 0;
        background-color: white;
    }
    .active {
        background-color: lightgray;
    }

    .cell:nth-child(3n) {
        border-right: solid 2px black;
    }
    .cell:nth-child(9n) {
        border-right: 0;
    }
    .cell:nth-child(19),
    .cell:nth-child(20),
    .cell:nth-child(21),
    .cell:nth-child(22),
    .cell:nth-child(23),
    .cell:nth-child(24),
    .cell:nth-child(25),
    .cell:nth-child(26),
    .cell:nth-child(27),
    .cell:nth-child(46),
    .cell:nth-child(47),
    .cell:nth-child(48),
    .cell:nth-child(49),
    .cell:nth-child(50),
    .cell:nth-child(51),
    .cell:nth-child(52),
    .cell:nth-child(53),
    .cell:nth-child(54) {
        border-bottom: solid 2px black;
    }

    .numbers_pad {
        display: flex;
        background-color: white;
        grid-column: 1/10;
        margin: 2rem auto;
        cursor: default;
        width: 300px;
        justify-content: space-between;
    }
    .number {
        width: 30px;
        height: 30px;
        font-size: 1.7rem;
        border: dashed 1px gray;
    }
    .valid {

    }
    .invalid {
        color: lightgray;
    }
    .valid:hover {
        background-color: cornflowerblue;
    }
</style>