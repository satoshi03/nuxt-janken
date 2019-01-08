<template>
  <section class="container">
    <div>
      <div v-if="!hand" class="janken-panel">
      <h2 class="subtitle">
        出す手を選びましょう
      </h2>
        <img @click="selectHand('ROCK')" src="~/assets/janken_gu.png">
        <img @click="selectHand('SCISSORS')" src="~/assets/janken_choki.png">
        <img @click="selectHand('PAPER')" src="~/assets/janken_pa.png">
      </div>
      <h2 class="subtitle" v-if="hand">{{ getResultMessage() }}</h2>

      <div v-if="hand && !enemyHand">
        <h2 class="subtitle">
          相手にリンクを送りましょう
        </h2>
      <div class="link">
        <input class="link-input" v-bind:value="getLink"></input>
        <a
          target="_blank"
          v-clipboard:copy="getLink"
          class="button--green">リンクをコピー</a>
      </div>

      </div>
    </div>
  </section>
</template>

<script>
import firebase from '~/plugins/firebase'

export default {
  validate ({ params }) {
    // 数値でなければならない
    return /^\d+$/.test(params.id)
  },
  data () {
    const ROCK = 'ROCK'
    const SCISSORS = 'SCISSORS'
    const PAPER = 'PAPER'
    const WIN = 'WIN'
    const DRAW = 'DRAW'
    const LOSE = 'LOSE'
    const JUDGE_DICT = {
      ROCK: {
        ROCK: DRAW,
        SCISSORS: WIN,
        PAPER: LOSE,
      },
      SCISSORS: {
        ROCK: LOSE,
        SCISSORS: DRAW,
        PAPER: WIN,
      },
      PAPER: {
        ROCK: WIN,
        SCISSORS: LOSE,
        PAPER: WIN,
      },
    }
    return {
      djugeDict: JUDGE_DICT,
      hand: "",
      enemyHand: "",
      enemyId: "",
      id: "",
      host: "",
      status: "ready",
      database: undefined,
      rooms: undefined,
    }
  },
  created: function () {
    this.database = firebase.database();
    this.id = this.$route.params.id
    this.userId = this.$route.query.userid
  
    if (!this.userId) {
      this.userId = Math.floor(Math.random() * Math.floor(1000));
    }

    this.rooms = this.database.ref('rooms');
    this.rooms.on('child_added', snapshot => {
      var newHand = snapshot.val()
      if (newHand.id == this.id && newHand.userid != this.userId) {
        this.enemyHand = newHand.hand
        this.enemyId = newHand.userid
      }
    })
  },
  mounted: function () {
    var protocol = location.protocol;
    var slashes = protocol.concat("//");
    this.host = slashes.concat(window.location.hostname);
  },
  computed: {
    getLink() {
      return this.host + "/rooms/" + this.$route.params.id
    }
  },
  methods: {
    selectHand(hand) {
      this.hand = hand
      this.setHand()
    },
    setHand() {
      if (this.id == "" || this.userId == "" || this.hand == "") {
        return
      }
      this.rooms.push({'id': this.id, 'userid': this.userId, 'hand': this.hand})
    },
    getHands() {
      if (this.id == "" || this.userId == "" || this.hand == "") {
        return []
      }
      console.log(this.rooms.value)
    },
    getResultMessage() {
      if (this.isWin()) {
        return "あなたの勝ちです！"
      } else if (this.isDraw()) {
        return "引き分けです！"
      } else if (this.isLose()){
        return "あなたの負けです"
      } else {
        return "相手の手を待っています..."
      }
    },
    isWin() {
      return this.judge() == "WIN"
    },
    isDraw() {
      return this.judge() == "DRAW"
    },
    isLose() {
      return this.judge() == "LOSE"
    },
    judge() {
      if (this.hand == "" && this.enemyHand == "") {
        return
      }
      return this.djugeDict[this.hand][this.enemyHand]
    }
  },
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.janken-panel img {
  height: 200px;
}

.link {
  padding-top: 15px;
}

.link-input {
  width: 400px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  height: 35px;
  font-size: 20px;
  margin-top: 2px;
  padding: 10px 10px;
}

/*
input {
  width: 80%;
  height: 40px;
  border-radius: 4px;
   color: #35495e;
}
*/
</style>

