<template>
	<div>
		<header class="header">
			<div class="back iconfont">&#xe624;</div>
			<div class="search"><a href="#" class="prompt">输入城市/景点/游玩主题</a></div>
			<div class="city">北京<i class="iconfont">&#xe600;</i></div>
		</header>

		<swiper :options="swiperOption">
			<swiper-slide v-for="item in swiperInfo" :key="item.id">
				<div class="swiper-img-con">
					<img class="swiper-img" :src="item.imgUrl">
				</div>
			</swiper-slide>
			<div class="swiper-pagination" slot="pagination"></div>
		</swiper>

    <swiper :options="lazyOption">
			<swiper-slide v-for="(pageInfo, index) in pages" :key="index">
        <div class="icon-wrapper">
          <div v-for="item in pageInfo" :key="item.id" class="icon-item">
            <div class="icon-img-con">
              <img class="icon-img" :src="item.imgUrl" />
              <a href="#" class="icon-word">{{item.title}}</a>
            </div>
          </div>
        </div>		
			</swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
		</swiper>

    <div class="message">
      <div class="location-msg">
        <div class="location-icon iconfont">&#xe611;</div>
        <div class="location">定位失败</div>
      </div>
      <div class="hot-spring-msg">
        <div class="hot-spring-icon iconfont">&#xe613;</div>
        <div class="hot-spring">5折泡温泉</div>
      </div>
    </div>

    <div class="blank"></div>
    <div class="activity-item" style="background:url(http://img1.qunarzz.com/piao/fusion/1710/a2/e395615b16fb1302.png) center center no-repeat;background-size:auto 100%; height:1rem;"></div>
   
    <div class="lazy-load" :options="lazyOption">
      <h2 class="hot-recommend">热销推荐</h2>
        <div class="hot-list" v-for="item in hotInfo" :key="item.id">
          <img :src="item.imgUrl" :alt="item.alt" class="hot-img" />
          <div class="hot-message">
            <h3 class="hot-areaName">{{item.name}}</h3>
            <p class="hot-state">{{item.state}}</p>
            <div><p class="hot-price">{{item.price}}</p><span class="hot-price-cheapest">起</span></div>
          </div>
        </div>
        <div class="goods-select"><a href="#" class="all-goods">查看所有产品</a></div>
    </div>
    <div class="where-to-go">
      <h2 class="hot-recommend">周末去哪儿</h2>
      <div class="where-togo" v-for="item in goInfo" :key="item.id">      
        <div class="site">
          <img :src="item.imgUrl" :alt="item.alt" class="go-img" />
          <p class="places">{{item.name}}</p>
          <p class="explain">{{item.state}}</p>
        </div>
      </div>
    </div>   
	</div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      swiperInfo: [],
      iconInfo: [],
      hotInfo: [],
      goInfo: [],
      swiperOption: {
        autoplay: 3000,
        pagination: '.swiper-pagination',
        loop: true
      },
      lazyOption: {
        autoplay: 3000,
        pagination: '.swiper-pagination',
        loop: true
      }
    }
  },

  computed: {
    pages () {
      const pages = []
      this.iconInfo.forEach((value, index) => {
        let page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(value)
      })
      return pages
    }
  },

  methods: {
    getIndexData () {
      this.$http.get('/static/index.json')
        .then(this.handleGetDataSucc.bind(this))
    },

    handleGetDataSucc (res) {
      const body = res.body
      if (body && body.data && body.data.swiper) {
        this.swiperInfo = body.data.swiper
        this.iconInfo = body.data.icons
        this.hotInfo = body.data.hotList
        this.goInfo = body.data.goInfo
      }
    }
  },

  created () {
    this.getIndexData()
  }
}
</script>
<style scoped>
  .header {
    display: flex;
    background: #05bad5;
    color: #fff;
  }
  .back {
    width: .64rem;
    line-height: 0.86rem;
    text-align: center;
  }
  .search {
    flex: 1;
    margin: .14rem .18rem;
    background: #fff;
    border-radius: .1rem;
  }
  .icon-shuaxin {
    color:#ccc;
    font-size:0.26rem;
    padding:0 0.15rem;
  }
  .prompt {
    color:#ccc;
    font-size:0.26rem;
    line-height: 0.6rem;
    padding-left: 0.2rem;
  }
  .city {
    margin-right: .1rem;
    line-height: .86rem;
    text-align: left;
    overflow: hidden;  
    white-space: nowrap; 
    text-overflow: clip;
  }
  .city-linknear {
    width: 1rem;
    line-height: .86rem;
    text-align: left;
    display: block;    
  }
  
  .swiper-img-con {
    overflow: hidden;  
    width: 100%;
    height: 0;
    padding-bottom: 31.25%;
  }
  .swiper-img {
    width: 100%;
  }
  .icon-item {
    width:23%;
    float: left;
    box-sizing: border-box;
    padding: 0.4rem 0.4rem 0 0.5rem;
    margin-bottom: .6rem;
  }
  .icon-img-con {
    width: 100%;
    height: 0;
    padding-bottom: 100%;
  }
  .icon-img {
    width: 100%;
    cursor: pointer;
  }
  .icon-word {
    display: block;
    font-size: .2rem;
    margin-top: .2rem;
    text-align: center;
    cursor: pointer;
  }
  .message {
    width: 100%;
    height: 0.75rem;
    border-top: 1px solid #f5f5f5;
  }
  .location-msg {
    width: 50%;
    height: 0.75rem;
    float: left;
  }
  .location-icon {
    line-height:  0.75rem;
    margin-left: 1rem;
    margin-right: .1rem;
    float: left;
  }
  .location {
    height: 0.75rem;
    text-align: center;
    line-height: .75rem;
    float: left;
  }
  .hot-spring-msg {
    width: 50%;
    height: 0.75rem;
    float: left;
  }
  .hot-spring-icon {
    line-height:  0.75rem;
    margin-left: 1rem;
    margin-right: .1rem;
    float: left;
  }
  .hot-spring {
    height: 0.75rem;
    text-align: center;
    line-height: .75rem;
    float: left;
  }
  .blank {
    width: 100%;
    height: .16rem;
    background: #f5f5f5; 
  }
  .hot-recommend {
    display: block;
    height: .6rem;
    line-height: .6rem;
    font-size: .22rem;
    color: #000;
    background: #f5f5f5;
    padding-left: .2rem;
  }
  .hot-list {
    width: 100%;
    height: 1.9rem;
    border-bottom: 1px solid #f5f5f5;
  }
  
  .hot-img {
    width: 1.4rem;
    height: 1.4rem;
    margin: .18rem;
    float: left;
  } 
  .hot-message {
    width: 5.5rem;
    height: 1.9rem;
    float: left;
  }
  .hot-areaName {
    font-size: .22rem;
    color: #000;
    padding-top: .2rem;
  }
  .hot-state {
    font-size: .2rem;
    color: #9e9eb6;
    padding-top: .2rem; 
  }
  .hot-price {
    font-size: .2rem;
    color: #ff8300;
    padding-top: .3rem; 
    float: left; 
  }
  .hot-price-cheapest {
    font-size: .18rem;
    color: #9e9eb6;
    padding-top: .31rem; 
    float: left; 
  }
  .goods-select {
    width:100%;
    height: .65rem;
  }
  .all-goods {
    display: block;
    height: .65rem;
    text-align: center;
    line-height: .65rem;
    font-size: .2rem;
    color: #00afc7;
  }
  .site {
    width: 100%;
  }
  .go-img {
    width:100%;
    height: 2.8rem;
  }
  .places {
    font-size: .22rem;
    color: #212121;
    padding: .18rem .18rem 0 .18rem;
  }
  .explain {
    font-size: .2rem;
    color: #616161;
    padding: 0.15rem .18rem .3rem .18rem;
  }
</style>