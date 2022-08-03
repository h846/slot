<template>
  <v-app>
    <v-main>
      <v-container
        justify="center"
        align-content="center"
        fill-height
        style="width: 600px"
        v-show="disp"
      >
        <Login @draw="startDrawing" />

        <v-row justify="space-around" align-content="center" no-gutters>
          <v-col cols="4" style="text-align: center">
            <div class="reel">
              <v-img :src="slotImg[0]" height="150" />
            </div>
          </v-col>

          <v-col cols="4" style="text-align: center">
            <div class="reel">
              <v-img :src="slotImg[1]" height="150" />
            </div>
          </v-col>

          <v-col cols="4" style="text-align: center">
            <div class="reel">
              <v-img :src="slotImg[2]" height="150" />
            </div>
          </v-col>
        </v-row>

        <v-row>
          <v-col style="text-align: center" v-if="prize == null">
            <v-btn
              elevation="2"
              x-large
              rounded
              block
              dark
              color="primary"
              @click="hitDetection"
              >S T O P</v-btn
            >
          </v-col>
        </v-row>
        <v-row justify="center" align-content="center">
          <v-col v-if="alert">
            <PrizeAlert v-bind="prize" :alert="alert" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';
import Login from './components/Login';
import PrizeAlert from './components/prizeAlert';

export default {
  name: 'App',

  components: {
    Login,
    PrizeAlert,
  },

  data: () => ({
    disp: false,
    intervalID: null,
    prizeInfo: null,
    alert: false,
    prize: null,
    userInfo: {
      id: '',
      name: '',
    },
    peopleImg: [
      {
        id: 1,
        name: 'Jeffrey',
        src: require('@/assets/img/1-jeffrey.png'),
      },
      {
        id: 2,
        name: 'Risa',
        src: require('@/assets/img/3-risa.png'),
      },
      {
        id: 3,
        name: 'Risa2',
        src: require('@/assets/img/3-risa.png'),
      },
      {
        id: 4,
        name: 'Masami',
        src: require('@/assets/img/2-masami.png'),
      },
      {
        id: 5,
        name: 'Masami2',
        src: require('@/assets/img/2-masami.png'),
      },
      {
        id: 6,
        name: 'Takashi',
        src: require('@/assets/img/4-takashi.png'),
      },
      {
        id: 7,
        name: 'Toru',
        src: require('@/assets/img/5-toru.png'),
      },
      {
        id: 8,
        name: 'Masakuni',
        src: require('@/assets/img/6-masakuni.png'),
      },
      {
        id: 9,
        name: 'Tomoko',
        src: require('@/assets/img/7-tomoko.png'),
      },
      {
        id: 10,
        name: 'Yoshitaka',
        src: require('@/assets/img/13-yoshitaka.png'),
      },
      {
        id: 11,
        name: 'Yoshitaka2',
        src: require('@/assets/img/13-yoshitaka.png'),
      },
      {
        id: 12,
        name: 'Yoko',
        src: require('@/assets/img/8-yoko.png'),
      },
      {
        id: 13,
        name: 'Tsutomu',
        src: require('@/assets/img/9-tsutomu.png'),
      },
      {
        id: 14,
        name: 'Yumiko',
        src: require('@/assets/img/10-yumiko.png'),
      },
      {
        id: 15,
        name: 'Ken',
        src: require('@/assets/img/11-ken.png'),
      },
      {
        id: 16,
        name: 'Naoko',
        src: require('@/assets/img/12-naoko.png'),
      },
      {
        id: 17,
        name: 'Operator',
        src: require('@/assets/img/15-operator.png'),
      },
      {
        id: 18,
        name: 'le-novel',
        src: require('@/assets/img/14-le-novel.png'),
      },
    ],
    slotImg: [],
  }),

  methods: {
    startDrawing(userInfo) {
      this.disp = true;
      this.userInfo = userInfo;
      //slot-dbから賞品情報を取得
      axios
        .get(
          'http://lejnet/API/accdb?db=API/src/cswk/slot-game.accdb&table=prize'
        )
        .then(res => {
          this.prizeInfo = res.data;
          this.prizeInfo.sort((a, b) => {
            if (a.ID < b.ID) return -1;
            if (a.ID > b.ID) return 1;
          });
        });
      //100ミリ秒ごとに画像を変える
      this.intervalID = setInterval(
        function() {
          this.$set(this.slotImg, 0, this.getRandPeopleImg());
          this.$set(this.slotImg, 1, this.getRandPeopleImg());
          this.$set(this.slotImg, 2, this.getRandPeopleImg());
        }.bind(this),
        100
      );
    },
    getRandPeopleImg() {
      return this.peopleImg[Math.floor(Math.random() * 17)].src;
    },
    hitDetection() {
      console.log(this.prizeInfo);
      //在庫チェック
      let isOutOfStock = this.prizeInfo.every(val => val.quantity < 1);
      if (isOutOfStock) {
        clearInterval(this.intervalID);
        alert('在庫切れ！HRかISに連絡してください。');
        return;
      }

      let random_num = Math.floor(Math.random() * 60); // 0~59
      console.log('num ' + random_num);
      let id = null;
      let prize = null;

      if (random_num < 16 && random_num >= 0) {
        prize = this.prizeInfo[random_num];
        id = random_num + 1;
      } else if (random_num >= 16 && random_num < 35) {
        prize = this.prizeInfo[16];
        id = 17;
      } else if (random_num >= 35) {
        prize = this.prizeInfo[17];
        id = 18;
      }
      console.log('id ' + id);
      console.log('PRIZE IS ' + JSON.stringify(prize));

      if (prize.quantity > 0) {
        clearInterval(this.intervalID);
        let img = this.peopleImg[parseInt(id) - 1].src;
        this.$set(this.slotImg, 0, img);
        this.$set(this.slotImg, 1, img);
        this.$set(this.slotImg, 2, img);

        //賞品在庫数更新
        let sql = 'UPDATE prize SET quantity = quantity - 1 WHERE ID = ' + id;
        axios
          .post('http://lejnet/API/accdb', {
            db: 'API/src/cswk/slot-game.accdb',
            sql: sql,
          })
          .then(res => {
            //当選者記録
            sql = `INSERT INTO winners (winner_name, winner_id, prize) VALUES('${this.userInfo.name}', '${this.userInfo.id}', '${prize.prize_type}')`;
            console.log(sql, res);
            axios
              .post('http://lejnet/API/accdb', {
                db: 'API/src/cswk/slot-game.accdb',
                sql: sql,
              })
              .then(res => {
                //当選商品情報を格納
                this.prize = this.prizeInfo[id - 1];
                //アラート表示
                this.alert = true;
                console.log(res);
              });
          });
      } else {
        this.hitDetection();
      }
    },
  },
};
</script>
<style lang="stylus" scoped>

.reel{
  padding:5px;
}
</style>
